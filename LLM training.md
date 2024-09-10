---
tags:
  - workload
---
These are notes and references I've been accumulating to understand how AI workflows (both training and inferencing) work to help inform the architectural decisions that go into designing supercomputers specifically for AI.

## Workload partitioning

There are three ways in which **training** a model can be divided across GPU nodes:

1. Data parallelism
    - simplest way to train at scale (thousands of GPUs)
    - partition the training batch and give each GPU node its own subset of the training dataset (a minibatch)
    - each GPU node holds the entire model
    - communication happens after each epoch
    - scales very well since multiple copies of the model are training in parallel, but may increase the time to train a model (convergence time) since training data may be less randomized as a result of partitioning
2. Pipeline parallelism (aka layer parallelism)
    - break up model into layers, then distribute whole layers across GPU nodes
    - requires moderate rewriting the training code to include communication within each epoch
    - scales well for models with lots of big layers
3. Tensor parallelism (aka operator parallelism, tensor slicing)
    - break layers of a neural network up and distribute them across GPU nodes
    - requires significant rewriting the training code to include communicationwithin each epoch
    - does not scale well due to high communication requirements

These parallelization approaches can be used at the same time.  For example, training a large language model across multiple DGX nodes likely involves tensor parallelism within the DGX node (since it has NVLink which makes the communication fast), pipeline parallelism across 16 DGX nodes, and data parallelism to accelerate training by scaling to a thousand DGX nodes.

Implementing these levels of parallelism concurrently is complicated, and a set of frameworks and further refinements to them have popped up. [Huggingface has a page on parallelism][huggingface parallelism] that explains some of the more sophiciated combinations such as ZeRO.

Many papers that propose new parallelization schemes also describe these approaches in their introductions. For example, the [PyTorch FSDP paper][] explains these very well and introduces a more advanced approach, sharded data parallel.

[huggingface parallelism]: https://huggingface.co/docs/transformers/v4.17.0/en/parallelism
[PyTorch FSDP paper]: https://arxiv.org/abs/2304.11277

## Communication

Distributed training is very communications intensive, but different forms of parallelism trigger different communications patterns:[^railonly]

- Data parallelism uses AllReduce
- Pipeline parallelism uses point-to-point send/receive
- Tensor parallelism uses AllGather and ReduceScatter
- Sharded-data parallelism uses AllGather and ReduceScatter

There are many techniques that implement these collectives using primitives that reduce the overall communication required; for example, AllReduce never actually involves every processor talking to every other processor; instead, ring- and tree-based reduction algorithms are employed.

### Data parallelism

When performing data-parallel training, each concurrent instance of the model uses a different subset of inputs (the _batch_ is partitioned into _minibatches_ and each model instance gets a minibatch). Then,

1. During forward propagation, there is no communication since the model on each concurrent instance (and its weights) are identical. The only difference is in the input data (the minibatch).
2. During backpropagation, there is a nonblocking AllReduce that happens after each layer's gradients have been calculated.
3. There is a barrier at the end of backpropagation to ensure that all gradients have been added up appropriately.
4. After the backpropagation has completed, the optimizer is applied.  In the simplest case (like plain old stochastic gradient descent), the sum of all gradients are then used to update weights on each instance. Since the weights on each model instance were identical at step 1 and AllReduce ensures our gradients are all identical, there is no communication needed to update the weights.

I found an article by [Simon Boehm on Data-Parallel Distributed Training of Deep Learning Models][boehm2022] really helpful in understanding how this works.

Running the optimizer (step 4 above) can be a can of worms though, since many optimizers (like Adam) are stateful. They maintain quantities (like momentum) that persist across epochs to help them converge faster, and these quantities do need to be synchronized across all nodes. The communication pattern of these stateful optimizers can vary though.

[boehm2022]: https://siboehm.com/articles/22/data-parallel-training

There are ways to perform asynchronous data-parallel tranining where not all replicas of the model synchronize their weights after each pass, but extra consideration must be taken to ensure the model still converges.

### Tensor parallelism

Tensor parallelism is overwhelmingly bandwidth-dominant; when training LLMs using 3D parallelism (tensor-, pipeline-, and data-parallelism), over 75% of all bytes transferred between processors is for tensor parallelism, and this fraction increases with larger models.[^railonly]

## Memory

The [ZeRO-DP paper][] (2020) states that a trillion-parameter model using a stateful optimizer (like Adam) requires 16 TiB of GPU memory at 16-bit precision.  This implies around 16 bytes (128 bits) per parameter with 8&times; that for other quantities like optimizer states and gradients. This paper also enumerates what contributes to this 8&times; and breaks this down using Adam as an example. In brief, the 16 bytes (128 bits) per parameter is composed of the following:

