# Week 10 #

# 21/03 Monday #
  * Continue looking into how to implement the network interface
  * Get working mini test to see if messages are being recieved on the client/server side
  * Read up about on how to decoding data raw ip packets

## Process ##
	1. Set up a test program to see if we can send messages to the server using the sock API.
	2. Debugged issues on why we couldn't get the packet data
	3. Looked into the structure of IP packets to see how to decode them and extract information that we want
	
## Blockers ##
  * It seems that the sock method doesn't fit very nicely with our use
    case, extracting ip layer information is a little awkward, might
    be worthwhile to reconsider using GNRC

# 22/03 Tuesday #
  * Relook into using the GNRC stack 
  
## Process ##
	1. Discovered how to use the GNRC stack which doesn't interfere
       with the parsing that is already implemented by RIOT whilst
       having access to the information we need
	2. Worked on setting up the protocol threads
	3. Focus on first implementing the RIOT API wrappers to use for
       our C-bindings
	   
## Blockers ##
  * Unsure about what some of the outputs of the mirage-net functions
    are and need to research on what they are used for in the IP stack.
	   

# 23/03 Wednesday #
  * Worked on implementing pseudoheader function in the tcpip interface
  * Debugged some issues witht the RIOT threads

## Process ##
  1. Worked on fixing the get_ip function per interface
  2. Debugged some issues with the RIOT threads not accepting more than 4 messages
  3. Read up on the structure of the pseudoheader
  4. Learned how to write Cstructs
  5. Talked to ROmain about the approach to add in the interface

## Blockers ##
  * Running into some issues when writing the ocaml implementation. Need to have a better conceptual idea and plan for the approach becasue the typechecker keeps raising errors because I don't follow the interface (i.e. expose too much/too little of the type)

# 24/03 Thursday #
  * Debug problems with packetqueue recieving gnrc_packet_snip
  * Write ocaml functions to access CStructs and write to CStructs
  * Start writing OCaml external functions that call RIOT C-functions to allow us to access gnrc_packet_snip structure in OCaml
  * Figure out how to use Ipaddr library

## Process ##
	1. Test to see if network packets are properly being added to the queue
	2. Investigated why packets stop getting accepted after 4 packets are collected (Forgot to release the packet)
	3. Experimented and wrote convenient functions that access and write to CStructs 
	4. Discussed with Romain on how to get OCaml to access structs in C
	5. Implemented a mini working example on how to perform point 4.

## Blockers ##

	
# 25/03 Friday 
  * Write OCaml external function to access the first gnrc_packet_snip from the packet_queue and then release the packet
  * Write accessor functions to work with the gnrc_packet_snip CStruct

## Process ##
  * Wrote stub code to get gnrc_packet_snip from RIOT
  * Worked on getting and parsing the IP information
  * Tested with getting packets in the OCaml runtime
  * Debugged issue of missing data in the packet snip
  * Start to integrate into the event loop

## Blockers ##
  * Still encountering some weird segfault cases when GDB is used to debug the program

# Next week #
* Finish writing the interface
  * `input` and `write`
* Should be able to test and recieve packets from RIOT into our OCaml runtime 
* Start integrating into the event loop
