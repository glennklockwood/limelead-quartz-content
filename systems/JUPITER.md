---
tags:
  - system/europe/eurohpc
title: JSC JUPITER
---
JUPITER is the first European exascale supercomputer being installed for 2024 at Juelich. Technical details are here: [JUPITER Technical Overview (fz-juelich.de)](https://www.fz-juelich.de/en/ias/jsc/jupiter/tech).

## System overview

The system will be built on [[BullSequana XH3000]] and will have two partitions:

- Booster Module:
	- "roughly 6,000 compute nodes"
	- Uses [[GH200]] superchips
		- 4C:4G (four superchips per node)
		- 24,000 [[H100|H200]] GPUs
		- 4 NDR200 [[InfiniBand]] HCAs per node
	- "1 ExaFLOP/s (FP64, [[HPL]])" which seems feasible given Meta's H100 clusters are 24K H100s and estimated to be just a few FLOPS shy of 1 EF each.
- Cluster Module: [[SiPearl Rhea]] CPUs
	- "more than 1300 nodes"
	- "more than 5 PetaFLOP/s (FP64, [[HPL]])"
	- 1 NDR200 [[InfiniBand]] HCA per node

## Network architecture

NDR200 [[InfiniBand]] in [[dragonfly+]] will be used throughout:

- Each [[dragonfly+]] group is a nonblocking fat tree
- 25x GPU groups
- 2x CPU, storage, and management groups
- 867x switches
-  with "25400 end points"
	- At least 24,000 for GPU nodes
	- Leaving 1350 for CPU nodes?

## Storage subsystem

Storage: [[IBM ESS 3500]]

- 40 IBM ESS 3500 servers
- 29 PB raw, 21 PB formatted

## Facility

JUPITER is being installed using a modular data center. [FRZ Jülich has posted some great photos of what that looks like](https://www.fz-juelich.de/en/ias/jsc/news/news-items/news-flashes/exascale-supercomputer-jupiter-first-containers-set-up-at-jsc).