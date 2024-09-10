---
tags:
  - storage
---
VAST Data makes an eponymous all-flash parallel file system that blends NFS with parallel file system concepts.

## Technology

I wrote a description about some of the unique aspects of its architecture here: [VAST Data's storage system architecture (glennklockwood.com)](https://blog.glennklockwood.com/2019/02/vast-datas-storage-system-architecture.html). In brief,

1. **NFS clients**: It allows standard NFS clients to connect to it for low performance, but also supports enhanced NFS clients including NFSoRDMA and NFS multipathing to allow a single client to issue reads and writes to multiple VAST frontend servers in parallel.[^1]
2. **Novel write path**: It lands all writes to storage-class memory (formerly Optane, but now Kioxia FL6) to preserve low latency at the cost of write bandwidth. Data accumulates in this write layer over time, building massive stripes that contain similar data (using a locality-sensitive hash on incoming data). When a write stripe is full, it is compressed and flushed to low-durability, cost-effective QLC flash.[^3]
3. **Novel data protection**: VAST uses extremely wide stripes of up to 150+4 to protect data with very high efficiency. They use locally decodable codes instead of standard Reed-Solomon codes to avoid having to rebuild using 150 surviving drives; instead, they only need $\frac{150}{4}$ surviving drives to rebuild.[^2]

Items 2 and 3 above allow VAST to get away with using all low-endurance QLC flash at an effective price-per-gigabyte rate that becomes competitive with hybrid disk+flash storage systems in overall value. VAST is neither the fastest nor the cheapest, but it does both reasonably well.

## Customers

VAST is seeing broad success in midrange public HPC, private sectors including finance, and GPU-as-a-Service cloud providers.

VAST's most notable customers include:

- TACC, which houses the NSF's largest supercomputers[^4]
- CoreWeave, a growing player in providing GPUs-as-a-Service[^5]
- Lambda Labs, a major GPU-as-a-Service cloud provider[^6]
- Core42, a major GPU-as-a-Service cloud provider in the middle east[^7]

[^1]: [Meet Your Need for Speed with NFS and VAST Data](https://www.vastdata.com/blog/meet-your-need-for-speed-with-nfs)
[^2]: [Breaking Resiliency Trade-Offs with Locally Decodable Codes (vastdata.com)](https://www.vastdata.com/blog/breaking-resiliency-trade-offs-with-locally-decodable-erasure-codes)
[^3]: [Client-side Compression Techniques and their Impact on Similarity Reduction – VAST Data (zendesk.com)](https://vastdata.zendesk.com/hc/en-us/articles/360011304840-Client-side-Compression-Techniques-and-their-Impact-on-Similarity-Reduction)
[^4]: [VAST Data wins TACC supercomputer storage contract – Blocks and Files](https://blocksandfiles.com/2023/07/25/vast-data-tacc-contract/)
[^5]: [CoreWeave Partners With VAST Data](https://www.vastdata.com/coreweave)
[^6]: [Lambda Partners with VAST Data](https://www.vastdata.com/lambda)
[^7]: [Core42 Partners with VAST Data](https://www.vastdata.com/core42)