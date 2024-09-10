---
aliases:
  - Data Center GPU Max
  - Ponte Vecchio
title: Intel Ponte Vecchio
tags:
  - GPU
---
There are two SKUs:[^2]

- Intel Data Center GPU Max 1100 (56 Xe Cores)
- Intel Data Center GPU Max 1550 (128 Xe Cores)

## Specifications

Each Intel Data Center GPU Max 1550 has:

- 2 Xe Stacks
	- 128 Xe Cores (64 per Stack)
		- 1024 Xe Vector Engines (8 per Xe Core)
		- 1024 Xe Matrix Engines (8 per Xe Core)
	- 900 MHz (base), 1.6 GHz (peak)
	- No sparsity
- 128 GB HBM2e
	- 3.2768 TB/s[^3]
- 16 Xe Links (D2D)
- 1x16 PCIe Gen5 or CXL 1.1 (H2D)
- 600 W maximum

## Performance

The following are measured values from preproduction [[Aurora]].[^4] These are measured values using DGEMM, and although it seems like they should make use of the Xe Matrix Engines (since the benchmarks simply calls into `oneapi::mkl::blas::column_major::gemm`[^5]), it's unclear how much vector FMA vs. matrix operations are being conducted here. For example, the FP64 matrix peak performance should be somewhere between

$1024 \text{ Xe Vector Engines} \times 512 \text{ bits/Engine} / 64 \text{ bits} \times 900 \text{ MHz} \times 2 \text{ ops/clock} = 14.7 \text{ TFLOPS}$

and

$1024 \text{ Xe Matrix Engines} \times 4096 \text{ bits/Engine} \times \frac{1}{64 \text{ bits}} \times 1600 \text{ MHz} \times 2 \text{ ops/clock} = 210 \text{ TFLOPS}$

The measured 17 TFLOPS FP64 is far closer to the VFMA peak than the Matrix peak, yet it is higher than VFMA peak. Perhaps this indicates it's all VFMA but it runs at higher-than-base frequency?

| Data Type | VFMA | Matrix | Sparse |
| --------- | ---- | ------ | ------ |
| FP64      | 17   |        |        |
| FP32      | 23   |        |        |
| TF32      | 110  |        |        |
| FP16      | 263  |        |        |
| BF16      | 273  |        |        |
| FP8       |      |        |        |
| INT32     |      |        |        |
| INT8      | 577  |        |        |


## Nomenclature

The Intel terminology is confusing. According to James Brodman (Intel):[^1]

- 1 GPU = 2 stacks
- 1 stack = 4 slices + 4 HBM2e controllers + 8 Xe Links[^2]
- 1 slice = 16 cores
- 1 core = 8 vector engine + 8 matrix engines
- 1 vector engine = 512 bits
- 1 matrix engine = 4096 bits

Also,

- Stacks used to be called tiles
- Vector Engines used to be called execution units (EUs)

[^1]: [Unified Tensor Interface in DPC++ (iwocl.org)](https://www.iwocl.org/wp-content/uploads/9394-James-Brodman-Intel.pdf)
[^2]: [GPU Optimization Thread Mapping Occupancy (anl.gov)](https://www.alcf.anl.gov/sites/default/files/2023-11/GPU_Optimization_Thread_Mapping_Occupancy.pdf)
[^3]: https://ark.intel.com/content/www/us/en/ark/products/232873/intel-data-center-gpu-max-1550.html
[^4]: https://docs.alcf.anl.gov/aurora/node-performance-overview/node-performance-overview/
[^5]: https://github.com/argonne-lcf/user-guides/blob/main/docs/aurora/node-performance-overview/src/gemm.cpp#L65C5-L65C42