---
title: Azure ND A100 v4
aliases:
  - Standard_ND96amsr_A100_v4
  - Standard_ND96asr_A100_v4
  - NDm A100 v4
tags:
  - node
---
[ND A100 v4](https://learn.microsoft.com/en-us/azure/virtual-machines/ndm-a100-v4-series) is Azure's eight-way [[A100]] VM type and compute node. It comes in two varieties:

| Name        | Instance                  | Config                |
| ----------- | ------------------------- | --------------------- |
| ND A100 v4  | Standard_ND96asr_A100_v4  | Has 40 GB HBM per GPU |
| NDm A100 v4 | Standard_ND96amsr_A100_v4 | Has 80 GB HBM per GPU |

The physical node looks like this:[^1]

![[ND A100 v4.jpg]]

[^1]: Microsoft booth at SC22. See [Microsoft Azure at SC22 and The Enormous AMD EPYC Genoa Heatsink on HBv4 (servethehome.com)](https://www.servethehome.com/microsoft-azure-at-sc22-and-the-enormous-amd-epyc-genoa-heatsink-on-hbv4/) for more.