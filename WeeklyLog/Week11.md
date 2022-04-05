# Week 11 #

1. Complete the interface
2. Add in the `connect` function
3. Test on real hardware

# 28/03 Monday #
  * To integrate packet reception properly and consider edge cases
  * Clarify with Lucas about whether the conceptual idea of how I'm performing packet reception is okay
  * Ask Enguerrand about the weird segfaults
  * Work on the easy functions and make sure they work
  
## Process ##
1. Debugged error with Enguerrand
2. Accidently wiped progress from 2 days ago
3. Worked on getting back progress in sock API's
	
## Blockers ##
  * Debugged with Enguerrand
    * Problem has to do with the initialisation of OCaml values
    * Happens when optimised floating point instructions are used
    * Tried to evict these instructions by using the compiler option
      CFLAGS=-msoft-float. However these didn't prevent the floating
      point instructions from being used. Using
      CFLAGS=-use-general-regs failed to compile because some code in
      the OCaml source uses SSE reg explicitly.

# 29/03 Tuesday #
 * Work on input and the write function (Internal data structures)

## Process ##

## Blockers ##
  * The strange floating point exception. Worked on that with enguerrand 
  * Most of the interface completed (Only for TCP) and most importantly, I can recieve packets in the ocaml runtime. Have the write function
  left (and I think 1 or 2 more), not sure how to proceed with the input function because it is weirdly positioned in the interface, not really neccessary for the RIOT API's because we already know which transport protocol we are serving.
  * Now I'm thinking about how to test, because we need to compile the whole
  stack? Or maybe write some unit tests.

# 30/03 Wednesday #
  * Work on the `listen` function
  * Change the interface not to use the input function because it's
    supposed to be used by the Ethernet layer

## Process ##
## Blockers ##
  * How do I implement the listen function which needs to run a loop
    so that it doesn't block the event loop?

# 31/03 Thursday #
  * Work on listen function
  
## Process ##
## Blockers ##
  * Need to find a Lwt function that can support running an infinite
    loop without blocking the entire program
