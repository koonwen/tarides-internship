# Tarides-internship
Weekly logs of my internship progress

# Topics
- [Mirage](./MIRAGE.md)
- [RIOT OS](./RIOT_OS.md)
- [Compilers and OCaml](./COMPILERS.md)
- [Dune & Opam](./DUNE_OPAM.md)
- [Linux & Git](./LINUX_GIT.md)

# Legend
Each week summarizes what I spent my time on. I have tagged them according to a scale on how much effort I spent on each topic.

1. `Overview` : brief amount of time spent on trying to grasp the big picture of the topic. Usually includes quick google searches or short videos explaining the topic
2. `Tinkering` : I spent a moderate amount of time on the topic. This is usually when I am playing with a tool or a command to figure out how I can use it.
3. `Understanding` : I dedicated a sizeable amount of time researching and trying to grasp the topic. Includes more careful reading of documentation and generally applies to figuring out how a tool works.
4. `Learning` : I spent a significant amount of time on this topic because a strong understanding is neccessary in helping me to work on the project. Usually entails working through tutorials and protoyping.
5. `Implementing` : Lot's of hours spent. Highly likely that it is part of the main project

# Week 1
- [x] `Overview` of networking protocols: IPV6 interface implemented with 6lowpan, BLE, 802.15.4
- [x] `Tinkering` with Opam switches
- [x] `Tinkering` with Sysadmin tricks
- [x] `Understanding` OCaml Bytecode compilation (ocamlc) vs OCaml native compilation (ocamlopt)
- [x] `Learning` of MirageOS

# Week 2
- [x] `Overview` of LWT threads and it's relation to MirageOS
- [x] `Tinkering` with Git patching tricks
- [x] `Understanding` the dune build system
- [x] `Understanding` how to install and build a compiler with link time optimization from source
- [x] `Learning` how to interfacing C with Ocaml
- [ ] `Learning` Building OCaml 4.10+lto comopiler with arm-none-eabi-gcc cross compiler (Host:x86-64 to Target:32bit Arm)
- [ ] `Learning` Interfacing OCaml programs with RIOT OS APIS

