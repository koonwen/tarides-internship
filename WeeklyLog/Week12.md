# Week 12 #

# 4/04 Monday #
  * Work on registering callback a callback event to network events
  * Work on assembling the TCPIP stack
  
## Process ##
1. Experimented with different Lwt constructs
2. Used Lwt_condition variable to trigger callbacks when waiting for network events
3. This allows us to ensure that the listen function is only called once and applies to all network events

## Blockers ##
  * Not sure where to find the functor to assemble the TCPIP stack.
