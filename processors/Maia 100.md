---
title: Microsoft Maia 100
aliases:
  - Maia
tags:
  - GPU
---
Maia (Microsoft Artificial Intelligence Accelerator, but stylized _Maia_) is Microsoft's first-generation AI accelerator. Key features:[^blog]

- One Maia 100 = 16 clusters = 64 tiles
	- Tensor unit implemented as 16xRx16 (not sure what this means; does this notation connect with that used to describe [[tensor cores|tensor core]] capabilities?)
	- L1 and L2 scratchpads
	- Supports low precision MX formats (4-bit, 6-bit, 9-bit, FP32, BF16)[^sth]
- 64 GB HBM2e
	- 4x HBM2e stacks
	- 1.8 TB/s bandwidth
- 12x 400 GbE ports per chip
	- 3x400G to three other Maia chips per node (9x400 total)
	- 3x400G to T0 switch layer (3x400G total)
	- Each Maia 100 connects to three different T0 switches ([[multi-plane]] with three planes)
- Ethernet used for both intra- and inter-node interconnect
	- 4800 Gbps AllGather and Scatter-Reduce bandwidth
	- 1200 Gbps Alltoall bandwidth (what does this mean?)
	- Uses a "gather-based approach" for distributed GEMM instead of AllReduce-based approach. Not sure what this means, but it was explained at HotChips.[^sth]
- 105 billion transistors on a 820 mm^2 reticle-limited die; TSMC N5[^verge]
- 500W, capable up to 700W

[^blog]: [Inside MAIA 100 (microsoft.com)](https://techcommunity.microsoft.com/t5/azure-infrastructure-blog/inside-maia-100-revolutionizing-ai-workloads-with-microsoft-s/ba-p/4229118)
[^verge]: [Microsoft is finally making custom chips — and they’re all about AI (verge.com)](https://www.theverge.com/2023/11/15/23960345/microsoft-cpu-gpu-ai-chips-azure-maia-cobalt-specifications-cloud-infrastructure)
[^sth]: [Microsoft MAIA 100 AI Accelerator for Azure (servethehome.com)](https://www.servethehome.com/microsoft-maia-100-ai-accelerator-for-azure/)