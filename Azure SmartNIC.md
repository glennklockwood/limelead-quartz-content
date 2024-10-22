---
title: Azure SmartNICs
tags:
  - network
aliases:
  - Azure Boost
  - Overlake
  - Catapult
  - MANA
---
[[Microsoft]] uses SmartNICs in all of its Azure servers to offload SDN functions since 2015.[^nsdi] Microsoft has historically implemented these NICs as a bump-in-the-wire FPGA sitting between a standard host NIC (like a Mellanox ConnectX-5) and the network cable that connects up to a switch.[^nsdi]

## History

[Microsoft's Project Catapult](https://www.microsoft.com/en-us/research/project/project-catapult/) was a research program in Microsoft Research that used network-attached FPGAs to accelerate distributed computations for Bing. That effort built muscle within Microsoft which then informed Azure's SmartNIC program which is detailed in [[Firestone 2018]].

Project Catapult resulted in network-attachable FPGA hardware and techniques to program them which were then adopted by a different part of Microsoft to create SDN accelerators within NICs. As a result, some of the physical NICs have multiple names depending on their application area, and there are different generational numbering schemes in flight which get confusing.

This is an interesting history slide [presented at Fermilab in 2022](https://indico.fnal.gov/event/22303/contributions/246438/attachments/157852/206736/Catapult_Putnam_Snowmass_2022_FPGA_Cloud__for_HPC.pdf):

![[Project Catapult.png]]

The above graphic is summarized below:

| Speed | Research name                                                                                                                     | SmartNIC name  | FPGA                           |
| ----- | --------------------------------------------------------------------------------------------------------------------------------- | -------------- | ------------------------------ |
| 40G   | Catapult v2                                                                                                                       | Pikes Peak     | Altera Stratix V[^nsdi]        |
| 50G   | Catapult v3                                                                                                                       | Longs Peak     | Altera Arria 10[^hotchips17]   |
| 100G  | [Project Brainwave](https://www.datacenterdynamics.com/en/news/microsoft-previews-fpga-based-machine-learning-service-for-azure/) | Storm Peak     | Altera Stratix 10[^hotchips17] |
| 100G  | Overlake                                                                                                                          | Celestial Peak | Altera Stratix 10[^github]     |
| 200G  |                                                                                                                                   | MANA           |                                |

Microsoft had an [[Azure HBv4]] with a "second-generation"[^hbv4] 80G NIC on display at SC22 as well,[^sth] showing the same model number (A-2040) as Celestial Peak.[^github] When you use this node, it appears as a ConnectX-5 Ethernet adapter and does not need any special drivers, consistent with older generations that combined the FPGA with a Mellanox NIC.

## MANA

The Microsoft Azure Network Adapter (MANA) is a 200G SmartNIC[^blog] that was unveiled at Build 2022.[^1] It is built on an undisclosed(?) FPGA and an Arm SoC running Microsoft Linux and is built on a single card as shown by [[Mark Russinovich]]:

![[Azure MANA NIC.jpg]]

These NICs are implemented entirely by Microsoft and require a special RDMA driver which has been upstreamed to Linux.[^linux] Funny enough, [[Firestone 2018]] considered this too much work:

> [!quote]
> Another option was to build a full NIC, including SR-IOV, inside the FPGA — but this would have been a significant undertaking (including getting drivers into all our VM SKUs), and would require us to implement unrelated functionality that our currently deployed NICs handle, such as RDMA.

## Second-hand

For some reason, these older NICs often appear for sale on eBay, but they aren't immediately useable when plugged into standard servers.[^github] For example, searching for ["Microsoft A-2040" on eBay](https://www.ebay.com/sch/i.html?_from=R40&_nkw=microsoft+a-2040&_sacat=0&rt=nc&LH_Sold=1&LH_Complete=1) brings up a number of Celestial Peak NICs for sale from China. As a result, there's a bunch of speculative knowledge online around this hardware.

Given the tight integration between these FPGAs and the Azure hypervisor described in [[Firestone 2018]], it's not surprising these cards don't function on their own.

[^1]: [Microsoft Build 2023 Inside Azure Innovations - Infrastructure acceleration through offload](https://youtu.be/7G2aM9LqqMI?si=EZAzuNbAwsp_g40G)
[^blog]: [Microsoft Azure Boost Preview](https://techcommunity.microsoft.com/t5/azure-infrastructure-blog/introducing-microsoft-azure-boost-preview/ba-p/3876742)
[^nsdi]: [[Firestone 2018]]
[^sth]: [Microsoft Azure at SC22 and The Enormous AMD EPYC Genoa Heatsink on HBv4 (servethehome.com)](https://www.servethehome.com/microsoft-azure-at-sc22-and-the-enormous-amd-epyc-genoa-heatsink-on-hbv4/)
[^hbv4]: [HBv4-series VM overview, architecture, topology - Azure Virtual Machines - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-machines/hbv4-series-overview)
[^linux]: [net: mana: Add a driver for Microsoft Azure Network Adapter (MANA) - kernel/git/netdev/net-next.git - Netdev Group's -next networking tree](https://git.kernel.org/pub/scm/linux/kernel/git/netdev/net-next.git/commit/?id=ca9c54d2d6a5ab2430c4eda364c77125d62e5e0f)
[^hotchips17]: See https://www.microsoft.com/en-us/research/uploads/prod/2017/08/HC29.22622-Brainwave-Datacenter-Chung-Microsoft-2017_08_11_2017.compressed.pdf which is linked from https://www.microsoft.com/en-us/research/blog/microsoft-unveils-project-brainwave/
[^github]: [Move on to the new toy, Celestial Peak. How can we RE it ? · Issue #2 · tow3rs/catapult-v3-smartnic-re · GitHub](https://github.com/tow3rs/catapult-v3-smartnic-re/issues/2)