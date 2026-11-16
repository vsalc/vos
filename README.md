# vos

A small operating system kernel for 64-bit RISC-V, written by hand in C. Runs in QEMU.

I am building it to understand how an operating system works underneath: boot, traps, memory, processes, system calls. Every design decision is mine and every line of kernel code is typed by me. xv6 is a reference I read, not a base I copy.

## Status

Milestone 1 of 8, not started. See [docs/MILESTONES.md](docs/MILESTONES.md).

## Layout

- `docs/MILESTONES.md`: the eight milestones from boot to a shell.
- `docs/milestones/`: the to-do breakdown for each milestone.
- `docs/adr/`: decisions and the reasons for them.
- `docs/security/`: one note per milestone on the control it implements and how that control gets attacked.
- `GLOSSARY.md`: the project's own vocabulary.

## How it is built

One to-do, one branch, one pull request. An AI reviewer comments on each pull request. It names problems and never writes the fix. A pull request merges when the build passes and every review thread is resolved.

## After this

A full rewrite in Rust, as a second project, once this kernel runs a shell.
