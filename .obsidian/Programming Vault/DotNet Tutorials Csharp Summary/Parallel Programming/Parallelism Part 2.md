	
# Atomic Methods, Thread Safety and Race Conditions

## Atomic Methods

### Overview

  
Atomic Methods can be used comfortably in a multithreaded environment because they guarantee determinism, that is we will always obtain the same result, no matter how many threads try to execute the method simultaneously.

In the context of multithreading and parallel programming, atomic operations are those that appear to execute indivisibly. An atomic operation is uninterruptible; it either completes fully or has no effect. In contrast, non-atomic operations may be interrupted, leading to potential race conditions and data inconsistencies.

Here are examples of atomic and non-atomic operations:

#### Atomic Operations:

##### Example 1: Interlocked Increment (C#)

```csharp
int counter = 0;

// Atomic increment operation
Interlocked.Increment(ref counter);

```

In this example, `Interlocked.Increment` is an atomic operation that increments the value of `counter` in a thread-safe manner.


#### Non-Atomic Operations:

##### Example 1: Non-Atomic Increment (C#)

```csharp

int counter = 0;

// Non-atomic increment operation
counter++;


```

In this example, the `counter++` operation is not atomic. It involves multiple steps (read, increment, write), and another thread could interrupt, causing a race condition.

#### Important Note:

Choosing atomic or non-atomic operations depends on the context and requirements. Atomic operations are generally safer in concurrent scenarios to avoid race conditions, but they may have a performance cost. Non-atomic operations might be acceptable in situations where atomicity is not critical, and the performance gain is significant.

When dealing with multithreading, always consider synchronization mechanisms like locks, semaphores, or higher-level abstractions depending on your programming language and platform.

#### Characteristics of Atomic Methods:

1. **Indivisibility:**
    
    - An atomic operation is executed as a single, uninterruptible unit. Once started, it runs to completion without being interrupted by other operations.
2. **Consistency:**
    
    - Atomic operations maintain the consistency of data. If multiple threads are accessing shared data, atomicity ensures that the data is not left in an inconsistent state due to partial execution of an operation.
3. **Isolation:**
    
    - Atomic operations are isolated from the interference of other concurrent operations. They are shielded from the effects of concurrent reads or writes by other threads.
4. **No Interleaving:**
    
    - Atomic methods prevent interleaving of instructions from different threads. When one thread is executing an atomic operation, no other thread can simultaneously execute an atomic operation on the same data.
5. **No Race Conditions:**
    
    - Atomicity helps eliminate race conditions, where the outcome of the program depends on the timing and order of execution of threads. With atomic operations, the program behaves as if each thread executed its operations one at a time.
6. **Thread-Safe:**
    
    - Atomic methods contribute to thread safety by providing a way to perform operations on shared data without the need for explicit locks or synchronization mechanisms.
7. Also:
	- the operation will be completed successfully or will fail completely without making any modifications. This part is similar to database transactions where either all operations are successful or none are performed if there is at least one error.

#### Common Examples of Atomic Operations:

- **Read and Write of Primitive Types:**
    
    - Operations like reading and writing to primitive data types (e.g., integers, booleans) are typically atomic on most modern hardware architectures.
- **Compare-and-Swap (CAS):**
    
    - CAS is an atomic operation that checks if a variable's value is as expected and, if so, updates it to a new value. This is commonly used for implementing lock-free algorithms.
- **Increment and Decrement Operations:**
    
    - Increment (++) and decrement (--) operations on variables are often atomic for primitive types, but this may depend on the language and platform.
- **Test-and-Set:**
    
    - This is an atomic operation that sets a variable to a specified value and returns its previous value. It is commonly used in synchronization primitives.

#### How To Implement Atomic Methods

It's important to note that while some operations may be atomic at the hardware level, ensuring atomicity in a multi-threaded environment often requires the use of synchronization mechanisms like locks, semaphores, or atomic classes provided by concurrent programming libraries like the Interlocked class.

Using multiple atomic methods in the same operation may lead to deadlock problems. Atomic methods from the Interlocked class promise to be atomic by themselves. If more than one Interlocked class method is used in the same operations there is no promise that they will work as expected.

