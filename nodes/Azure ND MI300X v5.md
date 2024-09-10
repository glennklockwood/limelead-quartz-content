---
title: Azure ND MI300X v5
tags:
  - node
aliases:
  - ND MI300X v5
  - Standard_ND96isr_MI300X_v5
---
ND MI300X v5 is Microsoft's eight-way [[MI300X]] VM and node. It is built on the same node platform as the [[Azure ND H100 v5|ND H100 v5]] server, but with an 8-way OAM baseboard in place of the HGX board.

Each VM has:[^2]

- 96 Intel Xeon (Sapphire Rapids) cores (physically two 56-core sockets)
- 1,850 GiB DDR5
- 1,000 GiB of local storage
- 1x 80G Ethernet NIC (physically 100G [[Azure SmartNIC]])
- 8x 400G ConnectX-7 NDR [[InfiniBand]] HCAs
- 8x AMD Instinct [[MI300X]] GPUs

This is what it looks like:[^1]

![[Azure ND MI300X v5.jpeg]]

[^1]: I took this photo at SC'23.
[^2]: [ND-MI300X-v5 size series - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/gpu-accelerated/ndmi300xv5-series?tabs=sizebasic)