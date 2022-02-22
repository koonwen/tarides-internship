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