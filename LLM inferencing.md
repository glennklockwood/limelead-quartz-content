---
tags:
  - workload
---

# LLM inferencing

Inferencing is different from training in that models are often _quantized_ to reduced precisions to save on the memory and computational requirements to process requests.

From the [DeepSpeed-FastGen paper](https://arxiv.org/abs/2401.08671):

- Prompt processing:
  - input is user-provided text (the prompt)
  - output is a key-value cache for attention
  - compute-bound and scales with the input length
- Token generation:
  - adds a token to the KV cache, then generates a new token
  - [[memory bandwidth]]-bound and shows approximately O(1) scaling

Until I have time to summarize it, I recommend reading [Efficient Memory Management for Large Language Model Serving with PagedAttention](https://arxiv.org/abs/2309.06180) by Kwon et al to understand how GPU memory is consumed during inferencing. This paper explains the role of key-value caches to store parts of the attention mechanism.