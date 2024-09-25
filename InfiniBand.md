---
tags:
  - network
---
InfiniBand is a low-latency network technology used in high-performance computing and high-performance storage environments.

## Key features

InfiniBand delivers low-latency using RDMA, which allows servers to directly read and write to memory in application memory without involving either the OS kernel or interrupting the host CPU. InfiniBand's lowest-level API is the verbs library which has a user space library which is what MPI applications use (uverbs) and a kernel space library which is what file systems that support RDMA (like [[Lustre]] or [[VAST]]) use (kverbs). Higher-level libraries like UCX and libfabric are built on top of this verbs API.

The network switches in an InfiniBand fabric tend to be very dumb. Instead of letting switches decide how to route traffic as it flows across the fabric like Ethernet, InfiniBand relies on a separate _subnet manager_ (SM) to act as the brain of the fabric and update the global routing on all of the switches when anything on the fabric gets changed. The SM can be embedded in one of the InfiniBand switches on the network, but it is more common to have a standalone subnet management server somewhere on the fabric.

InfiniBand itself is lossless: rather than dropping packets, endpoints negotiate with each other to ensure that senders never send more traffic than can be picked up by the receiver.

## History

Although InfiniBand NICs (called host channel adapters, or HCAs) and switches used to be manufactured by a wide assortment of companies, the InfiniBand industry consolidated in the mid-2010s, and by 2019, Mellanox was the only company still releasing InfiniBand products. Mellanox was acquired by NVIDIA, and now NVIDIA remains the de facto exclusive developer of InfiniBand.

Just as NVIDIA GPUs have become the de facto standard processor for large-scale [[LLM training]], InfiniBand has become the de facto standard network technology in AI clusters.

## Routing

**Up-down routing** applies to fat trees and only lets traffic change directions once. For a three-layer tree with IB0, IB1, and IB2 switches, traffic cannot bounce between IB1 and IB2 to work around bad paths; once traffic makes it up to the top IB2 layer, it turns around and must only go down to its destination.

## Scalability challenges

### Address space limitations

The InfiniBand address space, used to define endpoints (LIDs), only supports 16 bits. This means the maximum theoretical number of InfiniBand endpoints on a single fabric is $2^{16} = 65,563$. In practice, the number is even lower (48,895) due to many of the highest-order LIDs being reserved.[^LIDlimit] As a result, you cannot build a cluster with more than 48,895 InfiniBand HCAs talking to each other. As the scale of AI training clusters approaches this (e.g., [[Meta's H100 clusters]] have 24K endpoints, and [xAI is claiming to be building a 50K H100 cluster](https://x.com/elonmusk/status/1798066855170679021)), we'll start to see creative solutions to working around this 48K hard limit.

### Connection scalability

Building connections on InfiniBand, each connection occupies 10 KiB of space in the NIC. If you want to connect 10K endpoints, you cannot fit these all in NIC memory, so you get _connection thrashing_ where the NIC is effectively paging in and out connection states to host memory which is O(microseconds), driving up the latency of communications.

[^LIDlimit]: [IB router architecture and functionality (nvidia.com)](https://enterprise-support.nvidia.com/s/article/ib-router-architecture-and-functionality)