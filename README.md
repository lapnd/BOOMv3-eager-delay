# SECURE-BOOM (Meltdown patch & eager delay Spectre patch)
This repository contains a patched version of the RISC-V core [BOOM (Berkeley Out-of-Order Machine)](https://github.com/riscv-boom/riscv-boom) w.r.t. Meltdown and Spectre vulnerabilities. The patched hardware design covers side-channels introduced by out-of-order execution in user mode and mispredicted speculative instructions in supervisor mode. The security guarantee is valid under the assumption that the operating system run on this hardware is trustful. Classical side-channel attacks visible from an ISA-level point of view were not addressed by the implemented fixes. In addition to the Spectre vulnerability in BOOM, which was already known in advance, a Meltdown vulnerability was discovered in BOOMv3 in the course of this work, which was previously unknown and was only incorporated into the design with the upgrade from version 2 to version 3. Both vulnerabilities were patched iteratively using the UPEC method (see below). Following the first, obvious patches, further remaining security gaps could be detected by the formal proof algorithm. 

The example of the Meltdown vulnerability shows that a formerly secure design can easily be corrupted by minor changes. 
Although microarchitectural optimizations were avoided in BOOM for the address translation of incoming loads, this could not guarantee security in the system. 
UPEC is not only capable of detecting vulnerabilities against transient execution attacks but also helps to find the root cause of the leakage within the hardware design. 

The performance of the revised processor was compared with the performance of the original version using the RISC-V benchmark test suite. 
It turned out that the implemented Meltdown patch did not cause any performance degradation compared to the original version and even the quite conservative approach to fix the Spectre vulnerability ([Eager Delay](https://dl.acm.org/doi/10.1145/3307650.3322216)) only resulted in 14 % performance overhead compared to the vulnerable design.


## The Berkeley Out-of-Order RISC-V Processor

The [Berkeley Out-of-Order Machine (BOOM)](https://github.com/riscv-boom/riscv-boom) is a synthesizable and parameterizable open source RV64GC RISC-V core written in the [Chisel](https://chisel.eecs.berkeley.edu/) hardware construction language.
Created at the University of California, Berkeley in the [Berkeley Architecture Research](https://bar.eecs.berkeley.edu/) group, its focus is to create a high performance, synthesizable, and parameterizable core for architecture research.
The current version of the BOOM microarchitecture ([SonicBOOM, or BOOMv3](https://carrv.github.io/2020/papers/CARRV2020_paper_15_Zhao.pdf)) is performance competitive with commercial high-performance out-of-order cores, achieving 6.2 CoreMarks/MHz.

## Hardware Vulnerabilities

The discovery of transient execution attacks has brought a whole new set of challenges to the field of hardware/software security. 
Detected security vulnerabilities such as Spectre and Meltdown and newly discovered variants thereof differ significantly from previously known microarchitectural side channel attacks. Transient execution attacks exploit incorrect speculative or out-of-order execution that usually would not affect the system state as they are reverted after they turn out to be incorrect. Nevertheless, these instructions leave traces in the caches that can be used to read confidential data. 
By influencing the outcome of a speculation and the transient instructions to be executed, attackers can have indirect access to secret data in a system. 
In contrast to previously known side channel attacks, these transient instructions are not part of the program execution and therefore not visible from an ISA-level point of view which makes it hard to discover possible side channels in the hardware design.


## Unique Program Execution Checking (UPEC)

UPEC (Unique Program Execution Checking) is a formal approach for detecting vulnerabilities to transient execution attacks in out-of-order processors. This technique has been developed by the [Electronic Design Automation Group](https://www.eit.uni-kl.de/eis/research/) at TU Kaiserslautern, Germany.

The verification suite that can be used to verify the BOOM design against transient execution attacks and further information on the functionality can be found [here](https://github.com/TUK-EIS/upec-boom-verification-suite).

A more detailed description of the employed formal verification technique can be found in:

[1] Fadiheh, M. R., Stoffel, D., Barrett, C., Mitra, S., & Kunz, W. (2019, March). Processor hardware security vulnerabilities and their detection by unique program execution checking. In 2019 Design, Automation & Test in Europe Conference & Exhibition (DATE) (pp. 994-999). IEEE.

[2] Rahmani Fadiheh, M., Stoffel, D., Barrett, C., Mitra, S., & Kunz, W. (2018). [Processor Hardware Security Vulnerabilities and their Detection by Unique Program Execution Checking](https://arxiv.org/abs/1812.04975). arXiv e-prints, arXiv-1812. 

[3]Fadiheh, M. R., Müller, J., Brinkmann, R., Mitra, S., Stoffel, D., & Kunz, W. (2020, July). [A formal approach for detecting vulnerabilities to transient execution attacks in out-of-order processors](https://ieeexplore.ieee.org/document/9218572). In 2020 57th ACM/IEEE Design Automation Conference (DAC) (pp. 1-6). IEEE.


## Disclaimer

This reporitory is a work-in-progress and remains in active development.
