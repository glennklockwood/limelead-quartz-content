---
tags:
  - node
---
Cray EX255a is the blade for [[Cray EX]] platforms that hosts [[GH200]].

Each blade has two nodes, and each node has

- 2x [[GH200]] superchips
	- 1 Grace CPU (72 cores, Arm Neoverse V2)
	- 1 Hopper H100 GPU
- 128 GB LPDDR5X DRAM
	- Although Grace supports "up to" 480 GB [[LPDDR5 RAS|with ECC]], the HPE spec sheet only offers a 128 GB option.[^datasheet]
	- Similarly, [[Alps]]'s nodes only have 128 GB (or 120 GB[^14090] with [[ECC]]).
	- It appears that this node has significantly less than the maximum LPDDR5 than Grace supports, probably reflecting an optimal cost/performance ratio for HPC applications.
- 4x [[Slingshot|Slingshot-11]] NICs[^sth]

I don't think this blade can accept NVMe SSDs like some other Cray EX blades.

Logically, each [[GH200]] appears as its own NUMA domain within a node.[^14090]

Here's a photo I took of the blade:[^1]

![[Cray EX254n photo.jpeg]]

[^1]: HPE's booth at ISC'24.
[^datasheet]: [NVIDIA Accelerators for HPE (hpe.com)](https://www.hpe.com/psnow/doc/PSN1014779860WWEN.pdf)
[^sth]: [8x NVIDIA Grace Hopper Superchips in a Blade HPE Cray EX254n at GTC 2024 (servethehome.com)](https://www.servethehome.com/8x-nvidia-grace-hopper-superchips-arm-in-a-blade-hpe-cray-ex254n-at-gtc-2024/)
[^14090]: [[2408.14090] Exploring GPU-to-GPU Communication: Insights into Supercomputer Interconnects (arxiv.org)](https://doi.org/10.48550/arXiv.2408.14090)