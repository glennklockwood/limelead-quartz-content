---
title: Azure ND H100 v5
tags:
  - node
aliases:
  - ND H100 v5
  - Standard_ND96isr_MI300X_v5
---
ND H100 v5 is [[Microsoft]]’s eight-way [[H100]] VM and node. It resembles the NVIDIA DGX [[H100]] reference architecture and uses a similar Intel Sapphire Rapids host platform.

Each VM has[^2]

- 96 Intel Xeon (Sapphire Rapids) cores (physically two 56-core sockets)
- 1,900 GiB DDR5
- 28,000 GiB of local storage (physically 8x NVMe drives)
- 1x 80G Ethernet NIC (physically 100G [[Azure SmartNIC]])
- 8x 400G ConnectX-7 NDR [[InfiniBand]] HCAs
- 8x Nvidia [[H100]] GPUs

This is what it looks like from the top:[^1]

![[Azure ND H100 v5 top.jpeg]]

Of note,

- At the far end are the intake fans
- The HGX baseboard is just behind them with eight tall heatsinks shown. Underneath the far grab bar are the NVLink Switches under equally tall heat sinks.
- At the near end is the Host Interface Board which connects the CPU board (not visible), the HGX baseboard, and the NICs and SSDs.

This is the rear end of it:

![[Azure ND H100 v5 rear.jpeg]]

From top to bottom:

* Two pairs of 4 E1.S SSD carriers
* One RJ45 management port and four [[cables and connectors|OSFP]] ports. Each [[cables and connectors|OSFP]] port carries two ports of NDR [[InfiniBand]]
* A two-port [[Azure SmartNIC]]
* A one-port Ethernet NIC that works in conjunction with the SmartNIC
* Six power supplies

These nodes are what comprise the [[Eagle]] supercomputer.

[^1]: Took these photos myself at Microsoft's booth at SC'23.
[^2]: [ND-H100-v5 size series - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/gpu-accelerated/ndh100v5-series?tabs=sizebasic)