# Week07 #

# 28/02 Monday #
  * Work on networking
  * Understand how mirage-flow and other components work

## Process ##
1. Followed iot-lab tutorials on networking 
2. Played around with the modules
3. Went through code base of mirage-flow
4. Read up on tun/tap

## Blockers ##
  * Still don't fully understand how to piece together the GNRC communication interface
  * Not entirely sure how mirage-flow works
  * Gaps in knowlege about how the different levels of the OSI are implemented.

# 01/03 Tuesday #
  * Figure out how to set up a bridge network in Linux for testing
  * Figure out how to send packets with the RIOT api w/o routing
  * Get a better understanding which level we are on the networking stack

## Process ##
1. Talked with Lucas about the network stack
2. Continued experimenting with RIOT networking API's
3. Discovered that RIOT networking stack is implemented by running
   different thread for each layer in the stack. Applications
   communicate with the networking stack via the sock protocol and
   threads in the stack communicate via message passing in the netapi

## Blockers ##
  * Cannot figure out how to include the correct modules required for our stack

# 02/03 Wednesday #
  * Continue researching on how to build and test the networking stack
  
## Prcoess ##
1. Read through papers on IPv6 over 6loWPAN
2. Go through RIOT codebase to understand how to configure/use GNRC modules work

## Blockers ##
  * Could not figure out how to test if 6lowpan interface is being used
  * Currently using GNRC default modules which perform auto
    initialisation of of the network stack based on your system
    default devices. However, according to the documentation, 6lowpan
    will only be included if it detects a IEEE 802.15.4 device.

# 03/02 Thursday #
  * Find out how the auto init modules call start the networking stack
  * Find out how to set up tun/tap virtual network interface infrastructure
  
## Process ##
1. Manually tracked through RIOT code base to find out what each module declaration is doing
2. Handpicked the neccessary modules we want to have in our network
   stack: IEEE 802.15.4 [Physical/Data-link] -> 6loWPAN (translation layer) -> IPv6 [Network] -> ICMP/UDP [Transport] 
3. Started researching about TUN/TAP and how to set up a virtual network

## Blockers ##
  * At the moment, the autoinit modules configure the Data-link layer
    to ethernet which is the underlying protocol required by the
    TUN/TAP interface and is neccessary for our testing. Am unclear
    whether or not if we used hardware that uses IEEE802.15.4, the
    autoinit will correctly configure
  * It seems that the 6loWPAN translation layer is not being used. I
    suspect this might be because the underlying protocol is ethernet
    and not IEEE 802.15.4
