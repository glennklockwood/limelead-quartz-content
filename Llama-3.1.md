---
title: Meta Llama-3.1
tags:
  - model
aliases:
  - Llama-3
---
Llama-3 is a family of models described in [The Llama-3 Herd of Models (arxiv.org)](https://arxiv.org/abs/2407.21783). Its largest form, Llama-3.1 405b, may be considered a [[frontier model]].

Llama-3 is a dense [[transformer]] that is a bigger and better-trained version of the previous Llama-2 model. Compared to that model,

1. They trained on more, higher-quality data (15 trillion tokens)
2. They trained more, using 16K nodes on [[Meta's H100 clusters|Meta's H100 RoCE cluster]] and cranking through $3.8 \times 10^{25}$ ops total.

Of note though, Llama-3 uses Grouped-Query Attention (GQA) instead of multi-head attention to reduce the number of key and value tensors, reducing computation requirements and memory footprint.[^moviegenpaper]

Oxen.ai has a great summary of the paper.[^oxen-ai-summary] In brief, the paper has:

- A good explanation of how they cleaned their training data
- Great anecdotes about [[component reliability]] and [[JMTTI]]
- A description of techniques they used to train long contexts, anneal the model, and other practical things.

[^oxen-ai-summary]: [arXiv Dive: How Meta Trained Llama 3.1 (oxen.ai)](https://ghost.oxen.ai/llama-3-1-herd-of-models/)
[^moviegenpaper]: [Movie Gen: A Cast of Media Foundation Models](https://ai.meta.com/static-resource/movie-gen-research-paper).