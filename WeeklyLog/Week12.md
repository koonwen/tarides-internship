# Week 12 #

# 04/04 Monday #
  * Work on registering callback a callback event to network events
  * Work on assembling the TCPIP stack
  
## Process ##
1. Experimented with different Lwt constructs
2. Used Lwt_condition variable to trigger callbacks when waiting for network events
3. This allows us to ensure that the listen function is only called once and applies to all network events

## Blockers ##
  * Not sure where to find the functor to assemble the TCPIP stack.

# 05/04 Tuesday #
  * Fix bugs around OCaml implementation of IP
  * Trace packets with wireshark to see TCP communication
  
## Process ##
1. Manually assemble the stack using the functor provided by Mirage-flow
2. Pull in neccessary dependencies required for the clock and Mirage-random

## Blockers ##


# 06/04 Wednesday #
  * Debug the network stack 
  * Get an echo server up and running

## Process ##
1. Figure our why the checksum is incorrect
2. Fix header sizing information passed to the TCP layer
3. Fixed the Cstub values passed around to OCaml and vice versa
4. After working echo server implementation, tried to compile for actual hardware

## Blockers ##

# 07/04 Thursday #
  * Clean up the OCaml code
  * Document the issues
  * Figure out why duplicate ACK message is being sent

## Process ##
1. Modularize our OCaml code to separate the event loop and the network events
2. Worked on modularising code at the top level (i.e. the extra C-code
   to handle network events passing into ocaml and to start the runtime)
   
## Blockers ##
- Running into undefined references when using external modules

# 08/04 Friday #
  * Fix the underfined reference bug
  * Wrap up the documentation of the internship
  * Figure out next steps

# TODO #
- Presentation ready
- Document the build
- Ring buffer (Handle packages probably)
- Try to reproduce bug by sending a lot of data
- Find out why we have the duliplicate Ack message
