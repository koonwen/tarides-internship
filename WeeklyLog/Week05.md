# 14/02 Monday
- Test and verify the sleep function
- Continue working on the run function

## Process
1. Lucas noticed that the run function doesn't make any call to the sleep function
2. Tried to write some tests for the sleep function to test if it is perfoming as expected instead of busy waiting

## Blockers
1. Not entirely sure what pieces of logic rely on the primatives and how the event-loop system is supposed to work at a high level.

# 15/02 Tuesday
- Complete minimal event loop run function with sleep API
- Start implementing the yield function

## Process
1. Code review with Lucas to see what is missing with the program. Talk about Lwt.poll and understand more clearly about how the Lwt scheduler works.
2. Convert Opam switch to use 32bit base compiler (from 4.11.2 onwards, there is a special way of initializing a switch which is to use -variants+options flag)
3. Work on cleaning up the code to correctly translate OCaml values into convenient types for the RIOT OS APIs whilst ensuring that our microsecond integers stay as int64 to prevent overflow. Also learned a bit about OCaml unboxed and heap allocated values.
4. Started to implement and debug the event_wait_timeout64() RIOT API stub.

## Blockers
- The queue->event_list->next get's changed from the event to 0x1 during CAMLparam2 macro. Not sure why.
- After talking to Lucas, the reason for this bug is that I was incorrectly allocated the address of the queue onto the stack and passing that value back to OCaml which expects it on the heap. 