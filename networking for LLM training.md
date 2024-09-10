---
title: Networking for LLM training
tags:
  - network
---
Although [[LLM training]] makes heavy use of global collectives (see [[LLM training#Communication|LLM training communication]]), communication between processors is highly localized and concentrated:[^railonly]

- 99% of processor pairs never communicate with each other when using 3D (data, pipeline, and tensor) parallelism.
- Tensor parallelism accounts for 75% or more of the traffic, and that communication is highly localized to less than 0.04% of processor pairs at scale.
- When processor pairs do communicate, they transfer large amounts of data and are bandwidth-bound, not latency-bound.

This all means that networks for [[LLM training at scale]] do not need to be densely interconnected, and there are many topologies that can effectively support distributed training at a lower cost than a fully nonblocking fat tree topology.

## In practice

ByteDance uses a three-level, rail-optimized fat tree for their 10,000 GPU cluster.[^megascale]

Alibaba uses a two-level, dual-plane fat tree for their 15,000(?) GPU cluster.[^hpn]

[[Eagle]] uses a rail-optimized fat tree of some form. Its exact topology is not public.

## In principle

HammingMesh is a topology designed specifically to match the needs of distributed model training.[^hammingmesh]

Rail-only is a concept where the planes in a [[multi-plane]] fat tree are completely independent.[^railonly]

[^railonly]: [[2307.12169] Rail-only: A Low-Cost High-Performance Network for Training LLMs with Trillion Parameters (arxiv.org)](https://arxiv.org/abs/2307.12169)
[^megascale]: [[2402.15627] MegaScale: Scaling Large Language Model Training to More Than 10,000 GPUs (arxiv.org)](https://arxiv.org/abs/2402.15627)
[^hpn]: [Alibaba HPN: A Data Center Network for Large Language Model Training (acm.org)](https://dl.acm.org/doi/10.1145/3651890.3672265)
[^hammingmesh]: [HammingMesh (acm.org)](https://dl.acm.org/doi/abs/10.5555/3571885.3571899)