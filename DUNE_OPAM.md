# Dune build system and Opam
  More often than not, OCaml projects come bundled with Dune and Opam configuration files. Dune (a build system developed by Jane street) has become the defacto standard for building OCaml projects. The Dune build system can be tricky for first timers because it integrates with Opam (The OCaml package manager) and thus there are differences when discussing about 'packages' in the Dune sense or in the Opam sense.

We will encounter 3 files of interest:
1. `.opam`
2. `dune`
3. `dune-project`

# Opam
The OCaml package manager. 
- Besides allowing us the ease of downloading packages from the opam repo, we can also create segregated development environments through what is known as an opam switch.
- `opam pin` command (used for pinning a specific repo to pull opam packages from instead of the default one) we use this in order to pull the latest version of Mirage)

### **<package>.opam**
- At the top level of you project, if we are creating opam packages (packages downloadable and sharable through the opam system) we need to declare a "< package name >.opam" file. This file should contain dependencies, build instructions, maintainer info, etc.
- A OCaml project may contain 0 or multiple packages, if there is more than one package, each package requires it's own "< package name >.opam". Each package is independent of others regardless if they are in the same repository and will not be able to "see" each others library or code.
- The reason for having multiple packages is to break apart the repository into smaller packages instead of one large one, or to expose different API's for different packages so that redundant dependencies are not carried along.

# Dune
### **dune-project**
The "dune-project" file is responsible for the project configurations

- Dune requires a "dune" file and a "dune-project" file to provide build instructions. Like the .opam file, the "dune-project" file should also be declared at the top-level of the project. 
- "dune-project" is designed to take over the role of .opam files. It contains the same info as the .opam files. However, we still need the .opam file in order to make our project distributable on opam. Hence, in our dune-project file, we can specify that we want dune to generate our opam files by including the line `(generate_opam_files true)`
- A complete "dune-project" file should look like this
```
(lang dune 2.8)
(name cohttp)
; version field is optional
(version 1.0.0)

(generate_opam_files true)

(source (github mirage/ocaml-cohttp))
(license ISC)
(authors "Anil Madhavapeddy" "Rudi Grinberg")
(maintainers "team@mirage.org")

(package
 (name cohttp)
 (synopsis "An OCaml library for HTTP clients and servers")
 (description "A longer description")
 (depends
  (alcotest :with-test)
  (dune (> 1.5))
  (foo (and :dev (> 1.5) (< 2.0)))
  (uri (>= 1.9.0))
  (uri (< 2.0.0))
  (fieldslib (> v0.12))
  (fieldslib (< v0.13))))

(package
 (name cohttp-async)
 ; optional version override to allow single package point
 ; releases.
 (version 1.0.1)
 (synopsis "HTTP client and server for the Async library")
 (description "A _really_ long description")
 (depends
  (cohttp (>= 1.0.2))
  (conduit-async (>= 1.0.3))
  (async (>= v0.10.0))
  (async (< v0.12))))
```
- Notice that there are two "package" stanza's, this declaration goes ahead to produce two "< package name >.opam" files as we have mentioned previously in the **.opam** section

### **dune**
- "dune" files on the other hand should exist in every directory (except perhaps the top level directory). The two main configurations that are commonly used to define these directories are "executables" or "library". As the name states, this instructs dune to compile our OCaml code to build a runnable program in the case of "executable" declaration, or to build a library of reusable code which can be used across other libraries or executables as in the case of the declaration "library".
- dune "library" configs are built to be reusable and accessible by the name of the library with dot notaion
- dune "executables" are built as runnable code from the command line with `dune exec < program name >`
- dune "rules" perform the same function as Makefile rules. We can use dune rules to specify some build instruction to run.

#### Dealing with Foreign Libraries
- dune has built in capabilities for building OCaml code with foreign C libraries.
- Inside the dune file, we specify the following in the foreign_stubs field:
```dune
(library/executable
  (name ...)
  (foreign_stubs (lanugae c) (names ... ...)))
```