# Week 09 #

# 14/03 Monday #
  * Discuss with Lucas on the implementation architecture

## Process ##
	1. Had a discussion with Lucas about the use of the sock API vs the netapi's
	2. Decided that using the sock API will be better for quick iteration and hacking something to work
	3. Started to plan out how to write the bindings neccessary for our C-stubs

## Blockers ##
  * The sock API is build on top of the netapi's, in that case, why don't we build directly on top of the netapi instead of going through the sock api?

# 15/03 Tuesday #
  * Go into depth about the socket API
  * Start building out base socket infrastructure 
  * Experiment with the netapi's to see if we can/want to bypass the sock api

## Process ##
	1. Researched and implmented the Makefile so that our build system pulls in the correct modules required for the GNRC network stack up until the sock api
	2. Start to implement the protocols (UDP and TCP) to run in their own thread and expose the neccessary API's to be used in OCaml.
	3. Finally got a hang over how the whole networking system is pieced together
	4. Built a small test program to see how to construct a networking stack and where we can insert our ocaml stubs.

## Blockers ##
  * Will need to think about our build system again before deploying on actual hardware to see if it compiles to use the 6loWPAN protocol automatically if it is detected
  * For some weird reason, we need to initialize the gnrc_udp module even though we want to plug in our own udp layer instead of using the default gnrc one.
  
# 16/03 Wednesday #
  * Start implementing the interface with (raw) sockets API
  * Write the C-bindings first before adding it to the event loop in OCaml
  
## Process ##
	1. Looked into the raw sockets API to pick out the important ones to stub
	2. Wrote some base infrastructure code to test sending messages over the network without calling into our OCaml application
	3. Start to write bindings and test if they can run in OCaml

## Blockers ##
  * Taking some time to familiarise back with how to write C-bindings
  * Encountering some build errors when trying to set up the network stack on RIOT.

# 17/03 Thursday #
  * Work on figuring out how to network pass state to OCaml (Global state)
  * Tried debugging build errors
  
## Process ##
	1. Asked Fabrice for help with debugging the OCaml runtime
	2. Worked on adding shell commands to see the current state and data of UDP packet captured

## Blockers ##
  * For some reason, when we include networking modules into the RIOT build system, our OCaml runtime becomes buggy. It often crashed with illegal floating point operations. Curiously, the bug cannot be tracked by using the crash reporting rr tool and becomes increasingly evasive as we try to track it. The error so far inconsistently occurs in two different places (as far as I know) related to GC operations. I suspect that there might be overlapping memory regions and the error occurs because a networking thread and the GC are manipulating the state of the same memory location.

# 18/03 Friday #
  * Try to write some toy networking tests in C to test out if my understanding of raw sockets is correct
  * Fix build system
  * Continue implementation
  
## Process ##
	1. Followed RIOT tutorials on Raw socket API. I can figure out how to use the TCP/UDP sock but having trouble getting the Raw sock API to accept the UDP/TCP protocol.
	2. Debugged build system floating point exception issue with Lucas.

## Blockers ##
  * Still need to figure out how to use the raw sock API
  * Need to think about which C-bindings to write and implement thems