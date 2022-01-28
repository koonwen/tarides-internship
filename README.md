# Tarides-internship
Weekly logs of my internship progress

# Topics
- [Mirage](./MIRAGE.md)
- [Riot OS](./RIOT_OS.md)
- [OCaml's Compilers](./OCAML.md)
- [Dune & Opam](./DUNE_OPAM.md)
- [Git](./GIT.md)

# Legend
Each week summarizes what I spent my time on. I have tagged them according to a scale on how much effort I spent on each topic.

1. `Overview` : brief amount of time spent on trying to grasp the big picture of the topic. Usually includes quick google searches or short videos explaining the topic
2. `Tinkering` : I spent a moderate amount of time on the topic. This is usually when I am playing with a tool or a command to figure out how I can use it.
3. `Understanding` : I dedicated a sizeable amount of time researching and trying to grasp the topic. Includes more careful reading of documentation and generally applies to figuring out how a tool works.
4. `Learning` : I spent a significant amount of time on this topic because a strong understanding is neccessary in helping me to work on the project. Usually entails working through tutorials and protoyping.
5. `Implementing` : Lot's of hours spent. Highly likely that it is the main project

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

# Dialogue
The goal of the internship is to be able to create a process/system that allows us to take high-level OCaml code and be able to compile it down to run on resource constraint IOT devices.

Working off of the preious intern (Ben's) work, he figured out that the main issue with our methodology, is that the OCaml code compilation pipeline produces code that is far to large (memory wise) to fit on our target devices. In particular, it is the OCaml runtime that takes up a lot of memory because the runtime includes a lot of C primatives that aren't used.

In the **Week 1** of the internship, time was spent on setting up my development environment, getting used to the tools and learning about the Mirage ecosystem.

To get up to speed with what Ben has worked on, For **Week 2**, as an exercise in cross compilation, I tried to build an OCaml compiler with my host system (Intel x86-64) to targetting to build for (Arm 32bit architecture). The end goal here was to successfully perform a cross compilation and be able to run my OCaml program on our target device. Knowing that code produced by our OCaml compiler will be far too large, Lucas suggests to try using the OCaml 4.10.0+lto compiler (The latest compiler with link time optimization). In parallel with building the cross-compiler, the second exercise is to play with the RIOT-OS interface and see if we can successfully link and build an OCaml program with the RIOT build system. The approach is here is essentially what Ben has been working on (skipping the step and omit performing ocamlclean). We use the OCaml bytecode compiler to generate our program and the OCaml runtime as C code. By tweaking the RIOT makefile, we can link in the runtime and allow RIOT's build system to take over.
