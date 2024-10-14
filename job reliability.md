---
title: Job reliability
---
There are many relevant terms and concepts. Here is a brain dump of them:

- **[[JMTTI]]** is Job Mean Time To Interrupt.
- **Job uptime** is what fraction of time in a fixed window (like 24 hours) a large-scale HPC job is actually running.
- **Forward progress** is like job uptime, but it only refers to time when the job is actually productively computing new things. If the job is running but it's writing a checkpoint or reading a checkpoint, it is not making forward progress.  Similarly, if a job is re-computing the last 20 timesteps because 20 timesteps were lost since the last checkpoint and the job crashed, that recomputation is not forward progress. This metric is very hard to measure without having direct knowledge of the specific application.
- **Normalized MTTI** and **normalized MTBF** follow what Gupta et al[^2] called "Scale-normalized MTBF" which is $\frac{\text{MTBF} \times \text{nodes in system}}{\text{some reference node count}}$.

## In practice

Below are some anecdotes about failures when running large-scale workloads. Most of the literature refers to traditional scientific computing workloads, but training frontier LLMs is emerging as a new source of failure rates in production.

| Metric                | Meta Llama-3[^1]  | Google Gemini[^5] | Meta OPT-175B[^opt175b]  | OLCF Titan[^2]    | LANL Trinity                 |
| --------------------- | ----------------- | ----------------- | ------------------------ | ----------------- | ---------------------------- |
| JMTTI                 | 2.5 hours         |                   | 7.5 hours                |                   | < 12 hours                   |
| Job uptime            | 90%               | 85-97%            | 59%                      |                   |                              |
| Normalized MTTI (10K) | 4.00 hours        |                   | 7.54 hours               |                   | < 34.0 hours                 |
| Normalized MTBF (10K) |                   |                   |                          | 27.1 hours[^2]    |                              |
| # components          | 16,000x H100 GPUs | TPUv4             | 992x or 1,024x [[A100]] GPUs | 18,688x K20X GPUs | > 9900x KNL +18,400x Haswell |
| Year deployed         | 2024              | 2023              | 2021                     | 2014              | 2017[^4]                     |

Normalized MTBF/MTTI above is normalized to 10,000 components above.

A team at Sandia ran an application called SPARTA on [[LANL Trinity]] across 1.2 million MPI processes over the course of three days in 2018.[^3] Their memo contained the following anecdotes:

- Six hardware failures (three SIGBUS and three node failures) across over 9,200 [[Haswell]] and 9,900 [[KNL]] nodes.
- Ran srun inside a for loop to automatically restart when a job failed due to a SIGBUS interrupt.
- They identified and banned two slow Haswell nodes

They also cite software bugs they discovered at scale, so it is unclear how much of those three days were actually spent computing. As such, it is hard to say what the JMTTI was except that the upper bound was $\frac{3 \text{ days} \times 24 \text{ hours}}{6 \text{ failures}} = 12 \text{ hours}$.

[^1]: "Despite these challenges, for Llama 3, we achieved higher than 90% effective training time while supporting automated cluster maintenance, such as firmware and Linux kernel upgrades, which resulted in at least one training interruption daily. The effective training time measures the time spent on useful training over the elapsed time." [The Llama-3 Herd of Models (arxiv.org)](https://arxiv.org/abs/2407.21783)
[^2]: [Failure in large scale systems: long-term measurement, analysis, and implications](https://dl.acm.org/doi/10.1145/3126908.3126937)
[^3]: [Full Trinity run with SPARTA](https://www.osti.gov/servlets/purl/1528747)
[^4]: Hemmert et al. [Trinity: Opportunities and Challenges of a Heterogeneous System](https://cug.org/proceedings/cug2018_proceedings/includes/files/pap135s2-file1.pdf). 2018.
[^5]: "Compared to both PaLM and PaLM-2, this provided a substantial speedup in recovery time, despite the significantly larger training resources being used. As a result, the overall goodput for the largest-scale training job increased from 85% to 97%." [Gemini: A Family of Highly Capable Multimodal Models](https://storage.googleapis.com/deepmind-media/gemini/gemini_1_report.pdf)
[^opt175b]: See the [[OPT-175B]] page for a breakdown of that training.