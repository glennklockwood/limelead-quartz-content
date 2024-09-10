---
tags:
  - system/japan
title: R-CCS Fugaku
aliases:
  - PRIMEHPC FX1000
  - FX1000
---
Fugaku is a big old supercomputer at RIKEN-RCCS in Kobe, Japan. It is built on Fujitsu A64FX processors with HBM2 and achieved an [[HPL]] score of [415.53 PFLOPS on the June 2020 Top500 list](https://top500.org/system/179807/).[^top500]

## System overview

Fugaku has:[^riken]

- 158,976 compute nodes
- 158,976 Fujitsu A64FX Arm CPUs
- 432 racks (it's huge)

396 racks are fully populated with 384 nodes, while 36 racks are half-populated with 192 nodes.[^riken] It's unclear if those half-racks have half as many shelves or depopulated BoBs.

The Fugaku platform (which doesn't have a brand name?) has:

- 2x nodes per "CPU Memory Unit" (CMU)
- 8x CMUs per "bunch of blades" (BoB)
- 3x BoBs per shelf
- 8x shelves per rack

Fujitsu sells a platform called PRIMEHPC FX1000 which appears to be identical to Fugaku, but Fujitsu differentiates PRIMEHPC FX1000 and Fugaku branding. Maybe this is the same relationship that [[Red Storm]] and [[Cray XT3]] had.

## Node architecture

Each node has:[^riken]

- 1x Fujitsu A64fx CPU
	- 48 Armv8.2-A SVE cores + 2 assistant cores
	- 2.0 GHz base, 2.2 GHz max
	- 3.072 TF FP64 base, 3.3792 TF FP64 max
- 32 GB HBM2, 1024 GB/s peak bandwidth
- 1x 10-port Tofu D router (28 Gbps x 2 lanes per port)
- 1x PCIe Gen3 x16

[^top500]: It debuted in at ISC 2020 but got a few more nodes and a higher score by November 2020.
[^riken]: [About Fugaku](https://www.r-ccs.riken.jp/en/fugaku/about/)