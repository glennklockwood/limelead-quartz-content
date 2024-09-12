---
title:
---
It's hard to keep track of Microsoft's public disclosures about its AI supercomputers, so here is what has been stated publicly.

## Mark Russinovich at Build 2024:

The following is from [Inside Microsoft AI innovation with Mark Russinovich](https://build.microsoft.com/en-US/sessions/984ca69a-ffca-4729-bf72-72ea0cd8a5db).

### Scale

Timeline:

- May 2020: 10,000 V100 GPUs, #5 supercomputer (not confirmed)
- Nov 2023: 14,400 [[H100]] GPUs, #3 supercomputer (confirmed)
	- "we're already on to the next one and designing for the next one after that which are multiple times even bigger than this one."
- May 2024: 30x supercomputers

> [!quote] 30x supercomputers comment
> We've built 30x the size of our AI infrastructure since November, the equivalent of five of those supercomputers I showed you every single month

The math is 5x [[Eagle]]s $\times$ 6 months between November 2023 and May 2024 to get 30x.

> [!quote] InfiniBand domain size
> If you take a look at the scale of those systems I was talking about, those AI supercomputers, there are tens of thousands of servers, and they all have to be connected together to make that efficient all-reduce happen.
> ...the InfiniBand domain covers the entire supercomputer, which is tens of thousands of servers.

> [!quote] Standard cluster size
> In the case of our public systems where we don't have customers that are looking for training at that scale ... the [[InfiniBand]] domains are 1,000 - 2,000 servers in size, which is still 10,000 - 20,000 GPUs.

### Project POLCA

- Training has only 3% power headroom
- Inference has 21% power headroom
- Allows 30% more servers in existing datacenter with minimum performance loss.