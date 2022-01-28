# Ocaml's Compilers
OCaml comes prepackaged with two compilers. They are runnable through the command-line with ocamlc (bytecode compiler) and ocamlopt (native code compiler).

### ocamlc
The bytecode compiler

# The Runtime system
OCaml's runtime is composed of three parts:
- bytecode interpreter
- memory manager
- set of C-functions

When OCaml programs are compiled, the compilation process includes linking in the OCaml runtime which includes the garbage collector as well as the representation of values. One interesting fact is that OCaml values reserve more bits than neccessary to represent the value. This is so that we can use the most significant bit to indicate some piece of information for the garbage collector.

- The OCaml runtime is written entirely in C and can be found inside the compiler directory under the name of libasm.run (For bytecode) and libasm.opt (For native compilation)?


# Interfacing C with Ocaml
- Using "cytpes" and "ctypes-foreign" package