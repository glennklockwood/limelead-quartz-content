---
title:
---
It's hard to keep track of Microsoft's public disclosures about its AI supercomputers, so here is what has been stated publicly.

## Kevin Scott at Build 2024

From https://news.microsoft.com/wp-content/uploads/prod/2024/05/KevinScott_transcript_KEY01_Build20204.pdf:

> [!quote]
> In 2020, we built our first Al supercomputer for OpenAI. It's the supercomputing environment that trained GPT-3. And so, we're going to just choose "marine wildlife" as our scale marker.
> 
> You can think of that system as about as big as a shark.
>
> The next system that we built, scale wise, is about as big as an orca. And that is the system that we delivered in 2022 that trains GPT-4.
>
> The system that we have just deployed is, scale wise, about as big as a whale relative to the shark-sized supercomputer and this orca-sized supercomputer. And it turns out you can build a whole hell of a lot of Al with a whale-sized supercomputer. (Laughter.)

## Mark Russinovich at Build 2024:

The following is from [Inside Microsoft AI innovation with Mark Russinovich](https://build.microsoft.com/en-US/sessions/984ca69a-ffca-4729-bf72-72ea0cd8a5db).

### Scale

> [!quote]
> The first AI supercomputer we built back in 2019 for OpenAI was used to train GPT-3 and the size of that infrastructure, if we'd submitted it to the TOP500 supercomputer benchmark, would have been in the top five supercomputers in the world on-premises or in the Cloud and the largest in the Cloud.
> 
> In November, we talked about the two generations later supercomputer, so that we built one to train GPT-4 after the GPT-3 one and this one we're building out to train the next generation of OpenAI's models this one with a slice of that infrastructure, just a piece of that supercomputer that we were bringing online.

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