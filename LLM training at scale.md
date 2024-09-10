---
tags:
  - anecdotes
---
Unique challenges arise when performing [[LLM training]] at the scale required for [[frontier models]], and there is no end in sight for training increasingly large models to get higher quality results.[^3]

## Multi-data center training

[[Gemini]] was "trained across multiple sites, and multiple clusters within those sites" according to Thomas Kurian.[^1] Groups of 4096 [[TPUv4]] chips were formed into superpods which share a common optical switch. Superpods are then connected within and across data centers for synchronous training. Each superpod hosts one model replica, and data parallelism is used across superpods. Google also said that multiple model replicas were stored in a single superpod to accelerate restart "despite the significantly larger training resources being used."[^2]

## Silent data corruption

Google reported silent data corruption events "every week or two." They used dedicated resources to monitor for silent data corruption, and used deterministic training to allow training to be replayed to recalculate weight updates when silent data corruption was detected. They do not say exactly how they detect silent data corruption and whether it only detected high-order bits or all corruption events.[^2]

Meta reported six silent data corruptions when training across 16K H100 GPUs for 54 days.[^llama3]

## Anecdotes

* **ByteDance's MegaScale paper**[^megascale] contains descriptions of the entire infrastructure required to train at 10K+ GPU scale.
- **Meta AI's OPT-175B logbook**[^opt-logbook] provides specific details about errors and mitigations that happened while training an LLM across 1K GPUs for two months.

[^1]: [Training Google's Gemini: TPUs, multiple data centers, and risks of cosmic rays - DCD (datacenterdynamics.com)](https://www.datacenterdynamics.com/en/news/training-gemini-tpus-multiple-data-centers-and-risks-of-cosmic-rays/)
[^2]: [Gemini: A Family of Highly Capable Multimodal Models](https://storage.googleapis.com/deepmind-media/gemini/gemini_1_report.pdf)
[^llama3]: [The Llama-3 Herd of Models (arxiv.org)](https://arxiv.org/abs/2407.21783)
[^megascale]: [[2402.15627] MegaScale: Scaling Large Language Model Training to More Than 10,000 GPUs (arxiv.org)](https://arxiv.org/abs/2402.15627)
[^opt-logbook]: [metaseq/projects/OPT/chronicles at main · facebookresearch/metaseq (github.com)](https://github.com/facebookresearch/metaseq/tree/main/projects/OPT/chronicles)
[^3]: See [OpenAI Keynote on Building Scalable AI Infrastructure](https://www.servethehome.com/openai-keynote-on-building-scalable-ai-infrastructure/) and the scaling plots cited from the GPT-4 technical report.