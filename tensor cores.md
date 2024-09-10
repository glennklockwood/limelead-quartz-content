---
aliases:
  - matrix cores
  - tensor core
  - matrix core
title: Tensor cores and Matrix cores
---
- **Tensor core** is NVIDIA's name
- **Matrix core** is AMD's name

## Basic functionality

Tensor Cores and Matrix Cores implement $D = A * B + C$ operations in a single clock cycle, where
- $A$ is a $m \times k$ matrix
- $B$ is a $k \times n$ matrix
- $C$ and $D$ are $m \times n$ matrices

AMD and NVIDIA describe MMA capabilities as supporting $m \times n \times k$ operations.

## History

NVIDIA V100 (1st generation Tensor Core)
- $A$, $B$ are FP16
- $C$, $D$ are FP16 or FP32
- support 4x4x4 matrices (FP16)
	- HMMA: 128 FP16 FLOPS (64 FP16 FMAs) per clock per Tensor Core
- 8 tensor cores per [[GPU terminology decoder ring|SM]]
- 80 SMs per GPU

NVIDIA A100 (3rd generation Tensor Core)[^3]
- $A$, $B$ are binary, INT4, INT8, FP16, BF16, TF32, and FP64
- supports 8x4x8 matrices (FP16)
	- HMMA: 512 FP16 FLOPS (256 FP16 FMAs) per clock per Tensor Core
- supports 2x2x4 matrices (FP64)
	- DMMA: 32 FP64 FLOPS (16 FP64 FMAs) per clock per Tensor Core
- 4 tensor cores per [[GPU terminology decoder ring|SM]]
- 108 SMs per GPU

NVIDIA [[H100]] (4th generation Tensor Core)[^4]
- $A$, $B$ are FP8 (E4M3 or E5M2), FP16, BF16, TF32, FP64, and INT8
- supports 8x4x16 matrices (FP16)
	- HMMA: 1024 FP16 FLOPS (512 FP16 FMAs) per clock per Tensor Core
- supports 4x2x4 matrices (FP64)
	- DMMA: 64 FP64 FLOPS (32 FP64 FMAs) per clock per Tensor Core
- 4 tensor cores per [[GPU terminology decoder ring|SM]]
- 132 SMs per GPU

AMD MI250X (CDNA2)
- 4 Matrix Cores per [[GPU terminology decoder ring|CU]]
- 220 CUs per GPU (110 per GCD)

AMD [[MI300X]] (CDNA3)
- 4 Matrix Cores per [[GPU terminology decoder ring|CU]]
- 304 CUs per GPU

NVIDIA describes their Tensor Core geometry using these funny diagrams from which you must extract the hardware capability of each Tensor Core generation. Their SDK documentation only includes the logical matrix sizes supported;[^5] it appears the CUDA runtime maps these to the hardware capabilities.

![[NVIDIA Tensor Core diagram.png]]

Simiarly the AMD SDK for Matrix Cores abstracts the underlying hardware dimensions.[^6]

## FLOPS per MMA

Per ChatGPT 4o:

To calculate the expression $A \times B + C$ where $A$, $B$, and $C$ are all $4 \times 4$ matrices, you need to follow these steps:

1.	Matrix Multiplication $A \times B$ requires $k$ multiplications and $(k - 1)$ additions per element
	- Therefore, for each element: $k$ multiplications and $(k-1)$ additions, or $(2k -1)$ FLOPS
	- Total elements: $4 \times 4 = 16$
	- Total floating-point operations for multiplication: $16 \times 7 = 112$
2.	Matrix Addition $(A \times B) + C$ requires 1 addition per element
	- Each element of the resulting $4 \times 4$ matrix requires 1 addition
	- Total elements: $4 \times 4 = 16$
	- Total floating-point operations for addition: 16

Adding the operations from both steps gives
- Total FLOPS per GEMM $= m \times n \times 2k$
- So for $m \times n \times k = 4 \times 4 \times 4 = 128$ FLOPS per MMA

[^1]: [Programming Tensor Cores in CUDA 9](https://developer.nvidia.com/blog/programming-tensor-cores-cuda-9/ ) is NVIDIA's blog post that describes the Tensor Core API.
[^2]: [NVIDIA Tensor Core Programming - Lei Mao's Log Book](https://leimao.github.io/blog/NVIDIA-Tensor-Core-Programming/) is a really good description of the Tensor Core API including IMMA instructions.
[^3]: [NVIDIA A100 GPU Architecture Overview](https://images.nvidia.com/aem-dam/en-zz/Solutions/data-center/nvidia-ampere-architecture-whitepaper.pdf)
[^4]: [NVIDIA H100 Tensor Core GPU Architecture Overview](https://resources.nvidia.com/en-us-tensor-core?ncid=no-ncid)
[^5]: [CUDA C++ Programming Guide (nvidia.com)](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#wmma-type-sizes)
[^6]: [AMD matrix cores (amd-lab-notes) - AMD GPUOpen](https://gpuopen.com/learn/amd-lab-notes/amd-lab-notes-matrix-cores-README/#mfma-compiler-intrinsic-syntax)