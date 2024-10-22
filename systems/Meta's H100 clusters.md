---
tags:
  - system
title: Meta's H100 clusters
---
[[Meta]] has two 24,576-GPU clusters which are their flagship systems.[^1] The Llama-3 paper calls them "Meta's production clusters" but they have no other catchier name.

- One has a 400G RoCEv2 backend using Arista 7800 switches
- One has a 400G NDR [[InfiniBand]] backend using NVIDIA Quantum-2 switches

## System architecture

Overall, each system has[^3]

- 24,576 NVIDIA [[H100]] GPUs
- 3,072 Grand Teton nodes
- 1,536 racks
- 8 pods

Each pod is a nonblocking fabric domain consisting of[^3]

- 3,072 NVIDIA [[H100]] GPUs
- 384 Grand Teton nodes
- 192 racks
- 7:1 blocking

Based on my estimate, each of these clusters should be able to get 957,700 PFLOPS with [[HPL]].
## Node architecture

Each node is built on Meta's Grand Teton platform which has:

- 1x HGX baseboard
	- 8x [[H100]] GPUs
	- Full NVLink interconnectivity
- 2x CPUs, each with
	- 1x 400G frontend NIC
- 4x PCIe Gen5 switches, each with
	- 1x connection into the HGX base board
	- 2x 400G backend NICs
	- 2x NVMe drives

Two nodes fit in a single rack, meaning each 24K cluster is 1,536 racks large.

## Fabric architecture

Their backend network is a multilevel fat tree. They use inconsistent terminology to describe the three layers:

- T0 leaf switches are "RTSW" or "ToR" switches. They are [[Minipack2]] devices.
- T1 aggregation switches are "CTSW" or "Cluster Switches."
- T2 spine switches are "ATSW" or "Aggregation switches."

T0-T1 links are *undersubscribed* by 2:1 to reduce congestion at this level,[^2] and the Llama-3 paper calls these nonblocking T1 domains "pods." This is quite unique as **they spent twice the money** to provision twice as much injection bandwidth than they could actually use just **to work around the congestion that arises from RoCEv2**'s poor handling of it at the T1 layer.

T1-T2 links are oversubscribed by 7:1.[^3] They claim their communication library was aware of this extreme taper and works around it during training.

[^1]: [Building Meta’s GenAI Infrastructure](https://engineering.fb.com/2024/03/12/data-center-engineering/building-metas-genai-infrastructure/)
[^2]: [RDMA over Ethernet for Distributed AI Training at Meta Scale](https://dl.acm.org/doi/10.1145/3651890.3672233) (2024)
[^3]: [The Llama-3 Herd of Models (arxiv.org)](https://arxiv.org/abs/2407.21783)