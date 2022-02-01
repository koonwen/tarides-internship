# Tarides-internship
Weekly logs of my internship progress

# Topics
- [Mirage](./MIRAGE.md)
- [RIOT OS](./RIOT_OS.md)
- [Compilers and OCaml](./COMPILERS.md)
- [Dune & Opam](./DUNE_OPAM.md)
- [Linux & Git](./LINUX_GIT.md)

# Legend
Each week summarizes what I spent my time on. I have tagged them according to a scale on how much effort I spent on each topic.

1. `Overview` : brief amount of time spent on trying to grasp the big picture of the topic. Usually includes quick google searches or short videos explaining the topic
2. `Tinkering` : I spent a moderate amount of time on the topic. This is usually when I am playing with a tool or a command to figure out how I can use it.
3. `Understanding` : I dedicated a sizeable amount of time researching and trying to grasp the topic. Includes more careful reading of documentation and generally applies to figuring out how a tool works.
4. `Learning` : I spent a significant amount of time on this topic because a strong understanding is neccessary in helping me to work on the project. Usually entails working through tutorials and protoyping.
5. `Implementing` : Lot's of hours spent. Highly likely that it is part of the main project

# Week 1
- [x] `Overview` of networking protocols: IPV6 interface implemented with 6lowpan, BLE, 802.15.4
- [x] `Tinkering` with Opam switches
- [x] `Tinkering` with Sysadmin tricks
- [x] `Understanding` OCaml Bytecode compilation (ocamlc) vs OCaml native compilation (ocamlopt)
- [x] `Learning` of MirageOS

# Week 2
- [x] `Overview` of LWT threads and it's relation to MirageOS
- [x] `Tinkering` with Git patching tricks
- [x] `Understanding` the dune build system
- [x] `Understanding` how to install and build a compiler with link time optimization from source
- [x] `Learning` how to interfacing C with Ocaml
- [ ] `Learning` Building OCaml 4.10+lto comopiler with arm-none-eabi-gcc cross compiler (Host:x86-64 to Target:32bit Arm)
- [ ] `Learning` Interfacing OCaml programs with RIOT OS APIS

# Week 3
- [x] `Tinkering` with Makefiles
- [x] `Understanding` the C compilation process
- [x] `Learning` the [Dune/RIOT build system](https://github.com/TheLortex/ocaml-riot-unix)
- [ ] `Learning` how to extend RIOT APIs in OCaml

# Dialogue
The goal of the internship is to be able to create a process/system that allows us to take high-level OCaml code and be able to compile it down to run on resource constraint IOT devices.

Working off of the preious intern (Ben's) work, he figured out that the main issue with our methodology, is that the OCaml code compilation pipeline produces code that is far to large (memory wise) to fit on our target devices. In particular, it is the OCaml runtime that takes up a lot of memory because the runtime includes a lot of C primatives that aren't used.

In the **Week 1** of the internship, time was spent on setting up my development environment, getting used to the tools and learning about the Mirage ecosystem.

To get up to speed with what Ben has worked on, For **Week 2**, as an exercise in cross compilation, I tried to build an OCaml compiler with my host system (Intel x86-64) to targetting to build for (Arm 32bit architecture). The end goal here was to successfully perform a cross compilation and be able to run my OCaml program on our target device. Knowing that code produced by our OCaml compiler will be far too large, Lucas suggests to try using the OCaml 4.10.0+lto compiler (The latest compiler with link time optimization). In parallel with building the cross-compiler, the second exercise is to play with the RIOT-OS interface and see if we can successfully link and build an OCaml program with the RIOT build system. The approach is here is essentially what Ben has been working on (skipping the step and omit performing ocamlclean). We use the OCaml bytecode compiler to generate our program and the OCaml runtime as C code. By tweaking the RIOT makefile, we can link in the runtime and allow RIOT's build system to take over.

For **Week 3**, Lucas has set up the build system which is a mixture of Dune and Make build rules to successfully compile our OCaml code. The pipeline is as such: 
1. The "example" directory is meant to be an isolated working space for us to develop in OCaml. The Dune build system is first to execute and compiles our OCaml code along with the runtime into bytecode. The bytecode is then cleaned and then converted into C code. The choice of using Dune for the first part of the pipeline is to maintain the developer workflow of using Dune in OCaml projects
2. Subsequently, the Makefile at the top level of the project is responsible for setting up the environment such that RIOT can take over the build. This setup includes moving both the runtime library (libcamlrun.a) and the source `main.c` file to the top level where RIOT expects it
3. Finally our Makefile "includes" the RIOT build system which compiles our program with the relevant C Flags.

with this test bed in place, we can start to writing APIs that interface RIOT APIs as OCaml APIs. Currently, the problem is that we want to define our C-stubs separately from our startup.c file and link in during build time. However, we rely on dune to do the build
which does not currently have RIOT's flags to compile correctly.

***Digression**: One initially peculiar thing to me was how we could successfully compile an OCaml printf. Wasn't the whole point of this project to fulfill the gap to provide OCaml code APIs to interact with RIOT OS? The reason for this oddity is because the `caml_functions` rely on newlibc packages' libc.a(static library). The library provides all the symbols that our runtime looks for during compilation. Furthermore, newlibc's symbols then rely on RIOT's system call library which is also fully provided because newlibc is designed for embedded devices
and therefore has a somewhat minimal library.

