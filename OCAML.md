OCaml
=====
# Runtime #

## Memory Management (The Garbage Collectors internals) ##
OCaml possess's a very powerful feature that is automatic memory
management. The benefit of which, is that we can release the
programmer from having to manually keep track of what memory is being
used and what can be freed. At the same time, removing any exploitable
security holes that arises from poor management of memory such as
buffer overflows. The main drawback of automatic memory mangement
however, is that we have less precise control over the memory of our
program and thus less performant code.

The OCaml GC is a variant of a tracing GC but unlike traditional
tracers, instead of performing the full marking and sweeping process
during program intervals, instead it does so incrementally, doing
portions of the work in short time slices. The OCaml GC also maintains
two two memory regions, known as the **Major** and the **Minor** heap.

### Major Heap ###
  * The major heap is the main storage point of GC whereby objects are
    provided a *permanent* region of memory for storage (as opposed to
    ephemeral storage on the stack).
  * Just like how a call to malloc requires internally finding a space
    within memory to find sufficient space for our object, major heap
    allocation's are costly

### Minor Heap ###
  * The Minor heap serves to increase the performance of the GC by
    providing a cheaper allocation policy
  * All the Minor heap really is, is a small contiguous memory chunk
    that is referenced to by a pointer and to allocate space, we
    simply decrement the pointer by as many bytes the object needs
  * When the GC is run, we go over all the objects in the minor heap
    and if they are live, the GC promotes it to the major heap
