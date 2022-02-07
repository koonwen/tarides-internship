# 31/01 Monday
- Learn about how object code is linked
- Start to understand the build pipeline of the project

## Process
1. Get build system `$ git clone https://github.com/TheLortex/ocaml-riot-unix`
2. Get RIOT repo `$ git clone https://github.com/RIOT-OS/RIOT`
3. Get 32bit libraries `$ sudo dnf install glibc-devel.i686`
4. Change `RIOTBASE ?= path/to/RIOT/repo` appropriately
5. Build RIOT_BUILD.h file `$ make (absolute path to RIOT_BUILD.h)`
6. Build the runtime `$** make runtime`
7. Build the project `$ make`

## Blockers
- Could not figure out why the dependency resolution was not working for the RIOT_BUILD.h header file

# 01/02 Tuesday
- Learn about how make rules are evaluated and C programs are built
- Debug the build issue

## Process
1. Reordered the Makefile rules so that RIOT's build system would run first. (This did not work because we need our C program to be linked with our runtime first before passing it to RIOT)
2. Renamed the path variable to locate the RIOT_BUILD.h dependency correctly

## Blockers
- Lacking understanding in Makefile syntax and evaluation order

# 02/02 Wednesday
- Work through the build system rules
- Figure out how to write RIOT API's to OCaml
- Continue working on understanding how to embed OCaml in C code

## Process
1. Played around with `ocamlc` with different flags `-output-obj -output-complete-obj -custom`
2. Working process is:
```sh
ocamlc -output-obj-o <output>.o input.ml [.cmx .cmi]
ocamlc -o <executable> <object_files>.o -L`ocamlc -where` -lcamlrun -lm
```
3. For implementing stubs.c, we need to include some OCaml directives that will help us when translating OCaml types to and from C types
4. We can streamline the build with dune using the `foreign_stubs (language c) (names ...)` fields

## Blockers
- Was initially unable to identify where the math header file was located for the runtime primitives. The file name is just `m`
- Even after successful compilation, I was unable to see my prints, it was later realised that unlike in pure OCaml, we need to be a little more manual with our programs including flushing the stdout buffer.

# 03/02 Thursday
- Struggle with the build system to figure out the neccessary flags to get RIOT to successfully compile our program/runtime
- Start looking into RIOT API's that we can wrap and provide in OCaml

## Process
1. Started by working through some simple compilation examples to provide callable C library functions in OCaml:
    1. Manual compilation with `ocamlc -custom`, producing the runtime appended to the program (issues: means that we need to use `ocamlc` for the final linking step, however we want to pass off our code to RIOT, therefore we have to statically link in the runtime)
    2. Performing static linking by producing the runtime object/c code with `ocamlc -output-obj` and then linking by passing the link flags `cc -'Locamlc -where' -lcamlrun -lm`
    3. Automating (2) with dune
2. Tinker with build rules to try and get RIOT to compile output files created by `dune build`
3. Started looking into RIOT API's that we will need to implement that base of translation layer

## Blockers
- The build rules to pass to RIOT is much more complicated than expected. I still do not understand what needs to be configured in the Makefile to help RIOT compile our runtime correctly with the startup.c file.

# 04/02 Friday
- Looked more deeply into what the OCaml runtime consists of
- Clarified the build pipeline with Lucas
- Understand more clearly the role and usefulness of Mirage

## Process
1. played around with different compilation flags to compile OCaml programs in more discrete steps to understand how RIOT the runtime is linked in.
2. Go through with Lucas questions I had pertaining to the the runtime, the build pipeline and Mirage
3. Started looking into how an event-loop algorithm works to start implementing the run and sleep functions for Mirage's interface.

## Blockers
- Don't have a good understanding of how the event-loop algorithm works
- Don't have a clear understanding on how to incorporate LWT into the run and sleep functions.