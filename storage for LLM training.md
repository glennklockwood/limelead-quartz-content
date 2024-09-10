---
title: Storage for LLM training
tags:
  - storage
---
[[LLM training]] uses storage for two primary purposes:

1. **Training data**, which is used to update model weights and converge the model
2. **Checkpointing**, where the model weights themselves are saved from GPU memory

## Requirements

### Training data

Training transformers is very compute-intensive by definition, meaning the data intensity of training them is comparatively small. Tokenized data, which is the binary data that models directly consume while training, is $\mathcal{O}(4)$ bytes per token of English language text, or a few terabytes per trillion tokens.

Leading models are training using tens of trillions of tokens (see [[LLM training datasets]]) which amounts to dozens of terabytes of training data for text-based models.

### Checkpointing

The capacity required for LLM checkpointing scale with the size of the model, not the size of the training cluster.[^kartik] Model checkpoint sizes can be approximated by assuming 16 bytes per parameter (see [[LLM training#Memory|LLM training memory requirements]]).

Similarly, the performance required for checkpointing scales with model size instead of cluster size, and it can be further reduced by using asynchronous, multilevel checkpointing.

In addition, asynchronous checkpointing is common at scale. In this scheme, the GPU is only blocked when copying checkpoint data from GPU memory to host CPU memory. The GPU then proceeds with computing while the CPU asynchronously flushes the checkpoint data to nonvolatile storage. ByteDance's MegaScale does this.[^megascale] 

There may be multiple levels of nonvolatile storage that are used at different intervals to further reduce bandwidth requirements and increase durability of the checkpoints. [Microsoft's Nebula framework](https://learn.microsoft.com/en-us/azure/machine-learning/reference-checkpoint-performance-for-large-models) does this by

1. Synchronously checkpointing to CPU memory
2. Asynchronously copying that to an adjacent node's local SSD to protect against a single-node failure
3. Asynchronously copying that to object storage to protect against multi-node failures, allow rollback, and store checkpoints long-term

## In practice

ByteDance uses HDFS for checkpointing.[^megascale]

[^kartik]: [A Checkpoint on Checkpoints in Large Language Models (vastdata.com)](https://www.vastdata.com/blog/a-checkpoint-on-checkpoints-in-llms)
[^megascale]: [[2402.15627] MegaScale: Scaling Large Language Model Training to More Than 10,000 GPUs (arxiv.org)](https://arxiv.org/abs/2402.15627)