---
title: Distillation
---
Distillation is the process by which a [[frontier model]] is used to generate high-quality synthetic data to train or fine-tune a smaller LLM (an [[SLM]]).

This is important because [Textbooks Are All You Need](https://arxiv.org/abs/2306.11644) demonstrated that training an SLM with high-quality data can reproduce the quality of an LLM on specific tasks. Replacing [[frontier model|frontier models]] with SLMs is advantageous because inferencing using a frontier model can be _very_ expensive, whereas an SLM might be small enough to inference directly on a client device like your laptop.

OpenAI has a distillation API now; see [Model Distillation in the API](https://openai.com/index/api-model-distillation/).