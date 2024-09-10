---
title: Memory bandwidth
---
Memory bandwidth is expressed in GT/s (gigatransfers per second). The bytes per transfer is a function of the width of each channel and how many channels are used in parallel.

For example, [[Fugaku]] has four stacks of HBM2 and a total of 1,024 GB/s of memory bnadwidth per CPU.[^fugaku-stacks] This breaks down to 256 GB/s/stack which can be calculated by first knowing that HBM has 128-bit channels and 8 channels per stack: 

$128 \text{ bits} \times 8 \text{ channels} = 128 \text{ bytes per transfer}$ 

The HBM is clocked at 1.0 GHz, and because HBM is DDR, there are two transfers per clock. Thus, HBM has $2 \times 10^{9} \text{ transfers/sec}$. Given 128 bytes per transfer,

$2 \times 10^{9} \text{ transfers/sec} \times 128 \text{ bytes/transfer} = 256 \times 10^9 \text{ bytes/s} = 256 \text { GB/s}$

Memory channels are the interfaces with which memory controllers can talk to memory, and a single memory chip (die) may implement one or more channels.[^lpddr5]

## DDR

DDR5 has 64-bit channels, but unlike previous generations, implements these as two independent 32-bit sub-channels. RDIMMs have an additional 8 bits for ECC.[^ddr5]

## GDDR

Each channel of GDDR6 uses two 16-bit subchannels.[^gddr6]

| Memory | Data width per channel | Typical number of channels |
| ------ | ---------------------- | -------------------------- |
| DDR5   | 32 bits                | 2 channels                 |
| GDDR5  | 32 bits                | 16 channels                |
| HBM2e  | 128 bits               | 8 channels                 |
| HBM3   | 64 bits                | 16 channels                |
| LPDDR5 | 16 bits                |                            |

## HBM

HBM2 uses 128-bit channels, and each stack has eight channels.[^hbm2]

HBM3 gets complicated:
- It uses 64-bit channels and sixteen channels per stack
- Each 64-bit channel is implemented using two 32-bit subchannels ("pseudo-channels").[^hbm3]

## LPDDR

LPDDR5 uses 16-bit channels.[^lpddr5]

[^ddr5]: [DDR5 Memory Standard: An introduction to the next generation of DRAM module technology - Kingston Technology](https://www.kingston.com/en/blog/pc-performance/ddr5-overview)
[^gddr6]: [Micron Reveals GDDR6X Details: The Future of Memory, or a Proprietary DRAM? | Tom's Hardware (tomshardware.com)](https://www.tomshardware.com/news/micron-reveals-gddr6x-details-the-future-of-memory-or-a-proprietary-dram)
[^hbm2]: [7.1. High Bandwidth Memory (HBM2) DRAM Bandwidth (intel.com)](https://www.intel.com/content/www/us/en/docs/programmable/683189/21-3-19-6-1/high-bandwidth-memory-hbm2-dram-bandwidth.html#:~:text=For%20each%20HBM2%20DRAM%20in%20an%20Intel%C2%AE%20Stratix%C2%AE,device%2C%20there%20are%20eight%20channels%20of%20128-bits%20each.)
[^hbm3]: [What is High Bandwidth Memory 3 (HBM3)? | Synopsys](https://www.synopsys.com/glossary/what-is-high-bandwitdth-memory-3.html)
[^lpddr5]: [LPDDR5 Tutorial - Deep dive into its physical structure - systemverilog.io](https://www.systemverilog.io/design/lpddr5-tutorial-physical-structure/)
[^fugaku-stacks]: [スパコンを使った 最先端の天気予報研究 (tsukuba.ac.jp)](https://www.ccs.tsukuba.ac.jp/wp-content/uploads/sites/14/2020/08/CCSsympo2020-Sato.pdf)