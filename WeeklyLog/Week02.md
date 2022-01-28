# 27/01 Thursday
- Familiarize with Dune and Opam
- Figuring out how to interface OCaml code with RIOT OS

## Process
1. Install and built RIOT repository for native compilation
2. Created Makefile that calls RIOT's "MAKEFILE.include" to build with RIOT
3. Created test "helloworld".ml file and compiled with bytecode compiler with
    `$ ocamlc -output-complete-obj -o ocaml.c ocaml.ml`
4. Tried to compile with RIOT with
    `$ CFLAGS=-I/home/koonwen/.opam/riot/lib/ocaml make QUIET=0 WERROR=0`


## Blockers
- Could not figure out the appropriate flags and build rules to supply to RIOT to successfully compile OCaml code that is converted to .c files with `$ ocamlc -output-obj -o < source >.c < source >.ml`

# 28/01 Friday
- Document work for the week
- Continue work on native cross-compiler 
- Continue work on using RIOT to build our OCaml code (converted to c)

## Process
1. The previous "Fatal sys/socket.h error: Cannot find file or directory" is related to a bug in ocaml that is addressed in later versions. As a workaround, we undefine the (HAS_SOCKETS) variable with `$ echo '#undef HAS_SOCKETS' >> ocaml/runtime/caml/s.h` as in the Makefile at [Gilbraltar Makefile](../Ocaml_compilers/README.md"https://github.com/dinosaure/gilbraltar/blob/bca12a2a937c3761a2b07e5016fd2d1b0bc16b51/caml/Makefile#L97")
2. Subsequently I encounter cannot find "sys/ioctl.h". Which is required to build the unix library however there is a way to omit this.

## Blockers
- Ran into compilation issues because flags not correctly set
- Gaps in knowledge about how the ocaml runtime and our C source file needs to be linked.