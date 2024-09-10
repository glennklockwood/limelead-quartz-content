---
title: CSCS Alps
tags:
  - "#system/europe"
---
Alps is a Frankenstein [[Cray EX]] supercomputer at CSCS composed of five different types of blades spread over two physical locations: CSCS in Lugano and EPFL in Lausanne.

## System overview

Its most notable partition are its [[GH200]] blades, but it has the following blades:[^official]

| Num nodes | Node type       | CPUs                       | GPUs                                   |
| --------- | --------------- | -------------------------- | -------------------------------------- |
| 2,688     | [[Cray EX254n]] | 4x [[Grace\|Nvidia Grace]] | 4x [[H100\|Nvidia H100]]               |
| 1,024     | [[Cray EX425]]  | 2x AMD Rome                | -                                      |
| 144       | [[Cray EX235n]] | 1x AMD Milan               | 4x [[custom A100\|NVIDIA custom A100]] |
| 128       | [[Cray EX255a]] | -                          | 4x [[MI300A\|AMD MI300A]]              |
| 24        | [[Cray EX235a]] | 1x AMD Trento              | 4x [[MI250X\|AMD MI250X]]              |

Also of note:

- It uses [[custom A100]] GPUs are also wonky in that some of them have 96 GB of HBM2e.
- Its [[GH200]] nodes have only 128 GB of LPDDR5X RAM, far less than the 512 GB supported by [[Grace]]. 

## Performance

As of August 2024, Alps is still in preproduction so its performance has not stabilized. However there are a few papers that talk about it already:

- [Understanding Data Movement in Tightly Coupled Heterogeneous Systems: A Case Study with the Grace Hopper Superchip](https://arxiv.org/abs/2408.11556) contains more architectural details of the system.
- [Exploring GPU-to-GPU Communication: Insights into Supercomputer Interconnects (arxiv.org)](https://arxiv.org/abs/2408.14090) discusses the performance of collectives on Alps.[^14090]

[^official]: [Alps | CSCS](https://www.cscs.ch/computers/alps)
[^14090]: [[2408.14090] Exploring GPU-to-GPU Communication: Insights into Supercomputer Interconnects (arxiv.org)](https://doi.org/10.48550/arXiv.2408.14090)