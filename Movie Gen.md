---
tags:
  - model
title: Meta Movie Gen
---
Movie Gen is a family of movie generation models announced by [[Meta]] in October 2024 and detailed in [Movie Gen: A Cast of Media Foundation Models](https://ai.meta.com/static-resource/movie-gen-research-paper). There are two variants:

**Movie Gen Audio**: a video-to-audio model which was scaled up to 13 billion parameters. It was trained on O(1M) hours of audio.

**Movie Gen Video**: a text-to-video and text-to-image model which was scaled up to 30 billion parameters and a 73K context length for video tokens (16 seconds at 16 fps). It was trained on O(100M) videos and O(1B) images.

Movie Gen Video was trained on 6,144 H100 GPUs in the Ethernet version of [[Meta's H100 clusters]] and fine-tuned on 512 GPUs.

The inferencing end of Movie Gen is rewritten using [[distillation]] of [[Llama-3.1|Llama-3]] 70B and 8B variants.

[^paper]: [Movie Gen: A Cast of Media Foundation Models](https://ai.meta.com/static-resource/movie-gen-research-paper).