The most common way is combine atomic methods with  **locks**. Locks allow us to block other threads from executing a piece of code when the lock is activated. If you are working with collections,  use **concurrent collections** as well, which are specially designed to handle multithreaded scenarios. If we do not use proper mechanisms, then we will end up with unexpected results, corrupted data, or incorrect values.


## Thread Safety

### Overview

When we say that a method is thread-safe, we are saying that we can execute this method simultaneously from multiple threads without causing any kind of error. We know that we have thread safety when the application data is not corrupted if two or more threads try to perform operations on the same data at the same time.

### How To Achieve Thread-Safety

If within the method We added an external variable. Then we could have a problem with unexpected results in that variable. Something that we can use to mitigate this is to use a synchronization mechanism like using Interlocked or using locks.

If we need to transform objects, then we can use immutable objects to avoid the problems of corrupting those objects. 

Ideally, we should work with pure functions. Pure functions are those that return the same value for the same arguments (determinism) and do not cause secondary effects (self-contained). Self-contained deterministic functions.

## Race Conditions

### Overview

Race conditions occur in C# when we have a variable shared by several threads and these threads want to modify the variables simultaneously. The problem with this is that depending on the order of the sequence of operations done on a variable by different threads, the value of the variable will be different. Simple operations as increasing by one.

A variable is problematic if we do them in multithreaded scenarios on a shared variable. The reason is that even increasing by 1 a variable or adding 1 to the variable is problematic. This is because the operation is not atomic. A simple variable increment is not an atomic operation.

### Understanding Race Conditions

This is what happen when 2 threads are reading, incrementing and writing from and to the same variable sequentially:

![[ParallelOverheadSequential1.webp]]![[ParallelOverheadSequential2.webp]]

It is obvious that the final value of that variable will be 2 every time. Which means when 2 threads increment a variable sequentially we are doing a deterministic operation.

Now if we do this simultaneously/in parallel it looks like this:
![[ParallelOverheadSimultaneous1.webp]]

Look at how the read, increment and write operations happen when the threads are executed simultaneously. Now that is no way to know the final result... it could be 1 but it could also be 2. We just achieved a race condition.

This means  depending on the order of the execution of the methods, we are going to determine the value of the variable. So, even though we increase the value twice in different threads because we were in a multithreaded environment,. we had a Race condition, which means that now we do not have a deterministic operation because sometimes it could be one. Sometimes the value of the variable could be two. It all depends on chance.

# Synchronization Mechanisms

## Overview

Synchronization mechanisms allows us to achieve thread safety, stop race conditions generation and can also be necessary for creation of atomic methods and operations.

**The `Interlocked` class performs better than locks**, so it should be used every time when possible. The problem is that not always the `Interlocked` class will achieve thread safety, when that happens we have to use locks instead of the `Interlocked` class.

Example of `Interlocked` class not achieving thread safety:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

namespace InterlockedDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            long IncrementValue= 0;
            long SumValue = 0;
            Parallel.For(0, 100000, number =>
            {
                Interlocked.Increment(ref IncrementValue);
                Interlocked.Add(ref SumValue, IncrementValue);
            });
            
            Console.WriteLine($"Increment Value With Interlocked: {IncrementValue}");
            Console.WriteLine($"Sum Value With Interlocked: {SumValue}");

            Console.ReadKey();
        }
    }
}
```

We are going to get different Sum Values even after using Interlocked. **Why**? This is because there is a Race condition. Then you might be thinking that we are using `Interlocked.Add` method and there should not be any race conditions. Right? But there is a Race condition because of the following.

Individually Increment and Add methods are thread-safe but the union of these two methods is not thread-safe.
So when you combine multiple `Interlocked` class methods your  code might not achieve thread-safety and when this happens you need to use locks and just ditch the `Interlocke` class all together:

```csharp
using System;
using System.Threading.Tasks;

namespace InterlockedDemo
{
    class Program
    {
        static object lockObject = new object();

