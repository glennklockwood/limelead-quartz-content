---
title: Job Mean Time To Interrupt
aliases:
  - MTBAI
  - MTTAI
  - MTBAF
---
# Job Mean Time to Interrupt

JMTTI is a metric that describes how long a large-scale parallel simulation can run before something happens that would cause that job to crash. To the best of my knowledge, it came out of the US nuclear weapons complex[^1] because they are one of the few HPC use cases today that run full-system simulations that must run continuously for days or weeks and therefore be affected by the frequency of failures at scale.

I like it as a [[reliability]] metric since it reflects the rate at which an HPC user has to touch the system because the job got interrupted. In turn, a low JMTTI will result in high user pain and annoyance with a supercomputer.

OLCF seems to refer to this as MTBAF (Mean Time Between Application Failure), Mean Time Between Application Interrupt (MTBAI), and Mean Time To Application Interrupt (MTTAI).[^2]

## Factors

JMTTI increases with system complexity because most/all HPC and AI use nodes connected in series; if one node is interrupted by an error, the whole job is interrupted. This means JMTTI often decreases linearly with node count.

Above certain scales, JMTTI is nonlinear with node count because of nonlinear components in the system like fabrics. For example, the number of spine switches and inter-switch links on a backend fabric increases nonlinearly with node count, resulting in a superlinear increase in JMTTI.

## Similar concepts

JMTTI is different from [[MTBF]] because not all component failures cause job interrupts, and not all job interrupts are caused by component failures.

[^1]: Daly, [A higher order estimate of the optimum checkpoint interval for restart dumps](https://doi.org/10.1016/j.future.2004.11.016) (2006)
[^2]: [Microsoft Word - OLCF-6 RFP Requirements (ornl.gov)](https://www.olcf.ornl.gov/wp-content/uploads/Attachment-C-OLCF-6-Technical-Requirements-Document-TRD.pdf)