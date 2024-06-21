The wikipedia page on go is incredible. You should use it as consulting material.

Statically typed: the types must be know at compile time.

compiled: Its not interpreted, meaning the code becomes a binary file that can be understood by the machine after compilation.

high-level: Highly abstract programming language, a programming language that highly abstracts the way in which a computer works
usually sacrificing performance and control of the hardware for readibility, safety, convenience, simplicity(lack of complexity)
and so on.

memory-safe: a language that does not allow you to make memory errors. A language that handles memory errors for you.

garbage-collection: a background repetitive process thread that is responsible for memory allocation and deallocation.

 structural type system: a type is defined by the structure/layout of the type rather than any explicit declaration a name.
A type that implements a interface is of both it's type and it's interface type.
    An object which is of an interface type is also of another type, much like C++ objects being simultaneously of a base 
    and derived class. 
The structural type system is used in interfaces and for type embedding when it comes to Composition
    -Go users like to say that interfaces and such are of the duck type system, that cant be the case since it implies that type
    conformance is checked at runtime. Interfaces and other structural types are checked for conformance at compile time, hence
    duck typing does not really happen in go.


nominal type system: a type is defined, differentiated and equivalent based on an explicit declaration of name. Structs, primit
ive types, and so on are part of the go nominal type system

CSP-style concurrency: a whey to implement concurrency based on the Communication Sequential Processes model that determines that
lightweight threads should communicate with each other through channels. Channels are conduits that synchronize the execution of
the lightweight threads reducing race conditions and allow the lightweight threads to safely exchange data  between themselves.
    Remember that we are talking about concurrency here so it is not true multitasking (like parallelism), that is why channels
    even have the opportunity to allow data to be transfered between threads safely. If we were talking about parallelism (true
    multitasking) instead of concurrency, that would not be possible.

Cross-plataform

High-performance networking(tranfer of data between computers through network nodes) and multiprocessing(what allows concurrency
to be a thing in the first place)

Optional concise variable declaration and initialization through type inference

Fast compilation

Remote package management (go get) and online package documentation

statically linked native binaries: an executable file that includes all necessary libraries and dependencies within itself at 
compile time.