        static void Main(string[] args)
        {
            long IncrementValue= 0;
            long SumValue = 0;
            
            Parallel.For(0, 10000, number =>
            {
                //Before lock Parallel 

                lock(lockObject)
                {
                    IncrementValue++;
                    SumValue += IncrementValue;
                }

                //After lock Parallel 
            });
            
            Console.WriteLine($"Increment Value With lock: {IncrementValue}");
            Console.WriteLine($"Sum Value With lock: {SumValue}");

            Console.ReadKey();
        }
    }
}
```
Now we will get the same result every time.

**Always have a dedicated object for the Lock in C# (an object that only exists to be a lock). Do not try to reuse the objects and also try to keep it simple. Try to make the least amount of work inside of a lock because having too much work inside of a lock could have an impact on the performance of your application.**
## Interlocked Class

### Overview

Interlocked class gives us a few methods that allow us to perform certain operations safely or atomically, even if the code is going to be executed by several threads simultaneously. 

The Interlocked class is great for getting rid of race conditions

### Properties

`Interlocked` class does not have properties in the traditional sense. However, one can consider the various methods as operation properties that directly manipulate shared variables in a thread-safe manner.

While `Interlocked` doesn't have properties, some methods effectively serve as atomic operations for properties or fields in a multi-threaded environment. 

This is valuable information:
**For example, `Interlocked.CompareExchange` can be used to implement a thread-safe property setter**. Here's a simple example using `Interlocked.CompareExchange` for a property-like operation:

`private int _myProperty;  public int MyProperty {     
`get     
`{         return _myProperty;     }     
`set     {         Interlocked.CompareExchange(ref _myProperty, value, _myProperty);     }
`}`

In this example, the `Interlocked.CompareExchange` method is used to update `_myProperty` atomically. It compares the current value of `_myProperty` with its expected value (which is also the current value) and updates it to the new value only if the comparison is true. This ensures that the operation is atomic and thread-safe.

**Keep in mind that properties themselves are not directly atomic; their individual read and write operations could be interrupted by other threads**. The usage of `Interlocked` methods helps ensure atomicity when dealing with shared variables, which is essential for proper synchronization in a multithreaded environment.
### Methods

These operations ensure that the variable is updated in an atomic, thread-safe manner, without the need for explicit locks. Here are some of the most important methods from the `Interlocked` class:

#### 1. **`Interlocked.Add` Method:**
    
    - Adds two integers and replaces the first integer with the sum.
    
    csharpCopy code
    
    `int result = Interlocked.Add(ref variable, valueToAdd);`
    
#### 2. **`Interlocked.Decrement` Method:**
    
    - Decrements a specified variable and stores the result.
    
    csharpCopy code
    
    `int result = Interlocked.Decrement(ref variable);`
    
#### 3. **`Interlocked.Exchange` Method:**
    
    - Sets a variable to a specified value and returns the original value. Equivalent to = assignment operation.
    
    csharpCopy code
    
    `int originalValue = Interlocked.Exchange(ref variable, newValue);`
    
#### 4. **`Interlocked.CompareExchange` Method:**
    
    - Compares variable value and expected value for equality and, if they are equal, replaces variable value with new Value. Returns original value. Equivalent to = assignment operation + condition that variable value must be equal to expected value.

```csharp
using System;
using System.Threading;

class Program
{
    private static int _myProperty = 0;

    static void Main()
    {
        int expectedValue = 0;
        int newValue = 42;

        // Try to set the property to the new value only if the current value matches the expected value
        if (Interlocked.CompareExchange(ref _myProperty, newValue, expectedValue) == expectedValue)
        {
            Console.WriteLine("Property updated successfully.");
        }
        else
        {
            Console.WriteLine("Property update failed. Current value is not equal to the expected value.");
        }

        Console.ReadLine();
    }
}

