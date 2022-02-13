# 07/02 Monday
- Read up about LWT
- Understand how a event-loop works
- Work on building the run function

## Process
1. Read up on event-loop algorithms
2. Read up on LWT to figure out how to use LWT together with RIOT threading API
3. Tried to understand how solo5 is implemented
4. Tried to incorporate solo5 implementation into our project

## Blockers
- Do not fully understand how the solo5 event loop is implemented. (Mirage_runtime.run_leave_iter_hooks, when Lwt.wakeup_paused is called, what is that calling? What pool of threads is it calling to?)
- Do not understand how Lwt is interacting with the RIOT threading API.
- Unlike solo5, the RIOT OS scheduler is preemtive, whereas Lwt is cooperative. This might mean that we need to change manually tweak the RIOT APIs so that they behave more like a cooperative scheduler.

# 08/02 Tuesday
- Tried to adapt solo5 template to the RIOT implementation
- Continued learning how to use LWT

## Process
1. Looked through and tried to understand the solo5 implementation
2. Spent time going through LWT as pre-requisite knowledge for the solo5 implementation

## Blockers
- Still do not have a firm grasp on how Lwt works internally to support threading within the process
- Need to have a clearer idea on how the event-loop works

# 09/02 Wednesday
- Understand how task() and wait() are used in Lwt
- Understand how solo5 and esp32 run functions are implemented
- Working on finding suitable API's from RIOT to implement the event loop

## Process
1. Understand the algorithm for our event loop
2. Find suitable API's from RIOT
3. Implement the C-stubs for them to be exposed in OCaml

## Blockers
- Having trouble with RIOT OS's event loop API

# 10/02 Thursday
- Debug the event queue
- Implement thread yield
- Implement a way to post event's to the event queue
- Implement a sleep

## Process
1. Used GDB to step through the execution to try and investigate why a posted event was not triggered although being put on the queue.
2. Took example program from RIOT OS documentation to see if the event could be triggered
3. Noticed that the event_timeout function was reading the event queue before the event was posted because RIOT switched back to the running process if there are no other process's to switch to.
4. Realised that the malloc'd pointer that we used to post the event segfaults because we need to initialize it's values to zero.

## Blockers
- Need to plan which event set will interrupt and give control back to our program
