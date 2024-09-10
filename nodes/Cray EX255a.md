---
tags:
  - node
---
Cray EX255a is the blade for [[Cray EX]] platforms that hosts [[MI300A]] APUs.[^1]

Each blade has

- 2x 4-socket node cards (8 APUs per blade)
- 1x NVMe M.2 SSD slot per node card (2 per blade)
- 4x-8x [[Slingshot|Slingshot-11]] injection ports per node (8x-16x per blade)

The lack of node-local storage is probably compensated for by [Cray's Rabbit chassis](https://www.hpcwire.com/2021/02/18/livermores-el-capitan-supercomputer-hpe-rabbit-storage-nodes/) which uses spare [[Slingshot]] switch ports in the EX platform to host NVMe servers.

The high density of 200G [[Slingshot]] ports is also likely a compensation for the fact that this platform is being released at a time when NVIDIA has moved on to 400G NDR InfiniBand.

Here's a photo of a blade:[^1]

![[Cray EX255a photo.jpg]]

[^1]: HPE's booth at ISC'24.