---
title: Availability
---
Availability is the fraction of a supercomputer's nodes that are online and useable by jobs in a given interval of time.

## Quantitative definition

SLAs are often expressed as average monthly availability; for example, Azure offers a 99.9% ("three nines" or "3x9") availability for a non-redundant VM on a monthly basis.[^1] If the average month has 730.5 hours, this means that VM will only be down for 0.1% of that, or $730.5 \text{ hours} \times (1 - 0.999) = 0.7305 \text{ hours}$ (43.83 minutes) each month.

[^1]: [Azure Resiliency Infographic_Final (microsoft.com)](https://azure.microsoft.com/files/Features/Reliability/AzureResiliencyInfographic.pdf?v=95f7f9240e31cb9d723ea0cfdea7864bef338788e9324919e9a93635fb8f64c5)