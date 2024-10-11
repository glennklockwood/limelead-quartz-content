---
title: Reasoning models
aliases:
  - o1
tags:
  - model
---
Reasoning models are a class of AI models that can adjust the amount of computation and time they use to generate better answers.  OpenAI's o1 model family is the first reasoning model.

## OpenAI o1

OpenAI's o1 models are the successful combination of reinforcement learning and scaling unsupervised learning with transformers.[^1] o1 creates its own chains of thought and follows them instead of training on a human-supplied chain of thought. Internally, these chains of thought involve the model questioning itself as it comes to a solution, and the model is aware of the constraints placed on its ability to think (e.g., limited time).

## Future

Current reasoning models can spend minutes reasoning, but future reasoning models will be allowed to think for "months or years" to improve the quality of their responses.[^1]

Reasoning models will be able to plan and self-correct, enabling these models to contribute to their own development and improvement.[^1]

Because reasoning allows models to work around problems it encounters on the path to find novel solutions. This forms a strong foundation for [[superintelligence]].

[^1]: [Building OpenAI o1 (Extended Cut)](https://www.youtube.com/watch?v=tEzs3VHyBDM)