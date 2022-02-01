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

# 1/02 Tuesday
- Learn about how make rules are evaluated and C programs are built
- Debug the build issue

## Process
1. Reordered the Makefile rules so that RIOT's build system would run first. (This did not work because we need our C program to be linked with our runtime first before passing it to RIOT)
2. Renamed the path variable to locate the RIOT_BUILD.h dependency correctly

## Blockers