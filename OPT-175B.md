---
tags:
  - anecdotes
  - model
---
[OPT-175B](https://arxiv.org/abs/2205.01068) is the largest of [[Meta]]'s Open Pretrained Transformer models, released in May 2022 and trained at the end of 2021.

It was trained on 992[^paper] or 1024[^logbook-final] NVIDIA [[A100]] 80G GPUs running in [[Microsoft|Microsoft Azure]][^azure] over the course of 56 days.[^zhang] A [114-page logbook](https://github.com/facebookresearch/metaseq/tree/main/projects/OPT/chronicles) was released by Meta that documents the on-call engineers who kept the training happening during that time, offering a unique view into [[LLM training at scale]].

## Efficiency

The model achieved 147 TFLOPS/GPU[^blog] and required 4.30E+23 FLOPS total.[^logbook-final] Doing some math, this means the total training time was:

$4.30 \times 10^{23} \text{ ops}\times \frac{1 \text{ GPU}}{147 \times 10^{12} \text{ ops} \cdot \text{ sec}^{-1}} \times \frac{1}{1024 \text{ GPUs}} = 2,856,611 \text{ s} = 793.5 \text{ h} = 33.1 \text{ days}$

The training began on November 5, 2021 and ended on January 6, 2022 for a total of 63 calendar days.[^logbook] However, Susan Zhang said it was trained over 56 days,[^zhang] and I haven't scoured the logbook to see where the missing week went. This means the overall job uptime was between 51.7% and 58.9%.

[^zhang]: [about me | Susan Zhang (suchenzang.github.io)](https://suchenzang.github.io/)
[^logbook]: [metaseq/projects/OPT/chronicles at main · facebookresearch/metaseq (github.com)](https://github.com/facebookresearch/metaseq/tree/main/projects/OPT/chronicles)
[^logbook-final]: [metaseq/projects/OPT/chronicles/final_update.md at main · facebookresearch/metaseq (github.com)](https://github.com/facebookresearch/metaseq/blob/main/projects/OPT/chronicles/final_update.md)
[^paper]: [[2205.01068] OPT: Open Pre-trained Transformer Language Models (arxiv.org)](https://arxiv.org/abs/2205.01068)
[^blog]: [Democratizing access to large-scale language models with OPT-175B (meta.com)](https://ai.meta.com/blog/democratizing-access-to-large-scale-language-models-with-opt-175b/)
[^azure]: The logbook refers to "CSP" and "cloud," but the [Baselines Logbook](https://github.com/facebookresearch/metaseq/blob/main/projects/OPT/chronicles/OPT_Baselines_Logbook.pdf) refer to the same tools with "cloud" replaced by "azure" (`fixmyazure`, `--full-azure-upload-path`). The logbook is also full of Azure-specific terms including blobs.