```

In this example, the `Interlocked.CompareExchange` method is used to try updating `_myProperty` to a new value (`newValue`) only if its current value matches the expected value (`expectedValue`). If the current value is equal to the expected value, the property is updated atomically, and a success message is displayed. If the current value is not equal to the expected value, the update is considered unsuccessful, and a failure message is displayed.

#### 5. **`Interlocked.Increment` Method:**
    
    - Increments a specified variable and stores the result.
    
    csharpCopy code
    
    `int result = Interlocked.Increment(ref variable);`
    
#### 6. **`Interlocked.Read` Method:**
    
    - Reads an int variable without the risk of encountering torn values (partial updates) in multi-threaded scenarios.
    
    csharpCopy code
    
    `int value = Interlocked.Read(ref variable);`
    
#### 7. **`Interlocked.Exchange` Method (for long type):**
    
    - Similar to `Interlocked.Exchange` but for `long` variables.
    
    csharpCopy code
    
    `long originalValue = Interlocked.Exchange(ref longVariable, newLongValue);`
    
#### 8. **`Interlocked.CompareExchange` Method (for long type):**
    
    - Similar to `Interlocked.CompareExchange` but for `long` variables.
    
    csharpCopy code
    
    `long originalValue = Interlocked.CompareExchange(ref longVariable, newLongValue, expectedLongValue);`
    

These methods help ensure atomicity and thread safety for common operations on shared variables. They are particularly useful in scenarios where multiple threads are accessing and modifying the same data to avoid race conditions and maintain data integrity. When used correctly, these methods help mitigate the need for explicit locks and improve the performance of multithreaded code.

#### 9. `Interlocked.Increment()`

Static method of the class. Increments a variable and stores the result in an atomic way. We need to specify the variable with the ref keyword as shown in the below example.



```csharp
static void Main(string[] args)

{

	var ValueInterlocked = 0;
	
	Parallel.For(0, 100000, _ =>
	
	{
	
		//Incrementing the value
		
		Interlocked.Increment(ref ValueInterlocked);
		
	});
	
	Console.WriteLine("Expected Result: 100000");
	
	Console.WriteLine($"Actual Result: {ValueInterlocked}");
	
	Console.ReadKey();

}
```
The actual result will be the expected result.



Sometimes Interlocked is not enough. Sometime we can only allow one or a few threads to have access to a critical section. For that we have locks.

## Locks

with locks, we can have a block of code that will only be executed by one/some thread/s at a time. That is, we limit a part of our code to be sequential/have limited concurrency, even if several threads try to execute that code at the same time.

We use locks when we need to perform several operations or an operation not covered by Interlocked.

*Something important to take into account is that ideally what we do inside a lock block should be relatively fast. This is because the threads are blocked while waiting for the release of the lock. And if you have multiple threads blocked for a longer period of time, this can have an impact on the speed of your application.


```csharp
class Program

{

	static object lockObject = new object();
	
	static void Main(string[] args)
	
