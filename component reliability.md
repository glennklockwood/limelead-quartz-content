---
title: Component reliability
---
There are many relevant terms and concepts. Here is a brain dump of them:

- [[MTBF|FIT rate]] and AFR - Failure In Time rate and Annual Failure Rate
- [[Weibull distribution]] - A statistical distribution that can fit the rate at which components or systems fail in time.

## In practice

Meta published reliability information from their 16K GPU training run of [[Llama-3.1]] 405b. 466 interrupts occurred over the 54-day training run, resulting in a 90% job uptime.

Of those 466, 47 were planned activities (hardware/firmware updates or changes to training). 419 were unplanned and broken down as follows:[^1]

| Component                      | Category      | Interruption Count | % of Interruptions |
|--------------------------------|---------------|--------------------|--------------------|
| Faulty GPU                     | GPU           | 148                | 30.1%              |
| GPU HBM3 Memory                | GPU           | 72                 | 17.2%              |
| Software Bug                   | Dependency    | 54                 | 12.9%              |
| Network Switch/Cable           | Network       | 35                 | 8.4%               |
| Host Maintenance               | Unplanned Maintenance | 32         | 7.6%               |
| GPU SRAM Memory                | GPU           | 19                 | 4.5%               |
| GPU System Processor           | GPU           | 17                 | 4.1%               |
| NIC                            | Host          | 7                  | 1.7%               |
| NCCL Watchdog Timeouts         | Unknown       | 7                  | 1.7%               |
| Silent Data Corruption         | GPU           | 6                  | 1.4%               |
| GPU Thermal Interface + Sensor | GPU           | 6                  | 1.4%               |
| SSD                            | Host          | 3                  | 0.7%               |
| Power Supply                   | Host          | 3                  | 0.7%               |
| Server Chassis                 | Host          | 2                  | 0.5%               |
| IO Expansion Board             | Host          | 2                  | 0.5%               |
| Dependency                     | Dependency    | 2                  | 0.5%               |
| CPU                            | Host          | 2                  | 0.5%               |
| System Memory                  | Host          | 2                  | 0.5%               |

The specific issue of silent data corruption is also discussed in [[LLM training at scale]].

[^1]: [The Llama-3 Herd of Models (arxiv.org)](https://arxiv.org/abs/2407.21783)