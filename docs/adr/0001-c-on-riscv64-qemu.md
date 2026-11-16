# C on 64-bit RISC-V, run in QEMU

The kernel is written in C and targets 64-bit RISC-V on the QEMU `virt` machine. C because the operating systems course and nearly every kernel reference use it, so only one hard thing is being learned at a time. RISC-V because the spec is small and boot is short, which gets to traps, memory and scheduling quickly. QEMU because the project is software only.

## Considered options

- Rust: better long-term bet, but learning ownership rules and paging together slows both. It becomes project two: a full rewrite once this kernel runs a shell.
- x86-64: matches real machines, but the legacy boot steps teach history, not operating systems.
- ARM 64-bit: could run on a real Raspberry Pi, but has fewer beginner references and a fiddlier boot.
- Go: the original plan. Dropped when the project became a kernel.
