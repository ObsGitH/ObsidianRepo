### Key Concepts of Go Garbage Collection

1. **Concurrent and Parallel GC**
    
    - Go's garbage collector is concurrent, meaning it runs alongside the main application, minimizing pause times.
    - It can also run in parallel on multiple CPU cores to improve efficiency.
2. **Mark-and-Sweep Algorithm**
    
    - Go uses a mark-and-sweep algorithm. This process involves two main phases:
        - **Mark Phase:** The GC identifies which objects are still in use (reachable) by traversing the graph of object references starting from the root set (global variables, stack variables, etc.).
        - **Sweep Phase:**
	        - The garbage collector scans the heap to find objects that were not marked during the mark phase.
	        - The GC scans the heap and reclaims the memory of objects that are not marked as in use.
1. **Tri-Color Marking**
    
    - Go’s GC uses a tri-color marking technique to efficiently manage memory:
        - **White Set:** Objects that are not yet marked as reachable. These objects have not yet been visited by the garbage collector. Initially, all objects are white because they are unmarked.
        - **Gray Set:** Objects that are reachable but whose children have not been checked.
        - **Black Set:** Objects that are reachable and whose children have also been checked.
    - During the mark phase, objects move from the white set to the gray set and finally to the black set.
4. **Write Barriers**
    
    - Write barriers are used to maintain the integrity of the marking process during concurrent execution. When an object reference is updated, the write barrier ensures that the referenced object is properly marked.
5. **Generational Hypothesis**
    
    - Go does not  implement generational garbage collection but optimizes for short-lived objects. Most objects are allocated on the heap, and short-lived objects are quickly collected, while long-lived objects are scanned less frequently.

### How GC Works in Go

#### 1. Allocation

- When a new object is created, Go allocates memory for it on the heap. Small objects are allocated from a thread-local allocation buffer (TLAB) to reduce contention.

#### 2. Mark Phase

- **Root Set Identification:** The GC starts by identifying the root set, which includes global variables, stack variables, and other references.
- **Marking:** The GC traverses the object graph starting from the root set, marking all reachable objects. Unmarked (white) objects encountered are moved to the gray set.
- **Gray Set Processing:** The GC processes the gray set, marking referenced objects black and adding their children to the gray set until no gray objects remain.

#### 3. Sweep Phase

- **Heap Scan:** The GC scans the heap for objects that are still white (unmarked).
- **Reclamation:** The memory of these unmarked objects is reclaimed and made available for future allocations.

#### 4. Finalizers

- Objects can have finalizers, which are functions that run before the object's memory is reclaimed. The GC ensures finalizers are called as needed.

### Tuning the Garbage Collector

Go's garbage collector is designed to require minimal manual tuning, but you can adjust its behavior using environment variables and runtime functions:

- **GOGC Environment Variable:** Controls the garbage collection target percentage. For example, setting `GOGC=100` means the GC will run when the heap size has grown by 100% since the last collection. Lower values increase the frequency of GC cycles, while higher values reduce it.
	- It is speculation from my part, but this makes me assume the rate in which the GC is called is determined by the heap memory growth rate

```go
export GOGC=100

```
    
- **`runtime.GC()` Function:** Manually triggers a garbage collection cycle.

```go
runtime.GC()

```
    
- **`debug.SetGCPercent()` Function:** Adjusts the garbage collection target percentage at runtime.

```go
import "runtime/debug"
debug.SetGCPercent(200)

```
    

### Performance and Latency

Go's GC aims to minimize pause times and maintain low latency, making it suitable for real-time applications. However, GC can introduce overhead, and optimizing memory usage patterns (e.g., reducing allocations, reusing objects) can help improve performance.
- Translation: GC reduces performance so it is interesting to work with objects in  a way that minimizes the performance cost of a GC. You can do that by reducing the amount of objects created ("reducing allocations"), making sure that you only deallocate that object from memory when there is no use for it anymore, not creating objects that serve the same purposes and reusing one of them over and over again in your code and so on. Allocating and deallocating data in the heap memory is already expensive without the GC, so you minimize the use and creation of complex types as much as you can.

### Example of Managing GC

Here's a simple example of how you might configure and trigger garbage collection in a Go program:

```go
package main

import (
    "fmt"
    "runtime"
    "runtime/debug"
)

func main() {
    // Set the GC target percentage to 200%
    debug.SetGCPercent(200)
    fmt.Println("GC percent set to 200")

    // Allocate some memory
    data := make([]byte, 1<<20) // 1 MB
    fmt.Println("Allocated 1 MB of memory")

    // Manually trigger garbage collection
    runtime.GC()
    fmt.Println("Manual GC triggered")

    // Use the allocated memory to prevent it from being optimized away
    _ = data
}

```