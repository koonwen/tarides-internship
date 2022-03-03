# RIOT OS
RIOT OS is an operating system primarily for low powered and low cost IOT devices that don't have much commercial support. Much more than that, they provide a build system which allows us to abstract away the need to handle hardware dependent features and low level interfaces.
 
 - In general, RIOT OS's user facing interface has been designed to interact primarily with C-code. However, for our purposes we want to further abstract the need to write in C and instead build an interface that allows us to write OCaml code and compile it with RIOT.
 
 - The general approach to do this is to make use of the OCaml's compiler "-output-obj" flag to generate our program as well as the OCaml runtime in C and then let RIOT take over to build our program from the generated C source file.
 
## Build system ##
RIOT is a highly modular system. This means that the developer is
responsible for instructing the RIOT build system to build the
neccessary modules **and** include the header files required by the
application source files.
  * RIOT is not particular well documented, information as to how to USE_MODULE and the path name for the header files are not always clear. The general rule of thumb is:
	  1. Names of modules implementation is in sys/
	  2. Names of the module's header file is in the sys/include
