# RIOT OS
RIOT OS is an operating system primarily for low powered and low cost IOT devices that don't have much commercial support. Much more than that, they provide a build system which allows us to abstract away the need to handle hardware dependent features and low level interfaces.
 
 - In general, RIOT OS's user facing interface has been designed to interact primarily with C-code. However, for our purposes we want to further abstract the need to write in C and instead build an interface that allows us to write OCaml code and compile it with RIOT.
 
 - The general approach to do this is to make use of the OCaml's compiler "-output-obj" flag to generate our program as well as the OCaml runtime in C and then let RIOT take over to build our program from the generated C source file.