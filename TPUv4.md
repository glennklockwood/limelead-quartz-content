---
tags:
  - GPU
title: Google TPUv4
---
See Google's TPUv4 documentation.[^1] In brief, each one has:

- 2 TensorCores (not to be confused with [[GPU terminology decoder ring|NVIDIA's terminology]])
	- 8 MXUs (2 per TensorCores)
	- 2 vector units (1 per TensorCore)
	- 2 scalar units (1 per TensorCore)
	- No sparsity
- 32 GB HBM2 (? stacks)
	- 1.2 TB/s (max)
- x16 PCIe Gen3
- 192 W maximum

## Performance

The following are theoretical maximum performance in TFLOPS:[^1]

| Data Type | VFMA | Matrix | Sparse |
| --------- | ---- | ------ | ------ |
| FP64      |      |        |        |
| FP32      |      |        |        |
| TF32      |      |        |        |
| FP16      |      |        |        |
| BF16      |      | 275    |        |
| FP8       |      |        |        |
| INT32     |      |        |        |
| INT8      |      | 275    |        |
[^1]: [TPU v4 (cloud.google.com)](https://cloud.google.com/tpu/docs/v4)