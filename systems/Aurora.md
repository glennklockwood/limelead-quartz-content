---
tags:
  - usa/doe/ascr
  - system
title: ALCF Aurora
---
Aurora is the [[Cray EX]] supercomputer operated by the Argonne Leadership Computing Facility (ALCF). It achieved a max [[HPL]] score of 1.012 EFLOPS using 87% of the system in June 2024[^2] after debuting six months earlier with 585.3 PFLOPS.

Although this system uses significant [[Cray EX]] technology including the cabinet and [[Slingshot]] network, Intel was the prime contractor and provided its own compute blades.
## System overview

Aurora has:[^1]

- 10,624 compute nodes
- 21,248 Intel Sapphire Rapids HBM CPUs (Intel Xeon CPU Max)
- 63,744 [[PVC|Ponte Vecchio]] GPUs (Intel Data Center GPU Max)
- 166 cabinets
## Node architecture

Although Aurora uses the [[Cray EX]] platform, the compute blades were manufactured by an Intel subcontractor, not Cray. Thus, they don't have a Cray model name, and you cannot buy [[PVC]] blades from Cray.

Each Intel node has:[^3]

- 2x Intel Xeon CPU Max 9470C (Sapphire Rapids HBM)
	- 52 cores per socket, 2 threads per core
	- 64 GB HBM per socket
	- 512 GB DDR5
- 6x Intel Data Center GPU Max 1550 ([[PVC|Ponte Vecchio]])
	- 128 Xe cores per socket
	- 128 GB HBM2e per socket
- 8x [[Slingshot|Slingshot-11]] NICs

These nodes are so large that there is only one node per blade, or 64 nodes per cabinet.

## Network architecture

Aurora uses [[Slingshot|Slingshot-11]] in a [[dragonfly]] topology with compute and storage integrated on the same fabric.  There are:[^4]

- 166x compute groups
- 8x storage groups containing DAOS servers
- 1x service group containing login nodes and other ancillary servers

The inter-group connectivity is:

- 2 links per compute-compute group
- 2 links per compute-service group
- 2 links per compute-storage group
- 8 links per storage-service group
- 24 links per storage-storage group

Each group has 32 switches, and each switch has:

- 16 ports down to endpoints (L0)
- 31 ports to other switches in the rack (L1)
- Up to 16 ports to other groups (L2)

The blocking factor within the compute subsystem is 1.55:

Blocking factor $= \frac{512 \text{ endpoints}}{165 \text{ other groups} \times 2 \text{ global links}} = 1.552$

## Storage subsystem

Aurora uses [[DAOS]] instead of a traditional parallel file system.  This storage system is composed of 1,024 Intel Coyote Pass servers, each containing:[^4]

- 2x Intel 5320 CPUs (Ice Lake)
- 512 GiB DDR4 DRAM (16x 32GB DIMMs)
- 8192 GiB Intel Optane 200 (16x 512 GB DIMMs)
- 244.8 TB Samsung PM1733 SSDs (16x 15.3 TB)
- 2x [[Slingshot|Slingshot-11]] NICs

It has a total of 230 PB of capacity using 16+2 erasure coding and over 31 TB/s of bandwidth.[^5]

[^1]: [Aurora_FactSheet_2024.pdf (anl.gov)](https://www.alcf.anl.gov/sites/default/files/2024-07/Aurora_FactSheet_2024.pdf)
[^2]: [ISC’24 recap (glennklockwood.com)](https://blog.glennklockwood.com/2024/05/isc24-recap.html#section22)
[^3]: [Hardware Overview - ALCF User Guides (anl.gov)](https://docs.alcf.anl.gov/aurora/hardware-overview/machine-overview/)
[^4]: [DUG22 - DAOS Community - Confluence (atlassian.net)](https://daosio.atlassian.net/wiki/spaces/DC/pages/11248861216/DUG22)
[^5]: [Aurora | Argonne Leadership Computing Facility (anl.gov)](https://www.alcf.anl.gov/aurora)