---
title: Model FLOPs Utilization
---
Model FLOPs Utilization is a metric proposed by Google that describes how well utilized a GPU is during model training:[^palm]

> [!quote] From the PaLM paper
> We propose a new metric for efficiency that is implementation-independent and permits a cleaner comparison of system efficiency, called model FLOPs utilization (MFU). This is the ratio of the observed throughput (tokens-per-second) relative to the theoretical maximum throughput of a system operating at peak FLOPs. Crucially, the “theoretical maximum” throughput only accounts for the required operations to compute the forward+backward passes, and not rematerialization. M

It sounds like arimethic intensity which is an essential component of the roofline model.

[^palm]: [PaLM: Scaling Language Modeling with Pathways](https://arxiv.org/abs/2204.02311)