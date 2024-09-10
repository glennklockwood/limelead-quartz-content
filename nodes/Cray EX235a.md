---
tags:
  - node
---
Cray EX235a is the blade for [[Cray EX]] platforms that hosts an AMD Trento CPU and four AMD [[MI250X]] GPUs.

Each blade has two nodes, and each node has[^3]

- 1x AMD Trento CPU
- Up to 512 GB DDR4 DRAM
- Up to 2x NVMe SSDs
- 4x AMD [[MI250X]] GPUs
- Up to 4x [[Slingshot|Slingshot-11]] NICs

These nodes are unique for a few reasons:

- AMD Trento is a modified version of AMD Milan that has Infinity Fabric connections to the [[MI250X]] GPUs, allowing the GPUs' HBM to be cache coherent with CPU memory.
- The [[Slingshot|Slingshot-11]] PCIe NICs are attached to the GPUs' PCIe ports, not the CPU's.

[^3]: [Frontier System Architecture (ornl.gov)](https://www.olcf.ornl.gov/wp-content/uploads/2-15-23-Frontier-System-Architecture-public-v7.pdf)