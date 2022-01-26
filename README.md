# Tarides-internship
Weekly logs of my internship progress

# Week 1
- [ ] MirageOS overview
 - Structure of Mirage
 - Goal of Mirage
 - Difference between Mirage 3.0 and Mirage 4.0 -> v3.0 pulls and builds dependencies for the host machine but we need to build these packages for the target machine. v4.0 keeps these dependencies in a parallel directory and builds them during the `mirage build` phase.
 - Mirage4.0 also integrates dune as the build system. `mirage build` is synonymous to `dune build`. One thing to take note of is that since there are many more packages to be compiled, we can take advantage of the`dune cache` setting so that not all sources are recompiled. Only those with changes.
- [ ] Opam switches
 - opam pin (used for pinning a specific repo to pull opam packages from instead of the default) we need this in order to pull the latest version of Mirage)
- [ ] OCaml Bytecode compilation (ocamlc) vs OCaml native compilation (ocamlopt)
  - When OCaml programs are compiled, the compilation process includes linking in the OCaml runtime which includes the garbage collector as well as the representation of values. One interesting fact is that OCaml values reserve more bits than neccessary to represent the value. This is so that we can use the most significant bit to indicate some piece of information for the garbage collector.
  - The OCaml runtime is written entirely in C and can be found inside the compiler directory under the name of libasm.run (For bytecode) and libasm.opt (For native compilation)?
- ipv6 interface implemented with 6lowpan
- BLE, 802.15.4
- [ ] Setting up and configuring user account on Mirage server
   - useradd
   - userpwd
   - usermod
   - command-line configs taken from mirage user .bashrc file
   - adding ~/.ssh/authorized_keys

# Week 2
- [ ] LWT threads and it's relation to MirageOS
  - LWT is a lightweight threading library that enables concurrency but not true parallelism. This means that whilst we can create concurrent tasks with the LWT library, OCaml does not yet support multicore. Therefore the LWT may appear to be executing cocnurrently because the thread tasks have undeterministic order in being selected to run by the scheduler. However, this is taking place sequentially on only one processor
  - Multicore OCaml aims to be released in Mid-2022 which will mean that Mirage might have to undergo some serious restructuring to take advantage of Multicore
  - OCaml was previously unable to utilize multiple cores because of the "global-lock on values" this was a design choice in order to prevent multiple threads from modifying a value during garbage collection. Hence, once a concurrent garbage collector can be figured out, then Multicore OCaml can be developed.
- [ ] Interfacing C with Ocaml
 - Using "cytpes" and "ctypes-foreign" package
- [x] Dune project template
 - "dune-project" is searched for at the "shallowest" directory (responsible for the project configurations)
 - "dune" file should be present in every directory and contains build rules for that directory. Dune files usually specify if the directory contains a library of code (Accessible by the name of the library with dot notaion) or executables which can be run using dune exec
- [x] Installation of compiler with link time optimization from source
 - The issue with using the opam installed compiler was that the code it was producing was too large to be run on the NRF-board.
 - How to apply git patches
 - Checking md5sum
- [ ] Building OCaml 4.10+lto comopiler with arm-none-eabi-gcc cross compiler (Host:x86-64 to Target:32bit Arm)
- [ ] Interfacing OCaml programs with RIOT OS APIS
