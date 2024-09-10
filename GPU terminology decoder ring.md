---
aliases:
  - SM
  - TPC
  - CU
  - XCD
  - warp
  - wavefront
  - GCD
---
Here's a decoder ring for all the proprietary terms for GPU components. Not all are direct equivalents due to architectural differences though.

| NVIDIA                        | AMD                           | Intel                        |
| ----------------------------- | ----------------------------- | ---------------------------- |
| SM                            | CU                            | Xe Vector Engine, fka EU[^3] |
| TPC                           | XCD, fka GCD,                 | Xe Core, fka subslice[^3]    |
| [[tensor cores\|Tensor core]] | [[tensor cores\|Matrix core]] | Xe Matrix Engine             |
| Thread                        | Work item/thread              | Thread                       |
| Block                         | Workgroup                     |                              |
| Warp                          | Wavefront                     | ?                            |
| Grid                          | Grid                          |                              |

The AMD and NVIDIA decoder ring is inspired by Frontier's User Guide.[^1]

[^1]: [Frontier User Guide — OLCF User Documentation (ornl.gov)](https://docs.olcf.ornl.gov/systems/frontier_user_guide.html#amd-vs-nvidia-terminology)
[^2]: [Hardware Overview - ALCF User Guides (anl.gov)](https://docs.alcf.anl.gov/aurora/hardware-overview/machine-overview/)
[^3]: [GPU Optimization Thread Mapping Occupancy (anl.gov)](https://www.alcf.anl.gov/sites/default/files/2023-11/GPU_Optimization_Thread_Mapping_Occupancy.pdf)