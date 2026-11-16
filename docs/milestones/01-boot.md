# Milestone 1: Boot

Done when `make run` starts QEMU and the kernel prints a hello line on the serial console, and CI checks the same thing on every PR.

Each to-do is one PR unless marked otherwise. Do them in order. For any to-do that feels unfamiliar, ask for a handoff and take it to a voice session first.

## 1.0 Toolchain (no PR)

Install the emulator, cross-compiler and debugger:

```
sudo apt install qemu-system-riscv gcc-riscv64-unknown-elf gdb-multiarch
```

Check that `qemu-system-riscv64 --version` and `riscv64-unknown-elf-gcc --version` both answer.

Things to be able to explain afterwards:

- Why the dev box's own `gcc` cannot build this kernel.
- What "unknown-elf" in the compiler name means, compared to "linux-gnu".

## 1.1 Learn the machine and choose the boot path (PR: one ADR)

Nothing to build yet. Understand the ground the kernel stands on, then make the first design decision.

Find out:

- What the QEMU `virt` machine is and what devices it has. QEMU can dump its device tree. Work out how, and read it.
- Where RAM starts on `virt`, and where the serial port (UART) lives.
- The three RISC-V privilege modes: machine, supervisor, user. What each may do.
- What a hart is.
- What OpenSBI is, what mode it runs in, and what state it leaves the CPU in when it jumps to a kernel.

Decision: does vos let OpenSBI boot first and start in supervisor mode, or start with no firmware in machine mode and do that work itself? Write the choice and the reason as `docs/adr/0003-...`. Read how xv6 does it and how Linux does it before choosing. They differ.

Read: RISC-V privileged spec (machine and supervisor chapters, skim), the xv6 book chapter on boot, the OpenSBI README.

## 1.2 The smallest kernel that reaches C

Get from the first instruction to a C function that loops forever. It prints nothing. Prove it works with the debugger.

Pieces, all written by hand:

- A linker script. It decides the address the kernel is loaded at, the order of sections, and where the boot stack is.
- An entry routine in assembly. C needs a few things to be true before the first C function can run. Work out what they are and set them up.
- A C function that never returns.
- A Makefile with a build target, a `run` target that starts QEMU, and a `debug` target that starts QEMU paused and waiting for gdb.

Check: attach `gdb-multiarch`, set a breakpoint on the C function, and see it hit. Look at the stack pointer and confirm it points where the linker script says.

Things to be able to explain:

- What `.text`, `.rodata`, `.data` and `.bss` each hold, and why `.bss` needs special treatment at boot.
- What flags tell the compiler there is no operating system and no standard library underneath, and why each is needed.
- What happens if the C function returns.

## 1.3 Hello over the serial port

Make the kernel print one line. How it reaches the serial port depends on the 1.1 decision: either talk to the UART hardware directly or ask the firmware to print.

Check: `make run` shows the line. Then the kernel parks the CPU without spinning at 100 percent. Find the instruction for that.

Things to be able to explain:

- What memory-mapped I/O is, and why a plain C pointer write can reach a device.
- Why that pointer needs `volatile`.

## 1.4 CI

Write `.github/workflows/ci.yml` by hand. It installs the toolchain, builds the kernel, boots it in QEMU, and fails unless the hello line appears. QEMU never exits by itself here, so the job needs a way to stop and still judge the output.

This PR touches `.github/`, so review it carefully yourself. The AI reviewer comments on it like any other PR.

## 1.5 Turn on the merge gate (settings, no PR)

Now that a CI check exists:

- Protect `main`: require a pull request, require the CI check, require conversation resolution.
- From here on, open each PR with auto-merge on: `gh pr merge --auto --squash`.
- First confirm that GitHub, Settings, Emails has "Keep my email addresses private" on. Auto-merge commits use the account email, and this keeps it the noreply address.

Ask Claude for the exact protection command when you get here.

## 1.6 Security lens (PR: one note)

Write `docs/security/01-boot.md` in your own words, after talking it through. Questions to answer:

- Which code ran before yours, at what privilege, and why must the kernel trust it?
- What could a malicious or buggy version of that code do that the kernel could never detect?
- What do real systems do about this (secure boot, measured boot), and which control families in a framework such as NIST 800-53 does that map to?