# Week 3
- [x] `Tinkering` with Makefiles
- [x] `Understanding` the C compilation process
- [x] `Learning` the [Dune/RIOT build system](https://github.com/TheLortex/ocaml-riot-unix)
- [ ] `Learning` the OCaml foreign function interface to extend RIOT APIs in OCaml

# Week 4
- [x] `Understanding` RIOT's timer and event API's
- [ ] `Understanding` how event loop's work
- [x] `Learning` Lwt
- [x] `Implementing` C stubs for RIOT OS API's
- [ ] `Implementing` the `run` and `sleep` functions

# Week 5
- [x] `Understanding` how event loop's work
- [x] `Understanding` console events and UART
- [x] `Implementing` the `run` and `sleep` functions
- [x] `Implementing` event handlers for interrupts coming from UART

# Week 6
- [x] `Tinkering` with the UART Callbacks and `Understanding` Serial communication
- [x] `Learning` about how Mirage devices make use of mirage-flow
- [x] `Learning` 802.15.4 protocol and how it relates to 6lowpan and bluetooth
- [x] `Implementing` the `read` function according to mirage-flow

# Week 7
- [x] `Understanding` RIOT's networking API's
- [x] `Learning` about the networking stack and our specific choice for the project
- [x] `Learning` about RIOT's networking module system and layer to layer communication

# Week 8
- [x] `Learning` and experimenting with the "ipv6 socket" vs the "netapi" method of interacting with the RIOT's network stack
- [x] `Learning` how to use wireshark
- [x] `Learning` what happens to datagrams going up and down the stack
- [ ] `Implementing` IPv6 layer itself by adapting the existing gnrc ipv6 thread

# Week 9
- [x] `Understanding` the composition of Mirage networking stack and the IP layer we are trying to implement
- [x] `Learning` about the difference between TCP/UDP socket vs the raw socket
- [X] `Implementing` the interface figuring out how to make the networking information available in OCaml and the Lwt event loop.

# Week 10
- [x] `Implementing` the RIOT GNRC stack instead of the sock api because it is easier to extract the IP information
- [x] `Implementing` the C stubs to pass packet information from RIOT into OCaml
- [x] `Implementing` the helper functions and Cstructs to decode the `gnrc_packet_snip` stucture passed to the OCaml runtime.

# Week 11
- [x] re-`Implementing` the RIOT GNRC stack with the sock API
- [x] `Implementing` the C stubs required for the translation to OCaml
- [x] `Implementing` the `listen` function for our interface
- [ ] `Implementing` the manual test case for the interface

# Week 12
- [x] `Tinkering` with the build system to make it more clean
- [x] `Implementing` the TCPIP stack
- [x] `Implementing` the listen function with various Lwt constructs
- [x] `Implementing` an echo server

# Dialogue
The goal of the internship is to be able to create a process/system that allows us to take high-level OCaml code and be able to compile it down to run on resource constraint IOT devices.

Working off of the preious intern (Ben's) work, he figured out that the main obstacle is that the OCaml code compilation pipeline produces code that is far to large (memory wise) to fit on our target devices. In particular, it is the OCaml runtime that takes up a lot of memory because the runtime includes a lot of C primatives that aren't used.

In the **Week 1** of the internship, time was spent on setting up my development environment, getting used to the tools and learning about the Mirage ecosystem.

To get up to speed with what Ben has worked on, For **Week 2**, as an exercise in cross compilation, I tried to build an OCaml compiler with my host system (Intel x86-64) to targetting to build for (Arm 32bit architecture). The end goal here was to successfully perform a cross compilation and be able to run my OCaml program on our target device. Knowing that code produced by our OCaml compiler will be far too large, Lucas suggests to try using the OCaml 4.10.0+lto compiler (The latest compiler with link time optimization). In parallel with building the cross-compiler, the second exercise is to play with the RIOT-OS interface and see if we can successfully link and build an OCaml program with the RIOT build system. The approach is here is essentially what Ben has been working on (omitting the step of performing ocamlclean). We use the OCaml bytecode compiler to generate our program and the OCaml runtime as C code. By tweaking the RIOT makefile, we can link in the runtime and allow RIOT's build system to take over.

For **Week 3**, Lucas has set up the build system which is a mixture of Dune and Make build rules to successfully compile our OCaml code. The pipeline is as such: 
1. The "example" directory is meant to be an isolated working space for us to develop in OCaml. The Dune build system is first to execute and compiles our OCaml code along with the runtime into bytecode. The bytecode is then cleaned and then converted into C code. The choice of using Dune for the first part of the pipeline is to maintain the developer familiarity of using Dune in OCaml projects
2. Subsequently, the Makefile at the top level of the project is responsible for setting up the environment such that RIOT can take over the build. This setup includes moving both the runtime library (libcamlrun.a) and the source `main.c` file to the top level where RIOT expects it
3. Finally our Makefile "includes" the RIOT build system which compiles our program with the relevant C Flags.

with this build system in place, we can start to write APIs that interface RIOT APIs as OCaml APIs. Currently, the problem is that we want to define our C-stubs in a separate library and link in during build time. However, we rely on dune to do the build
which does not currently have RIOT's flags to compile correctly.

***Digression**: One initially peculiar thing to me was how we could successfully compile an OCaml printf. Wasn't the whole point of this project to fulfill the gap to provide OCaml code APIs to interact with RIOT OS? The reason for this oddity is because the `caml_functions` rely on newlibc packages' libc.a(static library). The library provides all the symbols that our runtime looks for during compilation. Newlibc's symbols then rely on RIOT's system call library which is also fully provided because newlibc is designed for embedded devices and therefore has a somewhat minimal library.

At the start of **Week 4** I refresh my understanding of the build pipeline:

1. We first need to provide all the neccessary files for our OCaml code to be passed over to RIOT. These include:
   - startup.c (entry point to initialize our OCaml code)
   - runtime.c (The runtime of our code)
   - stubs.c (C-stubs that will be used to implement our mirage interface)
   - libcamlrun.a (The runtime library of our code)
2. We use dune to generate a custom runtime library (libcamlrun.a) and the actual runtime itself (runtime.c).
    - Regarding the custom runtime library, we need to configure the dune rules found in the ocaml repo so that it uses the Arm32 cross-compiler. For some reason, we need to use the RIOT requires the setup to be done in this way in order for the library to be link without error when RIOT takes over the build.
3. Once all these are generated, our custom RIOT Makefile can take over the build and compile for different embedded targets by passing in `TARGET=board_name make`.

During the week, I made some progress with the C-stubs and managed to implement most of the primatives that I will need to construct the run and sleep function. It is now a matter of thinking about how to write the logic in OCaml to give back control to our program depending on the type of interrupting event. The two methods to consider are using the high level OCaml map data structure or a simple bit flags. 

**Week 5** went by and I have made some progress in understanding what we are providing in our Mirage interface and how it should work. The revelation came when I realised that the `run` function is supposed to add logic at the level of interacting with the operating system and other system process's. `run` acts as a wrapper over our Ocaml code written in the form of Lwt threads. It's main job is to structure how our OCaml program should behave (giving back control to the OS) when it is doing nothing but waiting for I/O events. It also defines what interrupting event's to react to. Here it is key to understand that unlike our conventional multi-user OS, we don't have a prememtive scheduler. Therefore we need to manually add in this logic.

Regarding the implementation, I first began by creating a barebones `run` function that only does something if there is a timeout event (implemented with a `sleep` function). If the queue only has sleeping threads, then it will select the shortest timeout and return control to the OS, waking up when the timeout has expired. Afterwhich, the next task is to implement interrupting console event's. We do this via STDIO over UART. However, initially I found out that UART is not typically used in UNIX like systems but rather only in embedded devices. As a result, the API's to check for STDIO was incomplete for a native compilation. We looked into alternatives to work around this but at the end of the week, I discovered a useful tutorial forum about working with UART natively that would help us in the implementation.

**Week 6** Marks the mid-way point of the internship and warrants a re-evaluation of progress made. So far, the work up to this point has been from the bottom-up by first hacking the RIOT build system such that it can take OCaml programs and build them for RIOT targets. This included 
1. Configuring the toplevel Makefile to instruct dune to build the OCaml artifacts
   - runtime.c (custom OCaml runtime in c)
   - libacamlrun.a (custom 32bit OCaml runtime library) 
   - stubs.c (primative API's that our OCaml programs reference)
2. Providing a startup.c file that calls into our OCaml runtime/program.
3. Passing the neccessary C and Linker flags for the RIOT build

After the build system was set up, the next thing to do was understand how Mirage act's as a wrapper over OCaml programs. Mirage uses the Lwt framework to write concurrent logic in OCaml programs, however, these are only process level concurrency and we want to build in system level concurrency (e.g. if our program is waiting for an I/O event and no other thread in the program is ready for work, the OCaml process will block and no other system thread will be allowed to run). In essence, what we needed to implement, was an event loop wrapperframework over user OCaml code (the `run` function).

Once that was accomplished, to bear the fruits of our labour, we needed to implement custom IO functions that work just like Lwt functions and return promises. As an exercise, the I implemented a sleep function (bearing the same interface as the Lwt_unix.sleep function) that put the process thread to sleep and if there was no pending work, give up control to the OS for the given timeout period. In this way, achieving concurrency at the system level. System level control begets the use of system calls and unlike in familiar Unix environments, a lot of digging through the RIOT documentation had to be done to uncover convenient API's that will allow us to program our event loop. Moreover, because it is more pleasant to program in OCaml than in C (API's language), a bulk of the work involved familiarizing how to translate (RIOT API's C functions) into OCaml callable functions. I would also gain more experience on this translation process by implementing I/O event's such as `read` over UART serial communication. Further work was done to design `read` to conform to the mirage-flow interface which is how the mirage tool can plug together devices with different underlying implementations as long as they conform to the same interface.

The end of week 6 also marks the beginning work into networking and how to now adapt RIOT OS networking API's into OCaml so that users can write networked applications in OCaml!

**Week 7** was spent primarily on learning about the networking stack we want to use for the project. Since I did not have a strong understanding of networking, It took me most of the week to get a hang of things. In particular, our project aims to implement an IPv6 networking layer protocol which will allow us to use TCP transport protocol and later, a HTTP application protocol. However, things get a little more complicated because for low-resource embedded devices. The problem is that our embedded devices cannot handle the size of frames coming in from the data link layer (IEEE 802.15.4). Hence we need to provide a translation medium that will allow us to compress (for sending) and decompress (for recieving) network frames. This is done through 6loWPAN. For visual reference, our stack is as follows beginning from the lowest level:

IEEE802.15.4 (Physical & Data Link layer) -> 6loWPAN (translation) -> IPv6 (Network layer) -> TCP (Transport layer) -> HTTP/HTTPS (Application layer)

All that said, all we really need to do (as I later realised) is to find the correct RIOT API's for the network layer, implement the build rules and add them to the event loop. Since we are testing on Linux, we don't have much control on everything below the network layer. Good news is that between the layers, the interfaces are uniform, meaning that we can hack away at the upper layers without worrying about breaking the lower layers. When it finally comes time to test on the embedded devices, we can pretty much (hopefully) plug and play.

**Week 8** is here and I am still highly confused by RIOT's networking
system. The combination of outdated tutorials and the strange module inclusion system makes it extremely hard to find the macros and functions I need. I notice that I am spending way too long trying to pick out the modules I need after going through the documentation. Therefore I think it might be more productive to go through the header files to look for what I need instead of trying to rely on the documentation. As a way to clarify what I need to do, I will specify here that in our construction of the networking stack, we need to implement the following:

Physical/Data Link (Build rules, use the default plug in modules) -> IPv6 (Adpat RIOT's API's into the function signatures of mirage-tcpip (ip.mli) interface.

This week, (**Week 9**) I finally have a grip over how the module inclusion system for the networking stack works. Each module represents a protocol on some layer and depending on what protocols we want our application to use, we include them in the Makefile and *usually* they are auto-initialised and run in their own thread. Because each protocol runs in each thread and has a fixed interface layer to them, we cannot really do some hacks to their source and insert our own implementation. Instead, we need to insert our own thread that recieves the datagrams from the gnrc threads and then perform our manipulation. Design wise, we can either insert our thread on top of the sock API **OR** insert our thread on top of the netapi.

the result of discussions with Lucas has helped me to conclude that RIOT has put us in an awkward situation where building on top the gnrc ipv6 thread would have been the most natural but we have no API's for interacting with that layer. Alternatively, the raw socket API level gives us much less control over how we want to manipulate the packets. In any case, in the interest of not having to reimplement the IP layer as neccessary if we replace the ipv6 thread, we have chosen the raw IP sock route. Towards the end of the week, as I began to start trying to write C-bindings, we encountered a strange race condition occuring in our build system. Because of the inclusion of the networking modules, the OCaml startup was running into inconsistent floating point bugs. Turns out htat the fix for this was to slow the execution by putting in a sleep before CAMLstartup which had the desired effect of stubbing this strange bug, yay!

"Take 1 step back to take 2 steps forward". This is what happened in (**Week 10**). Although we had 'settled on' using the sock API, I found it quite unweildy and hard to handle in terms of the remote vs local endpoints it defined. Also, having less threads in our system might make sense in the future when we run on constrained memory environments.Therefore, taking a second look into the GNRC stack, I noticed that we could add our own transport protocol threads on top of the ipv6 thread just to recieve the data and pass it into our OCaml runtime. In addition, there were a bunch of warnings that said that this API could potentially be changed which deterred me from using it. In any case, the usage of GNRC was successful, I managed to recieve data into our threads and translate them into OCaml Cstructs. Internally, to be able to pass the information into the OCaml runtime, I set up an a packet queue to hold the data ephemerally and notify that an event occured by adding it to the event queue. Right now I am working solely on getting TCP up and running becuase we want to see if we can prove that we can get a http server up and running on our embedded devices. The following week will be tasked to complete the interface now that we have some of the major design choices settled.

(**Week 11**) blew past in part because I accidentally wiped most of the progress I made in Week 10. This means that I had to rebuild the packet reception from the beginning. Interestingly, by this time I had grasped how to use the socket API and found that it might be easier and faster to use the sock API as the base layer to rebuild the translation layer. In addition to having to reimplement how the underlying infrastructure interacts with OCaml, I managed to also complete the interface. Along with this, Lucas made me aware that the `input` function in the context of our RIOT application is incorrect. Rather, we need to have a `listen` function that applies the callback from the TCP layer because we have no Ethernet module (handled by RIOT) to call the `input` function. Finally, I should also mention that our perculiar runtime bug appears somewhat irregularly when we run our program. We were unable to find the bug with Enguarrend, however, we have narrowed down the issue and our next steps is finding the compilation flag to prevent using any floating point optimisation assembly instructions.

(**Week 12**) As we close in on the initial internship end date. The last bit of proof that our translation layer works is that we manually construct our TCPIP stack and run an echo server. With the help of Lucas, we debugged and found problems mostly related to wrong passing of OCaml values into C and vice versa. With those issues out of the way. We managed to have a minimally running echo server that proves that our OCaml code can run on top of RIOT OS and have an automated build system that allows us to write out networking application in OCaml. The following task is not to clean up the code base and present the project and also see how 6lowpan of BLE would work.
