# Mirage

## Structure of Mirage 4.0
- Coming into Mirage 4.0 from Mirage 3.0. the main difference is in how the tool deals with it's dependencies.
- In v3.0, Mirage would pull and builds dependencies for the host machine but the bottleneck of that is that we want to build these packages for the target machine.
- For v4.0 keeps these dependencies in a parallel directory and builds them during the `mirage build` phase.
- Mirage4.0 also integrates dune as the build system. `mirage build` is synonymous to `dune build`. One thing to take note of is that since there are many more packages to be compiled, we can take advantage of the`dune cache` setting so that not all sources are recompiled. Only those with changes.

## Goal of Mirage
- One way to think about mirage, is that it is a multipurpose adaptor tool that provides an extensible and unified and abstract layer between different backends. For example, packages such as cohttp-mirage implements cohttp in mirage which allows us to create applications with that package and build for whatever target Mirage. Cohttp's core is in pure synchronous OCaml but can unse either LWT or Async as the async backend library

	
## Features of Mirage
- Mirage relies heavily on [LWT](#LWT) to implement asynchrony in the code.
- In Mirage, data stored is not permanent, therefore it takes advantage of a git implementation to store data remotely. Irmin, which is a key-value data store library of Mirage, provides a high level abstraction of it's functions that can be configured with different backends. The result is that we can either decide to use a git implementation or the Irmin core as the implementation of the backend. 
	
The internship goal is first to provide the adaptor from Mirage to RIOT such that we can start exposing RIOT API's in OCaml in which we can then continue building other network/device stacks on top of them.

# LWT
LWT is a lightweight threading library that enables concurrency but not true parallelism. This means that whilst we can create concurrent tasks with the LWT library, OCaml does not yet support multicore. Therefore the LWT may appear to be executing cocnurrently because the thread tasks have undeterministic order in being selected to run by the scheduler. However, this is taking place sequentially on only one processor

- Multicore OCaml aims to be released in Mid-2022 which will mean that Mirage might have to undergo some serious restructuring to take advantage of Multicore
- OCaml was previously unable to utilize multiple cores because of the "global-lock on values" this was a design choice in order to prevent multiple threads from modifying a value during garbage collection. Hence, once a concurrent garbage collector can be figured out, then Multicore OCaml can be developed.

### Concepts:
- In Lwt, concurrency is achieved by writing promises. Promises have the type `'a Lwt.t`. If we want to write functions that run concurrently, we must write each function as a promise.
- Take note that functions that we write as promises are forked and run independently from the "main" thread.
- However, we may want each concurrent function to pass off it's result to another function in a sequential way. This is done by using binds.
- Therefore we should think of our programs as separate concurrent threads in which each thread can have it's own sequential line of execution if we implement them with binds.
- There are many different kinds of syntactic sugars for binds, these are: `Lwt.bind ('a Lwt.t) ('b Lwt.t)` or `>>=` or `let*`
- Lwt promises also have state, these are either `Sleep` or `Return` representing whether the promise has be fulfilled or not. Consider the following which makes use of state. The function takes in two promises and creates a task() with a result and a wakener (during the initialization, the state of the result is `Sleep`). The function `f` takes in a value and matches the state of `p`. If the state `Sleep` is found, the value passed to it in `v` is bound to the result causing the state of `p` to change to `Return`. Finally, notice how we execute both functions `a` and `b` concurrently and feed it's promised result into the function `f`. The first promise to be evaluated would be returned whilst the other is ignored.
```ocaml
let first a b =
  let p, r = task () in
  let f v = match state p with
    | Sleep -> wakeup r v; return ()
    | _ -> return () in
  let _ : unit t = a >>= f in
  let _ : unit t = b >>= f in
  p
```
- More often then not, we have some final output from our program or some kind of meeting point for each of our threads. If all our functions are defined concurrently, we will run into the issue of our "main thread" exitting before any of the other threads can do work. To control this, we define a main function that returns a promise and embed all our concurrent functions inside it and call Lwt_main.run on the main function.

## LWT in Mirage
Mirage is written with these LWT construct because we want to take advantage of asynchronous code even on a single core. Whereby threads yield to other threads that might need to wait for some event.

- Mirage itself is run as a process under a Unix backend, we write code with LWT threads so that we can perform the task of the scheduler (at the process level) and yield for appropriate operations such as IO. This means that there are two levels of asynchrony. Within the process itself, our program has multiple threads that give way and switch between each other. On the level of multiple processes, the base operating system (Linux) performs context switching between the processes.