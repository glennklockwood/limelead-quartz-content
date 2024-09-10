---
tags:
  - "#system/usa/doe/ascr"
aliases:
  - OLCF-5
  - Orion
title: OLCF Frontier
---
Frontier is a [[Cray EX]] supercomputer composed of [[MI250X]] GPUs operated by the Oak Ridge Leadership Computing Facility. It achieved a max [[HPL]] score of 1.206 EFLOPS.

## System overview

Frontier has:

- 9,408 [[Cray EX235a]] compute nodes[^2]
- 9,408 AMD Trento CPUs (Epyc 7A53)
- 37,632 AMD [[MI250X]] GPUs
- 74 cabinets

Frontier uses a [[Slingshot]] fabric in a [[dragonfly]] topology.

Interestingly, a fully populated 74-cabinet [[Cray EX|Cray EX4000]] with [[Cray EX235a]] nodes should have a total of 9,472 nodes. 64 nodes, or a half rack, are missing.
## Node architecture

Each [[Cray EX235a]] node has:

- 1x AMD Trento CPU
	- 64 [[Zen|Zen 3]] cores
	- 2x threads/core
	- 2.0 GHz base, 3.7 GHz max
- 512 GB DDR4 DRAM, 205 GB/s peak bandwidth
- 2x 2 TB NVMe SSDs
- 4x AMD [[MI250X]] GPUs
	- 128 GB HBM2e (64 GB per [[GPU terminology decoder ring|GCD]])
	- 3.2 TB/s peak HBM bandwidth
- 4x [[Slingshot|Slingshot-11]] NICs

Here is a diagram stolen from the [Frontier User Guide](https://docs.olcf.ornl.gov/systems/frontier_user_guide.html#frontier-compute-nodes):

![[Frontier_Node_Diagram.jpg]]

Each physical GPU appears as two separate GPUs to the Linux environment--one per [[GPU terminology decoder ring|GCD]]. Because of this, ORNL says each node has eight GPUs, while AMD says each node has four.
## Network architecture

Each [[Cray EX|Cray EX4000]] cabinet is a [[dragonfly]] group with:[^1]

- 128x compute nodes
- 32x 64-port 200G [[Slingshot]] switches
- 4 NICs per node

Each switch has:[^3]

- Up to 16 ports down to endpoints (L0)
- 31 ports to other switches in the rack (L1)
- Up to 16 ports to other groups (L2)

So per group, there are[^3]
- 512 ports down to endpoints
- 992 ports connecting intra-group switches
- Up to 512 ports connecting inter-group switches

Each group has 4 links to every other group, resulting in a blocking factor of 1.753:[^3] 

- Each group has injection bandwidth of 512x200G from endpoints, or 102.4 Tb/s
- Each group has ejection bandwidth of 4x200G to 73 other groups, or 58.4 Tb/s

Blocking factor $= \frac{512 \text{ endpoints}}{73 \text{ other groups} \times 4 \text{ global links}} = 1.753$

## Storage subsystem

Frontier's file system is named Orion, and it is a massive, multi-tier [[Lustre]] file system built from [[Cray ClusterStor E1000]]. It is directly integrated on to Frontier's [[dragonfly]] network and contains:[^cug-leverman]

- 2x ClusterStor management nodes
- 2x MGS nodes
- 40x MDS nodes
- 450x OSS nodes
- 160x LNET router nodes (for connectivity to external systems)
- 12x utility nodes
- 80x [[Slingshot]] switches
- 35x management switches

Each _scalable storage unit (SSU)_ is:

- 1x Cray E1000
	- 24x 3.84 TB NVMe drives
	- 2x nodes, each with
		- 1x AMD Rome CPU (32-core)
		- 256 GB DDR4
		- 2x [[Slingshot|Slingshot-11]] NICs in PCIe form factor
- 2x 4U106 SAS enclosures (212x 18 TB HDDs)

These SSUs are composed into an _SSC_ (scalable storage cluster?). One SSC is a [[dragonfly]] group and has:

- 45x SSUs
- 32x LNet router nodes
- 8x MDS nodes in four E1000s. Each node has
	- 1x AMD Rome CPU (32-core)
	- 256 GB DDR4
	- 2x [[Slingshot|Slingshot-11]] NICs
	- Half of the E1000's 24x 30 TB NVMe drives
- 5x management switches
- 16x [[Slingshot]] switches

The full file system has five such SSCs (five [[dragonfly]] groups) for a total of:

- 5,400x 3.84 TB NVMes in OSTs
	- 20,736 TB raw
	- 11,400 TB formatted
- 47,700x 18 TB HDDs in OSTs
	- 858,600 TB raw
	- 667,600 TB formatted
- 480x 30.72 TB NVMes in MDTs
	- 14,745.6 TB raw
	- 9,700 TB formatted for Data-on-MDT (DoM)

Though this is a ClusterStor file system, it runs ZFS and dRAID instead of ldiskfs and Cray's proprietary GridRAID.

[^1]: [Frontier Exascale Architecture (anl.gov)](https://extremecomputingtraining.anl.gov/wp-content/uploads/sites/96/2024/08/ATPESC-2024-Track-2-Talk-2-Holmen-Frontier-Exascale-Architecture-AMD-MI250x-and-HPE-Slingshot.pdf)
[^2]: [Frontier: Exploring Exascale | Proceedings of the International Conference for High Performance Computing, Networking, Storage and Analysis (doi.org)](https://doi.org/10.1145/3581784.3607089)
[^3]: [Frontier System Architecture (ornl.gov)](https://www.olcf.ornl.gov/wp-content/uploads/2-15-23-Frontier-System-Architecture-public-v7.pdf)
[^cug-leverman]: [Deploying a Parallel File System for the World’s First Exascale Supercomputer (cug.org)](https://cug.org/proceedings/cug2023_proceedings/includes/files/pap122s2-file2.pdf)