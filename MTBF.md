---
title: MTBF, FIT, and AFR
aliases:
  - FIT rate
  - survival function
---
MTBF, FIT, and AFR are closely related concepts that describe [[component reliability]]. In brief,

$\text{MTBF} \propto \frac{1}{\text{FIT}} \propto \frac{1}{\text{AFR}}$

## MTBF

Mean Time Between Failure (MTBF) refers to the mean time between failures of a single component or a system of components. Pedantically, a failure is something which requires replacement (like a GPU burnt up). Its units are _time_ (hours, days, months, years) and is calculated simply as:

$\text{MTBF} = \frac{\text{time spent observing}}{\text{\# observed failures}}$

If you had to send five nodes back for replacement in the past six months, your MTBF is

$\text{MTBF} = \frac{6 \text{ months}}{5 \text{ failures}} = 1.2 \text{ months}$

If you know the [[MTBF#Survival function|survival function]] of a component $R(t)$, you can also express MTBF in terms of that:

$\text{MTBF} = \int_0^{\infty} R(t) dt$

## FIT

FIT (failures in time) rate is a closely related concept and is the inverse of the MTBF per billion hours in service:

$\text{FIT rate} = \frac{10^9 hours}{\text{MTBF}_{\text{hours}}}$

It is a unitless quantity because it represents a number of failures.

Hardware vendors often express component [[reliability]] in terms of their FIT rate.

## Failure rate ($\lambda$)

Failure rate is the frequency with which a component fails and is measured in units of failures per unit time. If the unit time is $10^9$ hours, it is the same as the FIT rate.

As with FIT rate, it is inversely related to MTBF:

$\lambda = \frac{1}{\text{MTBF}}$

## Survival function

Survival function (or _reliability function_) describes the probability that a component will survive for at least a certain amount of time. It can be inferred if you know the failure rate $\lambda$ from above.

Survival function $R(t) = e^{- \lambda t}$

Recall that the failure rate $\lambda$ is inversely related to MTBF, you then get:

$R(t) = e^{- \frac{t}{\text{MTBF}}}$

Or maybe more meaningfully, the probability that a component will fail within a certain amount of time:

$F(t) = 1 - e^{- \frac{t}{\text{MTBF}}}$

## AFR

AFR (annualized failure rate) seems to have two definitions:

1. The intuitive one, which is the failure rate $\lambda$ normalized to a year
2. The [Wikipedia definition](https://en.wikipedia.org/wiki/Annualized_failure_rate) which is the probability that a component will fail in a year

The big difference is that #1 can be above 100% (more than one component fails per year, or a component fails multiple times per year), but #2 approaches 100% asymptotically.

### Intuitive (failures per year)

The intuitive AFR, or the frequency of component failure per year, is easy to define assuming a year is 8,766 hours:

$\text{AFR} = \frac{8766 \text{ hours}}{\text{MTBF}_{\text{hours}}}$

It is unitless because it is a percentage.

### Wikipedia (probability of failure)

The Wikipedia definition is really just the survival function from above. Recall:

$F(t) = 1 - e^{- \frac{t}{\text{MTBF}}}$

If you use $t = 8766$ hours per year, you get

$\text{AFR} = 1 - e^{- \frac{8766 \text{ hours}}{\text{MTBF}_\text{hours}}}$

### Further confusion

It doesn't help that $\frac{1}{x}$ is often used as an approximation of $1 - e^{-\frac{1}{x}}$. This results in some sources claiming that the intuitive definition is just an approximation of the other definition, but this is not true.

## Predicting MTBF, FIT, and AFR

A nice property of MTBF and FIT is that, for a system of components connected in series, you can add up FIT/MTBF rates for all components to get a FIT/MTBF for the total system.

### Example: Calculating node reliability

Let's say you have a node that is comprised of the following parts (FIT values made up by ChatGPT):

| Component     | Component FIT ($C_i$) | Qty per node ($N_i$) | Total FIT |
| ------------- | --------------------- | -------------------- | --------- |
| CPU + DRAM    | 1,000                 | 2                    | 2,000     |
| GPU + HBM     | 1,500                 | 8                    | 12,000    |
| NIC           | 300                   | 8                    | 2,400     |
| Transceiver   | 100                   | 8                    | 800       |
| BMC           | 500                   | 1                    | 500       |
| SSD           | 1,000                 | 8                    | 8,000     |
| SSD backplane | 200                   | 1                    | 200       |
| Power supply  | 2,000                 | 8                    | 16,000    |

The FIT for the whole node is the sum of all "Total FIT" values which is just the sum of FIT rates for every component:

$\text{Node FIT} = \sum_{i}^{\text{components}} N_i C_i = (1000 \cdot 2) + (1500 \cdot 8) + ... = 41,900$

This is valid because every component is connected in series; the failure of one component causes the whole node to fail.

> [!warning] 
> The above is not true if components are redundant; for example, the above assumes all eight power supplies are active/active with no redundancy. This is _never_ true in practice; you would calculate an aggregate FIT for 6+2 power supplies and use that above.

In the above example, the node FIT is 41,900. You can then calculate:

$\text{AFR} = \frac{\text{FIT} \times 8766}{10^9} = 36.7\%$

and

$\text{MTBF} = \frac{1}{\text{AFR}} = \frac{1 \text{ year}}{0.367} = 2.72 \text{ years} = 23,800 \text{ hours}$

### Example: Calculating cluster MTBF

Let's say you have a cluster of 1,024 of these nodes. Calculating the FIT rate of the whole cluster is as simple as connecting all nodes in series:

$\text{Cluster AFR} = \text{\# nodes} \times \text{Node FIT} = 1024 \times 41,900 = 42,905,600$

This means

$\text{AFR} = \frac{\text{FIT} \times 8766}{10^9} = 37,611\%$

Or conversely, the cluster will fail 376 times every year on average. This is better expressed as MTBF:

$\text{MTBF} = \frac{1}{\text{AFR}} = \frac{1 \text{ year}}{376.11} = 0.002659 \text{ years} = 23.3 \text{ hours}$

## Relationship to JMTTI

Assuming an MPI job runs across all these nodes ($\therefore$ one node failing = whole job failing), you _can_ claim that the [[JMTTI]] would be 23.3 hours as well. However, there are more things that can _interrupt_ a job than component failures--link flaps, kernel panics, cosmic rays, and the like. This will almost always make [[JMTTI]] lower than MTBF.

## In practice

There's a table in [[job reliability#In practice]] that contains a few MTBF measurements from large-scale supercomputers.