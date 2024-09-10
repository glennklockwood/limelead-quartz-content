---
title: Ultra Ethernet
tags:
  - network
aliases:
  - UET
---
## Members

Founding members include AMD, Arista, Broadcom, Cisco, Eviden, HPE, Intel, Meta, and Microsoft.

The following notes are a loose summary of [Ultra Ethernet Specification Update - Ultra Ethernet Consortium](https://ultraethernet.org/ultra-ethernet-specification-update/) with some extra context I've picked up along the way.

## Basics

Ultra Ethernet is a new standard that builds on top of Ethernet designed to enhance the performance of HPC and AI workloads.

Ultra Ethernet defines three different profiles:

- **AI Base**: Designed for distributed training and inference which have a lot of bandwidth-bound collectives.
- **HPC**: Supports MPI semantics and provides direct mappings for one-sided, two-sided, and collective communications. It describes support for hardware tag matching.
- **AI Full**: A superset of AI Base which includes features like tagging and rendezvous support. Seems like a mashup of AI Base and HPC profiles.

## Low-latency congestion management

Congestion control allows the receiver or switches to ask senders to slow down. Without congestion control, every sender is allowed to send at line rate. 

Two methods:

- **Sender-based**: Similar to TCP/IP, the sender gets feedback from the network (round trip time, packet loss, and explicit congestion notification markings on packets) to decide when to slow down sends.
- **Receiver-based**: Similar to InfiniBand, the receiver issues credits to senders that allow senders to transmit.

### Packet trimming

When switches carrying regular IP traffic gets overwhelmed with traffic, they just drop entire frames. This means the receiver has to wait for a packet that never arrives until it times out, then it has to request a full retransmission from the source.

Ultra Ethernet introduces packet trimming, a feature where a switch can drop the payload of a frame but still send the header forward. Since the truncated frame is small, it is much easier to fit into switch buffers as it moves towards its destination. The receiver then detects a truncated (trimmed) packetand can immediately signal the sender to retransmit without waiting for a timeout due to a lost packet. This allows the sender to more quickly respond to network congestion, and both ends gain awareness of loss and congestion.

### Packet spraying

Ultra Ethernet does not have to statically map one [[flow]] to one route. Though it does use ECMP groups populated by BGP, Ultra Ethernet can load balance packets across many routes.

This packet spraying is dynamic as well, and the network continuously gets feedback about the performance of different paths so it can adaptively route traffic round congestion. This happens in addition to ECMP, so packet spraying can route around those links until global BGP can reconfigure ECMP groups to not use those links.

### Link-level retry

Ultra Ethernet implements Link Level Retry (LLR).

When standard Ethernet drops a frame due to congestion or corruption, a full end-to-end retransmit has to be requested after the receiver times out and stops waiting for the frame to arrive. This incurs a high latency due to the timeout and second round trip to re-send the missing packet.

With Ultra Ethernet, the source port for a transfer remembers the packet until the destination port acknowledges receipt. This way if something happens to the packet along that link, the last hop can quickly resend the frame and packet. This avoids a high-latency timeout (because the packet isn't dropped) and fast retransmit (because the resend only happens from the last hop, not all the way back at the source of the packet).

## Scalability

Ultra Ethernet uses ephemeral delivery contexts, require only a single lightweight message to establish, and can be created and destroyed without context. This means that beginning a transmission to another node does not incur high-latency round trips and sending small messages all over the network can be very efficient.

It can also pool connection contexts, allowing multiple NICs on a node to share a single connection to another node. By contrast, other protocols require each NIC pair to establish its own connection contexts. The end result is that connection scalability is a function of node count, not NIC count, allowing multi-NIC, multi-GPU nodes to scale out much more efficiently.