- 16-bit (2-byte) weight
- 16-bit (2-byte) gradient
- 32-bit (4-byte) copy of the weight for the optimizer reduction
- 32-bit (4-byte) momentum (one part of the optimizer state)
- 32-bit (4-byte) variance (the other part of the optimizer state)

The [Frontier trillion-parameter training paper][frontier paper] (2023) states that each model parameter requires 24 bytes (192 bits). However their breakdown only adds up to 14 bytes per parameter:

- 16-bit (2-byte) weight
- 32-bit (4-byte) gradient
- 32-bit (4-byte) copy of the weight
- 32-bit (4-byte) momentum (the optimizer state)

Mixed precision is used to minimize numerical instabilities (things like floating point underflow and overflow) that can result from performing multiply-accumulate operations found throughout training. For example, [[tensor cores]] can take two 16-bit arrays, multiply them together using 32-bit precision, then add a 32-bit array to the result.

According to [Microsoft DeepSpeed introduction][] (2020), a 40 GB GPU can hold a model containing 1.2 billion parameters which corresponds to 32 bytes (256 bits) per parameter. This number probably includes what the [ZeRO-DP paper][] refers to as _residual memory consumption_ - things that don't strictly scale with the number of weights but otherwise consume practically usable memory.

A lot of research goes into reducing the memory footprint of models since a smaller footprint allows you to train a model on fewer GPUs.  For example, [checkpointing activations][] is a technique that allows you to trade GPU memory consumption for GPU computation; you can checkpoint activations and recompute using these checkpoints to fit more parameters into memory.

[Microsoft DeepSpeed introduction]: https://www.youtube.com/watch?v=wbG2ZEDPIyw
[checkpointing activations]: https://doi.org/10.48550/arXiv.1604.06174
[ZeRO-DP paper]: https://dx.doi.org/10.1109/SC41405.2020.00024
[frontier paper]: https://arxiv.org/abs/2312.12705v2

## Storage

### Performance

[Architectural Requirements for Deep Learning Workloads in HPC Environments][] by Ibrahim et al establishes a nice method for calculating how much storage bandwidth is required to keep a GPU fully utilized when training different models. They evaluate relatively small models that are most relevant to scientific research and demonstrate:

1. If you can establish how many flops are required to pass one sample through a model (forward and backwards) during training, you can use the average size of a sample to calculate a MiB per FLOP ratio for a model. This is equivalent to MiB/s per FLOPS and you can multiply it by the FLOPS capability of a GPU (or a whole system) to get an order-of-magnitude estimate of the bandwidth required per GPU to train a specific model.
2. They show that CosmoFlow is a computationally inexpensive model and requires 65 GB/s per petaFLOP/s. They found that, in practice, CosmoFlow can only utilize 35-50 TFLOP/s per NVIDIA V100 GPU, so training this model on a single V100 requires 2.275 - 3.250 GB/s, and **an 8-way V100 node would require 18.2 GB/s - 26 GB/s. By comparison, a typical NFS client cannot achieve more than 3 GB/s over TCP**, and even with nconnect, this only goes up to 10 GB/s.
3. By comparison, ResNet-50 being trained on ImageNet is the least I/O-intensive and only requires, at most, 573 MB/s per V100 or **3.9 GB/s per 8-way V100 node.**

[Architectural Requirements for Deep Learning Workloads in HPC Environments]: https://dx.doi.org/10.1109/PMBS54543.2021.00007
[ml-model-io-tool]: https://github.com/glennklockwood/limelead/blob/master/content/static/data-intensive/analysis/ml-model-io-requirements.js

[Exascale deep learning for climate analytics][] by Kurth et al directly calculated their required storage bandwidth for a modified "Tiramisu" network for climate dataset segmentation and classification at 189 MB/s per V100 GPU, or **1.14 GB/s for a 6-way Power9 GPU node,** or 1.16 TB/s for the full scale of the training job they ran. Their FLOP/s/sample was 4.188 on V100 GPUs, and they achieved 20.93 TFLOP/s/GPU during training.

### Capacity

See [[storage for LLM training]].

[Exascale deep learning for climate analytics]: https://dl.acm.org/doi/10.5555/3291656.3291724
[^railonly]: [[2307.12169] Rail-only: A Low-Cost High-Performance Network for Training LLMs with Trillion Parameters (arxiv.org)](https://arxiv.org/abs/2307.12169)