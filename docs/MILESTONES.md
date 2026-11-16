# Milestones

Eight milestones from power-on to a shell. Each one boots and shows something new. Traps come before memory so that every later crash prints a useful report.

The internal design of each milestone is decided when it is reached, after the topic is learned. Nothing below fixes a design.

| # | Milestone | Done when |
|---|-----------|-----------|
| 1 | Boot | `make run` starts QEMU and the kernel prints a hello line on the serial console. CI does the same. |
| 2 | Console and printf | The kernel prints formatted text and numbers, reads typed characters, and has a `panic` that stops with a message. |
| 3 | Traps and timer | A deliberate fault prints what happened and where. A timer interrupt ticks on screen. |
| 4 | Physical memory | The kernel hands out and takes back pages of RAM, and a self-test proves it. |
| 5 | Virtual memory | The kernel runs with paging on, under its own page table. A bad address faults cleanly. |
| 6 | Processes | Several kernel threads take turns, switched by the timer. |
| 7 | User mode and system calls | A program runs with user privilege, asks the kernel for services, and cannot touch kernel memory. |
| 8 | Programs and shell | The kernel loads separate programs and a small shell starts them by name. |

After milestone 8: decide whether to extend (filesystem, pipes, multicore) and start the Rust rewrite as a second project.

## To-do breakdowns

- [Milestone 1: Boot](milestones/01-boot.md)
