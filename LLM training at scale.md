---
tags:
  - anecdotes
---
Unique challenges arise when performing [[LLM training]] at the scale required for [[frontier model]]s, and there is no end in sight for training increasingly large models to get higher quality results.[^3]

## Multi-data center training

[[Gemini]] was "trained across multiple sites, and multiple clusters within those sites" according to Thomas Kurian.[^1] Groups of 4096 [[TPUv4]] chips were formed into superpods which share a common optical switch. Superpods are then connected within and across data centers for synchronous training. Each superpod hosts one model replica, and data parallelism is used across superpods. Google also said that multiple model replicas were stored in a single superpod to accelerate restart "despite the significantly larger training resources being used."[^2]

[[Mark Russinovich]] said [^markruss]

> [!quote]
> “I think it’s inevitable, especially when you get to the kind of scale that these things are getting to,” he said. “In some cases, that might be the only feasible way to train them is to go across data centers, or even across regions,” he said.

[[Dylan Patel]] wrote a broad report on this behind a paywall, but the summary claims that Google and Microsoft/OpenAI are both pursuing this.[^dylan]

## Silent data corruption

Google reported silent data corruption events "every week or two." They used dedicated resources to monitor for silent data corruption, and used deterministic training to allow training to be replayed to recalculate weight updates when silent data corruption was detected. They do not say exactly how they detect silent data corruption and whether it only detected high-order bits or all corruption events.[^2]

Meta reported six silent data corruptions when training across 16K [[H100]] GPUs for 54 days.[^llama3]

## Asynchronous training

Asynchronous training is a nascent technique to allow training to occur where model weights are not globally synchronized continuously. This allows parts of training to continue even if a single GPU, node, or data-parallel model replica fails. Dylan Patel wrote a good summary of this.[^dylan]

## Convergence

Large language models are not trained to convergence. Instead, empirical scaling laws are used to determine the ideal number of training tokens required for a model of a given size, then this amount of data is used to train the model. From the [PaLM 2 Technical Report](https://ai.google/static/documents/palm2techreport.pdf):

>[!quote] Scaling law experiments
> Scaling Transformer language models has become a popular way to achieve state-of-the-art performance. Kaplan et al. (2020) studied the relationship between scaling the amount of training data (D) and model size (N), and reached the empirical conclusion that it follows a power law, with N needing to grow faster than D. Hoffmann et al. (2022) built upon this observation with a similar study that tuned smaller models’ hyperparameters better. Their results corroborated Kaplan et al. (2020)’s power law conclusion; however, they arrived at different results regarding the optimal ratios, showing that N and D should instead grow in equal proportions.

No such scaling laws exist for training [[frontier models for science]] which forces that field to do extensive hyperparameter optimization prior to training. This is one of the places where [[frontier models for science]] are far behind the state of the art.

## Anecdotes

* **ByteDance's MegaScale paper**[^megascale] contains descriptions of the entire infrastructure required to train at 10K+ GPU scale.
- **Meta AI's [[OPT-175B]] logbook**[^opt-logbook] provides specific details about errors and mitigations that happened while training an LLM across 1K GPUs for two months.

[^1]: [Training Google's Gemini: TPUs, multiple data centers, and risks of cosmic rays - DCD (datacenterdynamics.com)](https://www.datacenterdynamics.com/en/news/training-gemini-tpus-multiple-data-centers-and-risks-of-cosmic-rays/)
[^2]: [Gemini: A Family of Highly Capable Multimodal Models](https://storage.googleapis.com/deepmind-media/gemini/gemini_1_report.pdf)
[^llama3]: [The Llama-3 Herd of Models (arxiv.org)](https://arxiv.org/abs/2407.21783)
[^megascale]: [[2402.15627] MegaScale: Scaling Large Language Model Training to More Than 10,000 GPUs (arxiv.org)](https://arxiv.org/abs/2402.15627)
[^opt-logbook]: [metaseq/projects/OPT/chronicles at main · facebookresearch/metaseq (github.com)](https://github.com/facebookresearch/metaseq/tree/main/projects/OPT/chronicles)
[^3]: See [OpenAI Keynote on Building Scalable AI Infrastructure](https://www.servethehome.com/openai-keynote-on-building-scalable-ai-infrastructure/) and the scaling plots cited from the GPT-4 technical report.
[^dylan]: [Multi-Datacenter Training: OpenAI's Ambitious Plan To Beat Google's Infrastructure](https://www.semianalysis.com/p/multi-datacenter-training-openais)
[^markruss]: [Microsoft Azure CTO: US data centers will soon hit limits of energy grid | Semafor](https://www.semafor.com/article/10/11/2024/microsoft-azure-cto-us-data-centers-will-soon-hit-limits-of-energy-grid)