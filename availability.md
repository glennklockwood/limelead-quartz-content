---
title: Availability
aliases:
  - job uptime
  - forward progress
---
Availability is the fraction of a supercomputer's nodes that are online and useable by jobs in a given interval of time.

## Quantitative definition

Availability is pretty straightforward:

$\text{Availability} = \frac{\text{uptime}}{\text{uptime } + \text{ downtime}}$

SLAs are often expressed as average monthly availability; for example, [[Microsoft|Azure]] offers a 99.9% ("three nines" or "3x9") availability for a non-redundant VM on a monthly basis.[^1] If the average month has 730.5 hours, this means that VM will only be down for 0.1% of that, or $730.5 \text{ hours} \times (1 - 0.999) = 0.7305 \text{ hours}$ (43.83 minutes) each month.

## Job uptime, forward progress

Job uptime and forward progress are very similar to, but not exactly the same as, availability.

**Job uptime** is the fraction of time a job is up and running across nodes. If nodes are available, the job _can_ be accumulating uptime, but the jobs may also be in the process of starting up and not yet running. If a job can restart across all nodes instantaneously as soon as all nodes are up, job uptime is the same as availability.

**Forward progress** is the fraction of time a job is up, running, and doing productive computation. By exclusion, any time the job is up and running but _not_ doing productive computation does not count towards forward progress. This is an important distinction if a job spends a lot of time checkpointing to protect itself against failure; if a job runs for 10 minutes then spends 10 minutes checkpointing, its forward progress is only 50% even though its job uptime is 100%.

Forward progress is a concept closely related to [[JMTTI]] and came out of the nuclear weapons simulation community.[^3]

## Relationship with reliability

If you know the [[MTBF]] and mean time to repair (MTTR), you can calculate the availability of a server:

$\text{Availability} = \frac{\text{MTBF}}{\text{MTBF} + \text{MTTR}}$

You can relate this term directly to [[job reliability]] too. For example, Llama-3 had a JMTTI of 2.5 hours and a job uptime of 90%.[^2] Tweaking the above,

$\text{Availability} = \frac{\text{JMTTI}}{\text{JMTTI} + \text{MTTR}}$

$0.90 = \frac{2.5}{2.5 + \text{MTTR}}$

$\text{MTTR} = \frac{5}{18} = 0.278 \text { hr} = 16.7 \text { min}$

Since [[Meta]] used only 16,000 of the 24,000 GPUs on [[Meta's H100 clusters|their H100 cluster]], this probably reflects a mean time to _restart from checkpoint_ of ~16.7 minutes.

[^1]: [Azure Resiliency Infographic_Final (microsoft.com)](https://azure.microsoft.com/files/Features/Reliability/AzureResiliencyInfographic.pdf?v=95f7f9240e31cb9d723ea0cfdea7864bef338788e9324919e9a93635fb8f64c5)
[^2]: [The Llama-3 Herd of Models (arxiv.org)](https://arxiv.org/abs/2407.21783)
[^3]: Daly, [A higher order estimate of the optimum checkpoint interval for restart dumps](https://doi.org/10.1016/j.future.2004.11.016) (2006)