# Mirage
## Structure of Mirage
## Goal of Mirage
- Difference between Mirage 3.0 and Mirage 4.0 -> v3.0 pulls and builds dependencies for the host machine but we need to build these packages for the target machine. v4.0 keeps these dependencies in a parallel directory and builds them during the `mirage build` phase.
- Mirage4.0 also integrates dune as the build system. `mirage build` is synonymous to `dune build`. One thing to take note of is that since there are many more packages to be compiled, we can take advantage of the`dune cache` setting so that not all sources are recompiled. Only those with changes.

# LWT
LWT is a lightweight threading library that enables concurrency but not true parallelism. This means that whilst we can create concurrent tasks with the LWT library, OCaml does not yet support multicore. Therefore the LWT may appear to be executing cocnurrently because the thread tasks have undeterministic order in being selected to run by the scheduler. However, this is taking place sequentially on only one processor

- Multicore OCaml aims to be released in Mid-2022 which will mean that Mirage might have to undergo some serious restructuring to take advantage of Multicore
- OCaml was previously unable to utilize multiple cores because of the "global-lock on values" this was a design choice in order to prevent multiple threads from modifying a value during garbage collection. Hence, once a concurrent garbage collector can be figured out, then Multicore OCaml can be developed.