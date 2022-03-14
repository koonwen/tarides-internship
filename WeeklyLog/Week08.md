# Week 08 #

# 03/07 Monday #
  * Find the appropriate API's to add to the C-bindings 
  * Add networking events to the event loop

## Process ##
  1. Try to understand the difference between mirage-net (most likely
     deprecated) and mirage-tcpip.
  2. Draft out the ip.mli interface in our main hacking space
  3. Look for the API's we need to do add to our C-bindings.
  4. Go through tutorials on intercepting the packets. Try to use wireshark
  
## Blockers ##
  * Got stuck trying to figure out how how each layer communicates and
    interoperates. Unsure what the thread registering is for and how
    the multiplexing parameters.

# 03/08 Tuesday #
  * Play around with the networking modules to get a feel of how the modules work together
  
## Process ##
  1. Started by building a simple IPv6 echo server with the GNRC netapi interface
  2. Tinker with the module inclusion and build rules to see how each layer gets included and auto-init
  2. Figured out that there is another IPv6 raw socket interface that exposes API's that might be better for our use case

## Blockers ##
  * Couldn't test the IPv6 echo server. Gap in knowledge on how the socket interface is supposed to work

# 03/09 Wednesday ##
  * Weigh the pros and cons of using GNRC-IPv6 interface or using the IPv6 sock interface
  * Send discussion points to Lucas to get his POV
  
## Process ##
  1. Tinker with the IPv6 raw sock interface to see if it is better for our use case
  2. Read up about socket programming.
  3. Dig through RIOT implementation to see how to use the IPv6 stack
  4. Worked on implementing a toy example of a client and server using the socket interface

## Blockers ##
  * I still am not entirely confident on how to approach using the
    sockets in RIOT. Mostly am unsure on how to test the IPv6 layer

# 03/10 Thursday #
  * Look at the network stack of some RIOT application examples
  * Learn how to use wireshark to sniff packets.
  * Figure out how the tap bridge set up works

## Process ##
  1. Go through current work
  2. Look at more complex RIOT application examples
  3. Read up more about how a TAP bridge works
  4. Look into the interface we want to implement

## Blockers ###
  * It seems like the API's I've been looking at only deal with
    recieving and sending data via a specific sock, set for a
    particular transport protocol. However, what we want to have, is
    the ability to parse the incoming packets (depeding on the type of
    the transport protocol) and then be able to provide the functions
    for extracting the IP and writing or removing the header
    information to pass to the next layer. This means we might need to
    start multiple threads that handle each protocol (hard to organise
    the scheduling), or work on one monolith that switches the way it
    handles the packet, depending on header info.

# 04/10 Friday #
  * Experiment with proposed architecture
	1. init_gnrc_netdev thread
	2. init_ipv6_gnrc thread
	3. create and run a new thread that captures packets forwarded from ipv6 thread (ipv6_ocaml thread)
	4. init ipv6_ocaml thread
	5. execute runtime

## Process ##
  1. Experimented with using RIOT netapi's to recieve packets from the GNRC IPv6 thread.
  2. Could not figure out why after introducing our new thread that our application's network interface cannot be found. Suspect that our the neighbour resolution messages are getting dropped because our thread does nothing.
  3. Figured out that the GNRC IPv6 layer is stripping our datagrams of the headers and passing our thread with the IPv6 headers removed. However, we want to expose the packet with IPv6 headers to our OCaml application to handle.

## Blockers ##
  * Not sure how come we have to interface with out application through the bridge.
  * How is our application interface set up, how does our tap bridge perform address resolution?  
  * Packets seem to be getting dropped at the GNRC IP layer. 
 