	{
	
		var ValueWithLock = 0;
		
		Parallel.For(0, 100000, _ =>
		
		{
			lock(lockObject)
			{
				//Incrementing the value
				ValueWithLock++;
			}
		});
	
		Console.WriteLine("Expected Result: 100000");
		
		Console.WriteLine($"Actual Result: {ValueWithLock}");
		
		Console.ReadKey();
	
	}

}
```

# Difference in Multithreading vs Parallelism vs Asynchronous Programming

## Overview

This just became the webpage: [[https://dotnettutorials.net/lesson/multithreading-vs-asynchronous-programming-vs-parallel-programming-in-csharp/]] just go read it there. This time it was really well written.

1. **Multithreading:** a single process split into multiple threads.
2. **Parallel Programming:** multiple tasks running on multiple cores simultaneously.
3. **Asynchronous Programming:** a single thread initiating multiple tasks without waiting for each to complete.

## Multithreading

capability to create and manage multiple threads within a single process. A thread is the smallest unit of execution within a process, and multiple threads can run concurrently, sharing the same resources of the parent process but executing different code paths.

1. Multiple Threads are execute our application code by context switching
2. Context switching implies that only one thread is being executed at any given point in time. Creating an illusion of True Concurrency (simultaneous execution). It is true that multiple threads are being executed, but not simultaneously. Which is what I like to call false Concurrency.
3. While one thread is blocked executing his share of the application's code,  the other threads can keep executing their share independently from the one that is blocked, which improves performance

###### **Basics:**

- Every C# application starts with a single thread, known as the main thread.
- Through the .NET framework, C# provides classes and methods to create and manage additional threads.

###### **Core Classes & Namespaces:**

- The primary classes for thread management in C# are found in the System.Threading namespace.
- **Thread**: Represents a single thread. It provides methods and properties to control and query the state of a thread.
- **ThreadPool**: Provides a pool of worker threads that can be used to execute tasks, post work items, and process asynchronous I/O operations.

###### **Advantages:**

- **Improved Responsiveness:** In GUI applications, a long-running task can be moved to a separate thread to keep the UI responsive.
- **Better Resource Utilization:** Allows more efficient CPU use, especially on multi-core processors.

###### **Challenges:**

- **Race Conditions:** Occur when two threads access shared data and try to change it simultaneously.
- **Deadlocks:** Occur when two or more threads are waiting for each other to release resources, resulting in a standstill.
- **Resource Starvation** occurs when a thread is continually denied access to resources and can’t proceed with its work.

To address these challenges, synchronization primitives like Mutex, Monitor, Semaphore, and lock keywords in C# are used.

###### **Considerations:**

- Creating too many threads can degrade the application performance due to context-switching overhead.
- Threads consume resources, so that excessive use can degrade performance and responsiveness.
- Synchronization can introduce its own overhead, so it’s essential to strike a balance.


## Asynchronous Programming

Asynchronous programming in C# is a method of performing tasks without blocking the calling thread. This is especially beneficial for I/O-bound operations (like reading from a file, fetching data from the web, or querying a database), where waiting for the task to be completed might waste valuable CPU time that could be better spent doing other work.

The important point that you need to keep in mind is that if the thread performs some IO Operation, it will not wait till the IO Operation is completed, which means the thread will not block; it will come back to the Thread Pool and the same thread can handle another request. In this way, it will use the CPU resources effectively, i.e., it can handle more client requests and improve the application’s overall performance.

###### **Basics:**

- **async and await** are the two primary keywords introduced in C# 5.0 to simplify asynchronous programming. When a method is marked with async, it can use the await keyword to call other methods that return a Task or `Task<T>`. The await keyword effectively tells the compiler: “If the task isn’t done yet, let other stuff run until it is.”

###### **Core Classes and Namespaces:**

- **Task:** Represents an asynchronous operation that can be awaited. Found in the System.Threading.Tasks namespace.
- **`Task<T>`:** Represents an asynchronous operation that returns a value of type T.
- `**TaskCompletionSource<T>`:** Represents the producer side of a `Task<T>`, allowing the creation of asynchronous operations not based on async and await.

###### **Advantages:**

- **Responsiveness:** In UI applications, using asynchronous methods can keep the UI responsive because the UI thread isn’t blocked.
- **Scalability:** In server-side applications, asynchronous operations can increase throughput by allowing the system to handle more requests. This is achieved by freeing up threads that otherwise would be blocked.

###### **Considerations:**

- Using async and await doesn’t mean you’re introducing multithreading. It’s about the efficient use of threads.
- Avoid async void methods, as they can’t be awaited, and exceptions thrown inside them can’t be caught outside. They’re primarily for event handlers.
- Asynchronous code can sometimes be more challenging to debug and reason about, especially when dealing with exceptions or coordinating multiple asynchronous operations.

The power of asynchronous programming in C# lies in its ability to improve both responsiveness and scalability, but it requires understanding the underlying principles to use effectively and avoid pitfalls.

## Parallel Programming

Parallel programming in C# is the process of using concurrency to execute multiple computations simultaneously to improve the performance and responsiveness of software.  The same task will be executed by multiple processes, multiple cores where each process has multiple threads to execute the application code.

###### **Basics:**

- **Data Parallelism:** It refers to scenarios where the same operation is performed concurrently (in parallel) on elements in a collection or partition of data.
- **Task Parallelism:** This is about running several tasks in parallel. Tasks can be distinct operations that are executed concurrently.

###### **Core Classes and Namespaces:**

- `**System.Threading.Tasks`:** This namespace contains the TPL’s primary types, including Task and Parallel.
- **Parallel:** A static class that provides methods for parallel loops like `Parallel.For` and `Parallel.ForEach`.
- **PLINQ (Parallel LINQ):** A parallel version of LINQ that can be used to perform parallel operations on collections.

###### **Advantages:**

- **Performance**: By leveraging multiple processors or cores, computational tasks can be completed faster.
- **Responsiveness**: In desktop applications, long-running computations can be offloaded to a background thread, making the application more responsive.

###### **Considerations:**

- **Overhead:** There’s an inherent overhead in dividing tasks and aggregating results. Not all tasks will benefit from parallelization.
- **Ordering:** Parallel operations might not respect the original order of data, especially in PLINQ. If order is crucial, you’d need to introduce order preservation, which could reduce performance benefits.
- **Synchronization:** When multiple tasks access shared data, synchronization mechanisms like lock or Concurrent Collections are needed to prevent race conditions.
- **Max Degree of Parallelism:** Limiting the number of concurrent tasks using properties like` MaxDegreeOfParallelism` in PLINQ is possible.

Parallel programming utilizes multiple cores to improve computational performance, whereas asynchronous programming is about improving responsiveness by not waiting for long-running tasks to be completed.

Using parallel programming in C# effectively requires a good understanding of your problem domain, the nature of your data and operations, and the challenges of concurrent execution. It’s essential to profile and test any parallel solution to ensure it offers genuine benefits over a sequential approach.

## Differences Between Them

#### **Multithreading:**

- **Definition:** Multithreading is about allowing a single process to manage multiple threads, which are the smallest units of a CPU’s execution context. Each thread can run its own set of instructions.
- **In C#:** The **System.Threading.Thread** class is commonly used to manage threads. The ThreadPool is another option that provides a pool of worker threads.
- **Use Cases:** It is suitable for both IO-bound and CPU-bound tasks. It’s especially effective for tasks that can run concurrently without waiting for other tasks to complete or for tasks that can be broken down into smaller, independent chunks.
- **Pros:** You can better use a multi-core CPU by spreading threads across cores if managed properly.
- **Cons:** Threads can be resource-intensive. Managing threads manually can lead to problems like deadlocks, race conditions, and increased context switching, which can degrade performance.

##### **Asynchronous Programming:**

- **Definition:** Asynchronous programming allows a system to work on one task without waiting for another to complete. It’s more about the flow of control than about concurrent execution.
- **In C#:** The **async** and **await** keywords, introduced in C# 5.0, make asynchronous programming much more straightforward. The **Task** and `**Task<T>`** classes in the **System.Threading.Tasks** namespace are integral parts of async programming in C#.
- **Use Cases:** Especially useful for IO-bound tasks ((like file or database operations, web requests, restful services call)) where you don’t want to wait (or block) for the operation to complete before moving on to another task. Think of making a web request and waiting for the response. Rather than waiting, asynchronous programming allows the system to work on other tasks during the wait time.
- **Pros:** Improves responsiveness, especially in UI applications. It can be easier to manage than raw threads.
- **Cons:** Asynchronous code can be harder to debug. It can introduce its own complexities, primarily if not understood well. Requires a certain understanding of the underlying mechanics to avoid pitfalls (like deadlocks).

##### **Parallel Programming:**

- **Definition:** Parallel programming is about breaking down a task into sub-tasks that are processed simultaneously, often spread across multiple processors or cores. So, it focuses on executing multiple tasks or even multiple parts of a specific task simultaneously by distributing the task across multiple processors or cores.
- **In C#:** The **System.Threading.Tasks.Parallel** class provides methods for parallel loops (like Parallel.For and Parallel.ForEach). The PLINQ (Parallel LINQ) extension methods can also be used for parallel data operations. It also provides Parallel.Invoke method to execute multiple methods parallelly.
- **Use Cases:** Best for CPU-bound operations where a task can be divided into independent sub-tasks that can be processed simultaneously. For example, processing large datasets or performing complex calculations.
- **Pros:** Can significantly speed up processing time for large tasks by utilizing all available cores or processors.
- **Cons**: Not all tasks are easily parallelizable. Overhead in splitting tasks and gathering results. This introduces the potential for race conditions if shared resources aren’t properly managed.

###### **Important Notes:**

It’s important to understand that these concepts can and often do overlap. For instance:

- Multithreading and Parallel Programming often go hand-in-hand because parallel tasks are frequently run on separate threads.
- Asynchronous programming can be multi-threaded, especially when tasks are offloaded to a separate thread.


###### **Choosing the Right Approach:**

The key is to choose the right approach for the right problem:

- **IO-Bound Operations:** Asynchronous programming is best suited for IO-Bound Operations.
- **CPU-Bound Operations:** Parallel Programming and Multithreading best suitable for CPU-Bound Operations.

It’s also important to note that adding parallelism or multithreading doesn’t always mean better performance. Overhead and context switching can sometimes make a multi-threaded solution slower than a single-threaded one. Proper profiling and understanding of the underlying problems are essential.