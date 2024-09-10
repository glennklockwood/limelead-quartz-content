---
tags:
  - node
title: Azure HBv4
---

Azure HBv4 is Microsoft's CPU-only node for HPC.

Each node has:[^1]

- 176 cores of AMD EPYC 9V33X (Genoa-X)
	- 2.4 GHz base, 3.7 GHz boost
	- Implemented using two sockets of 96-core CPUs
- 768 GiB DDR5
- 2x 1,800 TB NVMe
- 1x 480 GB SSD
- 1x 400G ConnectX-7 NDR [[InfiniBand]]
- 80G Ethernet (2nd generation [[Azure SmartNIC]]

[^1]: [HBv4-series VM overview, architecture, topology - Azure Virtual Machines - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-machines/hbv4-series-overview)