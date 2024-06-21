WIKIPEDIA:


### Concurrency: goroutines and channels[[edit](https://en.wikipedia.org/w/index.php?title=Go_(programming_language)&action=edit&section=12 "Edit section: Concurrency: goroutines and channels")]

The Go language has built-in facilities, as well as library support, for writing [concurrent programs](https://en.wikipedia.org/wiki/Concurrent_programming "Concurrent programming"). Concurrency refers not only to CPU parallelism, but also to [asynchrony](https://en.wikipedia.org/wiki/Asynchronous_I/O "Asynchronous I/O"): letting slow operations like a database or network read run while the program does other work, as is common in event-based servers.[[91]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-concurrency-is-not-96)

The primary concurrency construct is the _goroutine_, a type of [green thread](https://en.wikipedia.org/wiki/Green_thread "Green thread").[[92]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-:0-97): 280–281  A function call prefixed with the `go` keyword starts a function in a new goroutine. The language specification does not specify how goroutines should be implemented, but current implementations multiplex a Go process's goroutines onto a smaller set of [operating-system threads](https://en.wikipedia.org/wiki/Thread_(computer_science) "Thread (computer science)"), similar to the scheduling performed in [Erlang](https://en.wikipedia.org/wiki/Erlang_(programming_language) "Erlang (programming language)").[[93]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-phrasebook-98): 10 

While a standard library package featuring most of the classical [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control "Concurrency control") structures ([mutex](https://en.wikipedia.org/wiki/Mutex "Mutex") locks, etc.) is available,[[93]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-phrasebook-98): 151–152  idiomatic concurrent programs instead prefer _channels_, which [send messages](https://en.wikipedia.org/wiki/Message_passing "Message passing") between goroutines.[[94]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-99) Optional buffers store messages in [FIFO](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics) "FIFO (computing and electronics)") order[[95]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-summerfield-100): 43  and allow sending goroutines to proceed before their messages are received.[[92]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-:0-97): 233 

Channels are typed, so that a channel of type chan _T_ can only be used to transfer messages of type _T_. Special syntax is used to operate on them; <-ch is an expression that causes the executing goroutine to block until a value comes in over the channel ch, while ch <- x sends the value x (possibly blocking until another goroutine receives the value). The built-in switch-like select statement can be used to implement non-blocking communication on multiple channels. Go has a memory model describing how goroutines must use channels or other operations to safely share data.

The following simple program demonstrates Go's [concurrency features](https://en.wikipedia.org/wiki/Go_(programming_language)#Concurrency) to implement an asynchronous program. It launches two lightweight threads ("goroutines"): one waits for the user to type some text, while the other implements a timeout. The select statement waits for either of these goroutines to send a message to the main routine, and acts on the first message to arrive (example adapted from David Chisnall's book)

```go
package main

import (
    "fmt"
    "time"
)

func readword(ch chan string) {
    fmt.Println("Type a word, then hit Enter.")
    var word string
    fmt.Scanf("%s", &word)
    ch <- word
}

func timeout(t chan bool) {
    time.Sleep(5 * time.Second)
    t <- false
}

func main() {
    t := make(chan bool)
    go timeout(t)

    ch := make(chan string)
    go readword(ch)

    select {
    case word := <-ch:
        fmt.Println("Received", word)
    case <-t:
        fmt.Println("Timeout.")
    }
}
```


The existence of channels does not by itself set Go apart from [actor model](https://en.wikipedia.org/wiki/Actor_model "Actor model")-style concurrent languages like Erlang, where messages are addressed directly to actors (corresponding to goroutines). In the actor model, channels are themselves actors, therefore addressing a channel just means to address an actor. The actor style can be simulated in Go by maintaining a one-to-one correspondence between goroutines and channels, but the language allows multiple goroutines to share a channel or a single goroutine to send and receive on multiple channels.
- In other words go allows for mutiple models to share one actor and one model to belong to multiple actor which is not possible in the traditional actor model concurrency style.


!!! I need to investigate everything in this paragraph further !!!
From these tools one can build concurrent constructs like worker pools, pipelines (in which, say, a file is decompressed and parsed as it downloads), background calls with timeout, "fan-out" parallel calls to a set of services, and others. Channels have also found uses further from the usual notion of interprocess communication, like serving as a concurrency-safe list of recycled buffers, implementing [coroutines](https://en.wikipedia.org/wiki/Coroutine "Coroutine") (which helped inspire the name _goroutine_),and implementing [iterators](https://en.wikipedia.org/wiki/Iterator "Iterator")


Concurrency-related structural conventions of Go ([channels](https://en.wikipedia.org/wiki/Channel_(programming) "Channel (programming)") and alternative channel inputs) are derived from [Tony Hoare's](https://en.wikipedia.org/wiki/C._A._R._Hoare "C. A. R. Hoare") [communicating sequential processes](https://en.wikipedia.org/wiki/Communicating_sequential_processes "Communicating sequential processes") model. Unlike previous concurrent programming languages such as [Occam](https://en.wikipedia.org/wiki/Occam_(programming_language) "Occam (programming language)") or [Limbo](https://en.wikipedia.org/wiki/Limbo_(programming_language) "Limbo (programming language)") (a language on which Go co-designer Rob Pike worked),[[101]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-106) Go does not provide any built-in notion of safe or verifiable concurrency.[[102]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-memmodel-107) While the communicating-processes model is favored in Go, it is not the only one: all goroutines in a program share a single address space. This means that mutable objects and pointers can be shared between goroutines; see [§ Lack of data race safety](https://en.wikipedia.org/wiki/Go_(programming_language)#Lack_of_data_race_safety), below.

#### Suitability for parallel programming

Although Go's concurrency features are not aimed primarily at [parallel processing](https://en.wikipedia.org/wiki/Parallel_computing "Parallel computing"),[[91]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-concurrency-is-not-96) they can be used to program [shared-memory](https://en.wikipedia.org/wiki/Shared_memory_architecture "Shared memory architecture") [multi-processor](https://en.wikipedia.org/wiki/Multiprocessing "Multiprocessing") machines. Various studies have been done into the effectiveness of this approach.[[103]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-108) One of these studies compared the size (in [lines of code](https://en.wikipedia.org/wiki/Lines_of_code "Lines of code")) and speed of programs written by a seasoned programmer not familiar with the language and corrections to these programs by a Go expert (from Google's development team), doing the same for [Chapel](https://en.wikipedia.org/wiki/Chapel_(programming_language) "Chapel (programming language)"), [Cilk](https://en.wikipedia.org/wiki/Cilk "Cilk") and [Intel TBB](https://en.wikipedia.org/wiki/Intel_Threading_Building_Blocks "Intel Threading Building Blocks"). The study found that the non-expert tended to write [divide-and-conquer](https://en.wikipedia.org/wiki/Fork%E2%80%93join_model "Fork–join model") algorithms with one go statement per recursion, while the expert wrote distribute-work-synchronize programs using one goroutine per processor core. The expert's programs were usually faster, but also longer.[[104]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-109)

#### Lack of data race safety[[edit](https://en.wikipedia.org/w/index.php?title=Go_(programming_language)&action=edit&section=14 "Edit section: Lack of data race safety")]

Go's approach to concurrency can be summarized as "don't communicate by sharing memory; share memory by communicating".[[105]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-110) There are no restrictions on how goroutines access shared data, making [data races](https://en.wikipedia.org/wiki/Data_race "Data race") possible. Specifically, unless a program explicitly synchronizes via channels or other means, writes from one goroutine might be partly, entirely, or not at all visible to another, often with no guarantees about ordering of writes.[[102]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-memmodel-107) Furthermore, Go's _internal data structures_ like interface values, slice headers, hash tables, and string headers are not immune to data races, so type and memory safety can be violated in multithreaded programs that modify shared instances of those types without synchronization.[[106]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-111)[[107]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-SPLASH2012-112) Instead of language support, safe concurrent programming thus relies on conventions; for example, Chisnall recommends an idiom called "aliases [xor](https://en.wikipedia.org/wiki/Exclusive_or "Exclusive or") mutable", meaning that passing a mutable value (or pointer) over a channel signals a transfer of ownership over the value to its receiver.[[93]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-phrasebook-98): 155  The gc toolchain has an optional data race detector that can check for unsynchronized access to shared memory during runtime since version 1.1,[[108]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-113) additionally a best-effort race detector is also included by default since version 1.6 of the gc runtime for access to the `map` data type.[[109]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-114)

- SO MANY QUESTIONS! What is divide and conquer algorithms? what is distribute-work-synchronize programs? How do I implemetn aliases xor multaple convention? how do I share memory by communicating in practice? I need to learn this shit.

