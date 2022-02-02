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