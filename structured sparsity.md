---
title: Structured sparsity
aliases:
  - sparsity
---
Newer NVIDIA [[tensor cores]] and AMD [[tensor cores|matrix cores]] also include _Fine-Grained Structured Sparsity_ which is a technique for lowering the cost of [[LLM inferencing]]. It does nothing for training.

Using structured sparsity requires special handling of your model after it has been trained:[^3][^4]

1. Train a model like normal.
2. Use fine-grained structured pruning to drop out insignificant weights. This will reduce the number of parameters in the model by setting them to zero.
3. Fine-tune the model to update the remaining weights to compensate for all the weights that were just set to zero.

The concept is:

- Enforce a specific sparsity pattern into the model, and the model will adapt around it
- Primarily an inferencing accelerator because "the structure constraint does not impact the accuracy of the trained network for inferencing."
- Training with sparsity requires adding it early in the process, and "methodologies for acceleration without accuracy loss are an active research area."

It's called structured sparsity because you must zero out weights in a specific pattern. For example, "2:4 sparse matrix" allows two nonzero values in every four-entry vector:

![[NVIDIA A100 Structured Sparsity.png]]



[^3]: [NVIDIA A100 GPU Architecture Overview](https://images.nvidia.com/aem-dam/en-zz/Solutions/data-center/nvidia-ampere-architecture-whitepaper.pdf)
[^4]: [NVIDIA H100 Tensor Core GPU Architecture Overview](https://resources.nvidia.com/en-us-tensor-core?ncid=no-ncid)
[^7]: [Accelerating Inference with Sparsity Using the NVIDIA Ampere Architecture and NVIDIA TensorRT | NVIDIA Technical Blog](https://developer.nvidia.com/blog/accelerating-inference-with-sparsity-using-ampere-and-tensorrt/)