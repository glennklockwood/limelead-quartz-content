---
aliases:
  - SMC
title: GSP and SMC
---
# NVIDIA GPU System Processor

The GPU System Processor is NVIDIA's name for the component on each GPU that handles system-level functions such as power management, thermal monitoring, and other housekeeping tasks. `nvidia-smi` talks to this to retrieve its data without disturbing the workload being processed by the GPU.

Some documentation on GSP can be found here: [Chapter 42. GSP Firmware (nvidia.com)](https://download.nvidia.com/XFree86/Linux-x86_64/510.39.01/README/gsp.html)
# AMD System Management Controller

AMD's version of the GSP is called SMC.