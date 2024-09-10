---
title: Multi-plane topologies
tags:
  - "#network/topology"
aliases:
  - rail-optimized
---
Multi-plane fat tree is a topology where each node has multiple NICs, and each NIC connects to its own independent (or semi-independent) fat trees.

## Rail-optimized InfiniBand

NVIDIA calls this "rail-optimized" and uses a configuration whereby:

- Each GPU is associated with an [[InfiniBand]] HCA (NIC) within an eight-way DGX-like node
- Each of the eight NICs connect to a different leaf switch
- $\therefore$ each node connects to eight different leaf switches

**Downside**: You need eight switches and eight different GPU nodes in close physical proximity to be able to use copper cabling in this configuration. This means high-power racks and high-density nodes (probably liquid cooled). The alternative is to eat the cost of using a lot of (expensive) optics to connect nodes to leaf switches.

**Upside**: A wider pool of GPUs can talk to each other using a single switch hop. For example, using 64-port 400G NDR [[InfiniBand]] switches:

- With rail-optimized, you can connect 32x GPU nodes to a single switch
- Without rail-optimized, you can connect only 4x GPU nodes to a single switch

Spreading GPUs out like this provides more local paths, reducing potential congestion above the leaf switches.