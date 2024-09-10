---
title: LPDDR5 Reliability
---
LPDDR5 does not typically support side-band ECC,[^1] so enabling ECC results in a loss of memory capacity and bandwidth.

For example, the [[Grace]] CPU is advertised as coming copackaged with 480 GiB of LPDDR5X with ECC. What's really happening there is:

- There are 512 GiB of physical LPDDR5X
- Inline ECC is implemented as 15+1
- The usable memory is $512 \text{ GiB} \times \frac{15 \text{ bits  (data)}}{15 + 1 \text{ bits (data+parity)}} = 480 \text{ GiB}$

16 bits is the width of LPDDR5,[^2] so the above implies[^3] that ECC is stored inline with data. This can protect individual words against single-bit errors, but:

- This offers no protection against an entire row failing.
- It isn't obvious that this ECC scheme can protect against higher-order failures like entire memory chips going bad.

This latter case is what chipkill exists to protect against.

In principle, this should affect the [[component reliability]] of processors with copackaged LPDDR5.

[^1]: It's unclear if it's not allowed or simply too inefficient for anyone to implement. [What Designers Need to Know About Error Correction Code (ECC) In DDR Memories (semiengineering.com)](https://semiengineering.com/what-designers-need-to-know-about-error-correction-code-ecc-in-ddr-memories/) suggests it's possible but very inefficient.
[^2]: [LPDDR5 Tutorial - Deep dive into its physical structure - systemverilog.io](https://www.systemverilog.io/design/lpddr5-tutorial-physical-structure/)