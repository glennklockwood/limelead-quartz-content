---
aliases:
  - Slingshot-11
  - Slingshot-10
  - Cassini
  - Rosetta
tags:
  - network
---
Slingshot is the Ethernet-based high-speed interconnect developed by Cray after its Aries interconnect.  Slingshot runs at 200 Gbps and uses 64-port switches.

Cray released Slingshot in two phases:

1. **Slingshot-10** was Slingshot switches (codenamed Rosetta) combined with Mellanox ConnectX-5 100G NICs. The NICs talk RoCE up to the first switch, and inter-switch communication converts to using the proprietary [“HPC Ethernet” protocol](#protocol).
2. **Slingshot-11** introduced 200G Slingshot NICs (codenamed Cassini) to replace the ConnectX-5 NICs. In addition to doubling injection bandwidth to 200G, it allowed the [HPC Ethernet](#protocol) protocol to extend all the way to the compute nodes rather than relying on the switches to translate between standard Ethernet and [HPC Ethernet](#protocol).

## Rosetta

Rosetta is a 64-port, 200G switch built using 56G serdes using [[PAM4 and NRZ#PAM4|PAM4]] signaling. It was designed with [[dragonfly]] topology in mind, and at its maximum scale, supports 278,528 endpoints using 32 switches per group, each with:

- 16x L0 ports
- 31x L1 ports
- 17x L2 ports

$17 \text{ ports} \times 32 \text{ switches} = 544 \text{ global links per group}$ which means a maximum scale of 545 groups, each with 512 endpoints.

Rosetta switches implement a crossbar using 32 tiles, each with two switch ports. The ASIC itself is built on TSMC 16 nm and consume 250W apiece.

## Cassini

Cassini is the 200G NIC that can speak both [HPC Ethernet](#protocol) and standard Ethernet. It supports hardware tag matching, offloading MPI progression, one-sided operations, and collectives.[^3] It is available in both [[Cray EX]] mezzanine form factor and standard PCIe AIC.

## Software

Slingshot's primary API is libfabric, although it has a low-level API called CXI which is what Slingshot's fabric provider uses. Because libfabric really only provides a user-space API, Cray created kfabric so their ClusterStor E1000 Lustre appliances (whose clients use kernel-space [[Lustre]] clients) could use Cassini NICs for end-to-end RDMA.[^1]

## Protocol

Slingshot uses "HPC Ethernet" which is a modified version of standard Ethernet that increases packet rate and reduces latency:[^2]

- Reduces 64-byte frame size to 40 bytes (or 32-byte payload with embedded sideband data) by reducing the preamble and allowing IP packets to be sent without an L2 header.
- Removed 12-byte (96-bit) inter-packet gap
- Uses credit-based flow control
- Implements forward error correction
- Implements link-level retry

[^1]: [kfabric Lustre Network Driver (cug.org)](https://cug.org/proceedings/cug2023_proceedings/includes/files/pres119s2.pdf)
[^2]: [How Cray Makes Ethernet Suited For HPC And AI With Slingshot (nextplatform.com)](https://www.nextplatform.com/2019/08/16/how-cray-makes-ethernet-suited-for-hpc-and-ai-with-slingshot/)
[^3]: [Cray’s Slingshot Interconnect Is At The Heart Of HPE’s HPC And AI Ambitions (nextplatform.com)](https://www.nextplatform.com/2022/01/31/crays-slingshot-interconnect-is-at-the-heart-of-hpes-hpc-and-ai-ambitions/)