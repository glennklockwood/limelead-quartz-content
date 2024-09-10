---
title: LLNL El Capitan
tags:
  - "#system/usa/doe/nnsa"
---
El Capitan is LLNL's upcoming [[Cray EX255a]]/[[MI300A]] supercomputer.

It's supposedly going to be > 2 exaflops. Given [[MI300A]] has a peak FP64 performance of 61.3 TF and an [[HPL]] performance of 38.4 TF FP64, this means the machine will have between 32,626 and 52,083 APUs.

The [[Cray EX255a]] has four APUs per node which would place the system at between

- 8,157 nodes (let's round up to 8192), assuming that 2.0 EF is an $R_\text{max}$ figure
- 13,021 nodes, assuming that 2.0 EF is $R_\text{peak}$

Given each [[Cray EX]] rack accepts 64 blades, El Capitan will have between 64 and 102 cabinets.