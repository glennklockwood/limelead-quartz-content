---
title: Network flow
aliases:
  - flows
tags:
  - network
---
A network flow is a bunch of network messages that accomplish some higher level task. The term is meaningful because the lower layers of the network stack (e.g., L2, like Ethernet) cannot tell what the higher levels are trying to do (e.g., L7, like scp a file). In this example, the scp file transfer could be represented as one flow at the lower layers.

Mechanically, flows can be defined using combinations of information contained in packets. For example,[^accelnet]

> [!quote]
> VFP unified flows (UF) definition, matching a unique source and destination L2/L3/L4 tuple, potentially across multiple layers of encapsulation, along with a header transposition (HT) action specifying how header fields are to be added/removed/changed.

> [^accelnet]: [[Firestone 2018]]