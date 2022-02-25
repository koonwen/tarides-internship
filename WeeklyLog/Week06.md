# 21/02 Monday
- Adding in Mirage Hooks
- Move event handling into OCaml
- Implement a way of handling events
- Look at how to respond to network events
- See if there are console writing API's for RIOT

## Process
1. Added Mirage Hooks
2. Tried to port most of the handling logic of reading input into OCaml
3. Used a Map to store waiting console event's and their ID's

## Blockers
- Stuck on how to design the flow interface in OCaml (how much of the underlying C code should do the work). 
- Unsure on the difference on Lwt.task vs Lwt.condition

# 22/02 Tuesday
- Handle UART events more generically
- Add readchar and readline APIs

## Process
1. Re-evaluate what RIOT API's provide us
2. Notice that UART interrupts only notify us of 1 byte
3. Decide that we must redesign the implementation as compared to esp32 and solo5

## Blockers
- Can successfully respond to simple IO events. However, we need to implement the interface according to the flow.mli
- Need to discuss with Lucas why esp32 doesn't follow the flow.mli interface and what API's we need to have.

# 23/02 Wednesday
- Think about how to implement that read function, following the mirage flow API
- Understand how Mirage-console wraps around Mirage-flow
- Understand what is serial communication, how UART, SPI, I^2C work and are different
- Go through RIOT repo to understand how modules are included and can be used

## Process
1. Discussed with Lucas about the next steps: create a read function that might be useful for the networking protocols.
2. Looked into the RIOT API to see if any code has already been pre-implemented.
3. Read documentation about UART
4. Look through gilbraltar repo to see how their read calls are performed

## Blockers
- Having trouble understanding how to define the stdin and stdout of our program (Using tty)
- Need to understand how to use the CStruct library to implement the read function.

# 24/02 Thursday
- Go through the CStruct documentation and understand the library
- Work on the read function to comply to the flow interface

## Process
- Goal: implement read function
- Prerequisite:
  - Cstruct library
  - Flow interface
  - UART communication
1. Run through CStruct library
2. Understand mirage-flow structure

## Blockers
- Had some undefined references when using CStruct libraries. Was
  discovered later that the CStruct library relies on external C stubs
  that had to be pulled from source
  
# 25/02 Friday
- Start looking into networking and making a learning tree diagram

## Process
- Went through RIOT tutorials for networking
- Read up on the relationship between 802.15.4 and 6lowpan and nimBLE
- Looked into RIOT networking stack

## Blockers
- Confused about how the protocolas layers are implemented and where our code need's to come in
