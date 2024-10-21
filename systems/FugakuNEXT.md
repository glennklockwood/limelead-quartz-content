---
title: R-CCS FugakuNEXT
tags:
  - system
---
FugakuNEXT is the codename for the follow-on flagship supercomputer for Japan after [[Fugaku]].

Satoshi Matsuoka has started talking about their vision for FugakuNEXT since around 2022. The vision for its CPU is:[^matsuoka22]

![[FugakuNEXT CPU vision 2022.png]]

Themes that may be relevant to a processor or node include:[^matsuoka22]

- 3D stacking of memory and logic (as depicted above)
- Silicon photonics
- Large SRAMs, a la AMD 3D VCache
- Specialized [[tensor cores|tensor core]]-like data paths for scientific motifs like stencils, convolution, FFTs
- [[CGRA]] instead of or in addition to SIMD
- Processing-in-memory (PIM)

The [[CGRA]] is called out as a "strong scaling accelerator" candidate, so perhaps the CPU socket will have tiles of general-purpose CPU cores as well as [[CGRA]] tiles. 

[^matsuoka22]: [スパコンを使った 最先端の天気予報研究 (bnl.gov)](https://www.bnl.gov/modsim/events/2022/files/talks/satoshi-matsuoka.pdf)