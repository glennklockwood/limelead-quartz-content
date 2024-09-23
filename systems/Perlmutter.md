---
tags:
  - usa/doe/ascr
  - system
aliases:
  - NERSC-9
title: NERSC Perlmutter
---
Perlmutter is a heterogeneous [[Cray EX]] supercomputer composed of NVIDIA [[A100]] GPUs and AMD Milan CPUs operated by the National Energy Research Scientific Computing Center (NERSC).

## System overview

Perlmutter has:

- 1,536 GPU nodes with 40 GB [[A100]] GPUs
- 256 GPU nodes with 80 GB [[A100]] GPUs
- 3,072 CPU nodes with dual-socket Milan CPUs

Perlmutter uses a [[Slingshot]] fabric in a [[dragonfly]] topology.

## Node architecture

Each [[Cray EX235n]] node has:

- 1x AMD Milan 7763 CPU
- 256 GB DDR4 DRAM
- 4x NVIDIA [[A100]] GPUs
- No node-local storage
- 4x [[Slingshot|Slingshot-11]] NICs

Each [[Cray EX425]] node has:

- 2x AMD Milan 7763 CPUs
- 512 GB DDR4 DRAM
- No GPUs
- No node-local storage
- 1x [[Slingshot|Slingshot-11]] NICs
# Network architecture

Each [[Cray EX|Cray EX4000]] cabinet is a [[dragonfly]] group with:[^1]

- 128x GPU nodes _or_ 256 CPU nodes
- 32x 64-port 200G [[Slingshot]] switches
- 4 NICs per node (GPU nodes) _or_ 1 NIC per node (CPU nodes)

Perlmutter has multiple types of dragonfly groups:

- 24x[^1] (or 14x? 28x?[^2]) GPU groups containing GPU nodes
- 12x CPU groups containing CPU-only nodes
- 4x storage groups containing [[Lustre]] servers
- 1x service group containing login nodes and other ancillary servers

Within each group, each switch has:[^3]

- Up to 16 ports down to endpoints (L0)
- 31 ports to other switches in the rack (L1)
- Up to 16 ports to other groups (L2)

So per group, there are[^3]

- 512 ports down to endpoints
- 992 ports connecting intra-group switches
- Up to 512 ports connecting inter-group switches



[^1]: [Perlmutter Architecture (nersc.gov)](https://docs.nersc.gov/systems/perlmutter/architecture/)
[^2]: If there are 128 GPU nodes per cabinet and Perlmutter has 1,792 GPU nodes, there should be 14 cabinets. The Perlmutter User Guide is inconsistent in that it says "GPU cabinets contain one Dragonfly group per cabinet...making a total of 24 groups." I know that Perlmutter's original configuration had two dragonfly groups per GPU cabinet, so maybe there is residual confusion here.
[^3]: See [[Frontier#Network architecture]]; [[Cray EX]] has the same L0/L1 configuration.