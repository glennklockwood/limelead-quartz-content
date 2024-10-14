---
tags:
  - system/europe/eurohpc
title: CINECA Leonardo
---
Leonardo is a 240 PF [[BullSequana XH2000]] "pre-exascale" supercomputer deployed at CINECA in Bologna, Italy. It is a mix of CPU-only and [[A100]] GPU nodes and achieved an [[HPL]] score of 174.70 PF.[^top500]

It has 13,824 NVIDIA [[A100]] GPUs with a nonstandard 64 GB HBM2 each.

## Compute subsystem

It has two partitions:

- Booster Module: 3,456 compute nodes
	- BullSequana X2135 blades
	- 1x 32-core Xeon 8358 CPU
	- 512 GB DDR4-3200 DRAM
	- 4x NVIDIA [[custom A100]] with 64 GB HBM2
	- 4x100G HDR100 [[InfiniBand]] (ConnectX-6 dual-port HCAs)
- Data Centric Module: 1,536 compute nodes
	- BullSequana X2610 blades
	- 2x 56-core Intel Sapphire Rapids (what SKU?)
	- 1x 100G HDR100 [[InfiniBand]] (one NIC, one port)
	- 8 TB of NVMe (how many SSDs?)

Of note, Leonardo uses a [[custom A100]] GPUs with 64 GB HBM2.

## Interconnect

The fabric is HDR [[InfiniBand]] in a [[dragonfly+]]. Each GPU group is a two-level nonblocking fat tree with:[^14090]

- 36x 40-port 200G switches (or 80x 100G ports)
	- 18x leaf switches
		- 40x 100G ports down to 10 nodes
		- 18x 200G ports up to spines
	- 18x spine switches
		- 18x 200G ports down to leaves
		- 22x 200G ports up to other groups
- 180 nodes or 720 GPUs

I haven't done the math to figure out what the CPU-only groups look like.

## Storage subsystem

Wikipedia says it uses

- DDN Exascaler ES400NVX2 (all-flash [[Lustre]])
- DDN Exascaler SFA799X (HDD [[Lustre]])

## Location

It is sited at Tecnopolo Bologna in a data center that used to be a tobacco factory.[^1] This is the same place ECMWF sites its supercomputers.

## LISA Upgrade

In September 2024, the Italian government and EuroHPC JU issued an RFP for the "Leonardo Improved Supercomputing Architecture" (LISA) for delivery in April 2025 and acceptance in August 2025.[^27839d65-0b26-4052-8bee-8e800487fa35] It is a very prescriptive expansion that effectively asks for either 8-way [[H100]] HGX or [[MI300X]] OAM nodes:

> [!quote]
> the LISA partition must include at least 165 nodes, have memory coherence for the GPUs inside the nodes, and must have eight GPUs in the node[^27839d65-0b26-4052-8bee-8e800487fa35]

> [!quote]
> Each GPU has to have at least 80 GB of HBM memory. The host node has to have two X86 CPUs and must have at least 1 TB of main memory and all memory slots occupied to saturate the available [[DRAM architecture|memory channels]] from the CPUs.[^27839d65-0b26-4052-8bee-8e800487fa35]

[^1]: [OMA · Bologna Tecnopolo · Divisare](https://divisare.com/projects/204584-oma-bologna-tecnopolo)
[^14090]: [[2408.14090] Exploring GPU-to-GPU Communication: Insights into Supercomputer Interconnects (arxiv.org)](https://doi.org/10.48550/arXiv.2408.14090)
[^27839d65-0b26-4052-8bee-8e800487fa35]: [A GPU Upgrade For “Leonardo” Supercomputer But Not A Budget Upgrade](https://www.nextplatform.com/2024/09/23/a-gpu-upgrade-for-leonardo-supercomputer-but-not-a-budget-upgrade/)