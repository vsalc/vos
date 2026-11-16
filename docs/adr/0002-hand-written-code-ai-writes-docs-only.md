# Victor writes all code, AI writes docs only

The point of vos is understanding, so everything that builds or runs is typed by Victor: kernel C, assembly, the linker script, the Makefile, tests and the CI workflow. AI writes the to-do breakdowns, the glossary, ADRs, and the review workflow file. It teaches and reviews, and its review comments name the problem without writing the fix.

The design is Victor's own. xv6 is read as a reference kernel and never copied. Design decisions are made one milestone at a time, after the topic is learned, not up front.

## Consequences

- Progress is slower than agent-written code would be. That is accepted.
- Tests reveal design, so they are hand-written too.
- Every PR is still reviewed by a second agent, and merges automatically once CI is green and every review thread is resolved.
