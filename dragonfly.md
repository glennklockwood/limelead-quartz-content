---
title: Dragonfly topology
tags:
  - "#network/topology"
---
Dragonfly is a topology popularized by Cray[^1] starting in their Cascade ([[Cray XC]]) platform and carried forward to their Shasta ([[Cray EX]]) platform.

Dragonfly uses a hierarchy of switch groups, where within each group, every switch is connected to every other switch. As you add more dragonfly layers, instead of switches, you deal with groups of switches that are also connected in an all-to-all fashion.

Dragonfly networks are usually fully non-blocking at the lower levels (due to their all-to-all connectivity) and only have a taper at the highest (global) level. This connectivity is expressed in _links per arc_, where an arc is the connectivity between any two groups in the fabric.

This topology works very well for physically dense nodes, because the more nodes and switches you can fit into a rack, the more you can use inexpensive [[cables and connectors|DAC]] cables instead of pricey optics. As you go to less-dense form factors though, you wind up having to use all optical connections anyway, and the benefits of dragonfly diminish.

There are modifications to dragonfly, like [[dragonfly+]], which maintain the all-to-all connectivity between groups at the global level, but use an alternative topology (like a fat tree in the case of [[dragonfly+]]) within each group.
## Blocking factor

If a supercomputer uses four links per arc and $N$ groups,

- Each group has 512 injection links from endpoints
- Each group has 4 ejection links to each of $N-1$ groups

From this, you can calculate the blocking factor of the whole fabric:

Blocking factor $= \frac{512 \text{ endpoints}}{(N-1) \text{ other groups} \times 4 \text{ global links}}$

## In Cray EX

[[Cray EX]] uses 64-port [[Slingshot]] switches in two-level dragonfly topologies to interconnect massive supercomputers.

In the typical [[Cray EX]] configuration, each group has 32 [[Slingshot]] switches, and each 64-port [[Slingshot]] switch has:

- Up to 16 ports down to compute nodes (L0)
- 31 ports to other switches in the rack (L1)
- Up to 16 ports to other groups (L2)[^L2]

So per group, there are:

- Up to 512 ports down to endpoints
- 992 ports connecting intra-group switches
- Up to 512 ports connecting inter-group switches

Cray lets customers choose how many links per arc; for example:

- [[Aurora]] uses two links per arc
- [[Frontier]] uses four links per arc

## In Cray XC

[[Cray XC]] introduced the dragonfly topology and was built from 48-port [[Aries]] routers. The typical topology had

- 16 Aries routers per chassis (768 ports/chassis)
- 6 chassis per group (4608 ports/group)
- 96 Aries routers per group
- 2 cabinets per group

So each 48-port [[Aries]] router used:[^2]

- 4 ports down to compute nodes
- 15 ports to other routers in the chassis
	- called L1, rank-1, or "green"
- 15 ports to other chassis in the group
	- called L2, rank-2, or "black"
	- 3 links per inter-chassis arc
- 10 ports to other groups
	- called L3, rank-3, or "blue"
	- 4 links per inter-group arc

Although each Aries router contributed 4 global links, they were spread over the 960 global ports coming out of a dragonfly group. This meant the maximum size of a [[Cray XC]] was 241 groups (960 global links bundled into arcs of 4 could connect to 240 other groups).

[^1]: [Technology-Driven, Highly-Scalable Dragonfly Topology | IEEE Conference Publication | IEEE Xplore](https://ieeexplore.ieee.org/document/4556717)
[^2]: [Cray XC Series Network (anl.gov)](https://www.alcf.anl.gov/files/CrayXCNetwork.pdf)
[^L2]: The original [Slingshot talk by Steve Scott](https://www.nextplatform.com/2019/08/16/how-cray-makes-ethernet-suited-for-hpc-and-ai-with-slingshot/) said each switch supports up to 17 global links which is true. However, production systems never seem to do use the last port and keep the global link count at 16 per switch. See [[Slingshot#Rosetta]] for more.