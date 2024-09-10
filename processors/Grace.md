---
title: NVIDIA Grace
tags:
  - CPU
---
The NVIDIA Grace CPU is an ARM processor they developed. Here are its specs:[^1]

- 72x Arm Neoverse V2 cores, each supporting 4x128b SVE2
- L1 cache: 64 KB (instructions) + 64 KB (data)
- L2 cache: 1 MiB/core
- L3 cache: 177 MiB
- 480 GiB LPDDR5X (usable)
	- 512 GiB (physical--see [[LPDDR5 RAS]])
	- 32 channels
	- up to 546 GB/s[^3]
- 3.55 TFLOPS FP64 peak
- 68x lanes of PCIe Gen5
	- 4x PCIe Gen5 x16
	- 2x PCIE Gen5 x2
	- 12x lanes can also use coherent NVLink
- 250 W TDP

Grace-Grace is the superchip designed by NVIDIA that pairs two Grace CPUs on a single node.

- A single Grace CPU has up to 536 GB/s bandwidth (MemSet); Triad is 507 GB/s.[^2]

[^1]: [NVIDIA Grace CPU Superchip Architecture In Depth | NVIDIA Technical Blog](https://developer.nvidia.com/blog/nvidia-grace-cpu-superchip-architecture-in-depth/)
[^2]: [Inside NVIDIA Grace CPU: NVIDIA Amps Up Superchip Engineering for HPC and AI | NVIDIA Technical Blog](https://developer.nvidia.com/blog/inside-nvidia-grace-cpu-nvidia-amps-up-superchip-engineering-for-hpc-and-ai/)
[^3]: [HC2022.NVIDIA Grace.JonathonEvans.v5.pdf (hotchips.org)](https://www.hc34.hotchips.org/assets/program/conference/day2/ADAS%20and%20Grace/HC2022.NVIDIA%20Grace.JonathonEvans.v5.pdf)