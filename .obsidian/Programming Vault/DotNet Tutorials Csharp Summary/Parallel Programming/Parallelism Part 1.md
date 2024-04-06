


# Introduction to Parallelism
## Overview

 With parallelism, we can use our processor to perform several actions simultaneously. So, with parallelism, we have the opportunity to improve the speed of certain processes of our programs.
 
The main benefit of parallelism is saving time. Time is saved by maximizing the use of computer resources. The idea is that if the computer allows the use of multi-threading, we can use these threads when we have a task to solve. So, instead of underusing our processor using a single thread, we can use as many threads as we can to speed up the processing of the task.


We use parallelism in CPU-bound operations, CPU bound operations are those operations whose resolution depends on the processor, not on services external to the application. Doing an arithmetic operation will be an example of a CPU-bound operation. Taking a set of images and applying filters and transformations through them will be another CPU-bound operation.

You should not use Parallelism on all CPU-bound operations only the ones that are expensive enough to give us an increase in performance.

the processors are not getting much faster because of the physical limitations. What is being done then mainly is to include more cores in the Central Processing Unit.

If you try to use parallelism on a computer that cannot use parallelism, then the code will run in sequential. That is, it will not throw any errors, but it will not be faster either.

Multicore computers have a Central Processing Unit with multiple cores, usually these cores have 2 processors each. These means that for every core on your pc 2 operations can be done simultaneously, because the processors are the ones responsible for achieving parallelism

You need to remember that:
1. The Tasks must be independent.
2. The order of the execution does not matter




## When To Not Use Parallelism

### HTTP requests

An exception to the benefits of using parallelism using ASP.NET and ASP.NET Core since those scenarios are already parallelized. That is because every thread serves an HTTP request. And therefore, if you have one HTTP request that occupies several threads, then the Web server will have fewer resources to serve other HTTP requests. It is not recommended to occupy several threads for one HTTP request.

## Data Parallelism and Task Parallelism

**Data Parallelism:**  we have a collection of values and we want to use the same operation on each of the elements in the collection. The examples will be to filter the elements of an array in parallel or find the inverse of each matrix in a collection. This means each process does the same work on unique and independent pieces of data.

**Example:**

1. [**Parallel.For**](https://dotnettutorials.net/lesson/parallel-for-method-csharp/)
2. [**Parallel.ForEach**](https://dotnettutorials.net/lesson/parallel-foreach-method-csharp/)

**Task Parallelism:** Task Parallelism occurs when we have a set of independent tasks that we want to perform in parallel. An example would be if we want to send an email and SMS to a user, we can perform both operations in parallel if they are independent. This means each process performs a different function or executes different code sections that are independent.

1. [**`Parallel.Invoke`**](https://dotnettutorials.net/lesson/parallel-invoke-method-csharp/)
## TPL (Task Parallel Library)

TPL (Task Parallel Library) abstracts the low-level detail of thread handling, allowing us to run programs that run parallel without having to work with these threads manually.

We can’t expect our sequential program to run faster on the new processors as we know the processor technology advances means the focus is on Multicore-processors. Today’s desktop typically has 4 cores but the latest experimental multi-core chips have up to 1000 cores.

**Task Parallel Library (TPL)** allows for programs that target multi-core machines (automatically use multiple processors) which improves the performance, which means we can express the code as a Parallel task that will run concurrently on all the available processors.

We can have 2 threads being executed in "Core1", 1 thread in "core2" and another in "core3" simultaneously.

## PLINQ (Parallel Language Integrated Query)

In LINQ, we can filter the elements of an array. Then with Parallel LINQ, we can filter the same array in parallel. This allows us to use the cores of our processor to perform the evaluations of the elements of the array simultaneously.

If we have a collection, and if we want to use parallelism to process it, we have the option of using Parallel LINQ or PLINQ. 

Parallel LINQ (PLINQ) is basically the same as we have in LINQ. But with parallel functionality, we can define the maximum degree of parallelism and use a cancellation token to cancel the operation. 

PLINQ achieves parallelism by partitioning the source collection into segments and then processing these segments concurrently across multiple threads. 

It is possible to do any aggregate operation like Min(), Max(), Sum() and Average() with PLINQ that you can do with LINQ

The [`System.Linq.ParallelEnumerable`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable) class exposes almost all of PLINQ's functionality. It and the rest of the [`System.Linq`](https://learn.microsoft.com/en-us/dotnet/api/system.linq) namespace types are compiled into the System.Core.dll assembly.

[`ParallelEnumerable`](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable) includes implementations of all the standard query operators that LINQ to Objects supports, although it does not attempt to parallelize each one.

Here are the most used PLINQ extension methods:

|ParallelEnumerable Operator|Description|
|---|---|
|[AsParallel](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.asparallel)|The entry point for PLINQ. Specifies that the rest of the query should be parallelized, if it is possible.|
|[AsSequential](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.assequential)|Specifies that the rest of the query should be run sequentially, as a non-parallel LINQ query.|
|[AsOrdered](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.asordered)|Specifies that PLINQ should preserve the ordering of the source sequence for the rest of the query, or until the ordering is changed, for example by the use of an orderby (Order By in Visual Basic) clause.|
|[AsUnordered](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.asunordered)|Specifies that PLINQ for the rest of the query is not required to preserve the ordering of the source sequence.|
|[WithCancellation](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.withcancellation)|Specifies that PLINQ should periodically monitor the state of the provided cancellation token and cancel execution if it is requested.|
|[WithDegreeOfParallelism](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.withdegreeofparallelism)|Specifies the maximum number of processors that PLINQ should use to parallelize the query.|
|[WithMergeOptions](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.withmergeoptions)|Provides a hint about how PLINQ should, if it is possible, merge parallel results back into just one sequence on the consuming thread.|
|[WithExecutionMode](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.withexecutionmode)|Specifies whether PLINQ should parallelize the query even when the default behavior would be to run it sequentially.|
|[ForAll](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.forall)|A multithreaded enumeration method that, unlike iterating over the results of the query, enables results to be processed in parallel without first merging back to the consumer thread.|
|[Aggregate](https://learn.microsoft.com/en-us/dotnet/api/system.linq.parallelenumerable.aggregate) overload|An overload that is unique to PLINQ and enables intermediate aggregation over thread-local partitions, plus a final aggregation function to combine the results of all partitions.|


### Regular LINQ Query

```csharp
using System;
using System.Linq;

namespace ParallelLINQDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating a Collection of integer numbers
            var numbers = Enumerable.Range(1, 20);

            //Fetching the List of Even Numbers using LINQ
            var evenNumbers = numbers.Where(x => x % 2 == 0).ToList(); //this is the LINQ part of the code

            Console.WriteLine("Even Numbers Between 1 and 20");
            foreach (var number in evenNumbers)
            {
                Console.WriteLine(number);
            }
            
            Console.ReadKey();
        }
    }
}
```

Output:

```csharp
Even Numbers Between 1 and 20
2
4
6
8
10
12
14
16
18
20
```


### Same Query But Now It Is An PLINQ Query

To use PLINQ, you typically need to add a call to the `AsParallel` method to your LINQ query, which instructs the system to process the query in parallel if it’s deemed beneficial.

```csharp
using System;
using System.Linq;

namespace ParallelLINQDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating a Collection of integer numbers
            var numbers = Enumerable.Range(1, 20);

            //Fetching the List of Even Numbers using LINQ
            //var evenNumbers = numbers.Where(x => x % 2 == 0).ToList();

            //Fetching the List of Even Numbers using PLINQ
            //PLINQ means we need to use AsParallel()
            var evenNumbers = numbers.AsParallel().Where(x => x % 2 == 0).ToList();

            Console.WriteLine("Even Numbers Between 1 and 20");
            foreach (var number in evenNumbers)
            {
                Console.WriteLine(number);
            }
            
            Console.ReadKey();
        }
    }
}
```
output: the values in the output are going to be the same but they are going to appear in a random order since multiple iterations of the query are happening simultaneously so we do not know which one is going to finish first.

#### **How to Maintain the Original Order in Parallel LINQ?**

Suppose you want the output to be in order. In that case, you need to use the `AsOrdered()` method after the `AsParallel()` method

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace ParallelLINQDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating a Collection of integer numbers
            var numbers = Enumerable.Range(1, 20);
            
            //Fetching the List of Even Numbers using PLINQ
            //PLINQ means we need to use AsParallel()
            var evenNumbers = numbers
                .AsParallel() //Parallel Processing
                .AsOrdered() //Original Order of the numbers
                .Where(x => x % 2 == 0)
                .ToList();

            Console.WriteLine("Even Numbers Between 1 and 20");
            foreach (var number in evenNumbers)
            {
                Console.WriteLine(number);
            }
            
            Console.ReadKey();
        }
    }
}
```
output: The order will be the original order in which the elements are stored in the numbers collections.

##### **Difference Between `OrderBy` and `AsOrdered`
Method in C#:**

You might be interested in using the LINQ OrderBy method after the Where Extension method. But remember, the OrderBy method is used to sort the data, not to maintain the original order of the data. On the other hand, the AsOrdered method is used to maintain the original order of the elements:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
namespace ParallelLINQDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating a Collection of integer numbers
            List<int> numbers = new List<int>()
            {
                1, 2, 6, 7, 5, 4, 10, 12, 13, 20, 18, 9, 11, 15, 14, 3, 8, 16, 17, 19
            };

            //Using AsOrdered Method
            //Fetching the List of Even Numbers using PLINQ
            var evenNumbers1 = numbers
                .AsParallel() //Parallel Processing
                .AsOrdered() //Original Order of the numbers
                .Where(x => x % 2 == 0)
                .ToList();

            Console.WriteLine("Even Numbers Between 1 and 20 using AsOrdered");
            foreach (var number in evenNumbers1)
            {
                Console.WriteLine(number);
            }
            
            //Using OrderBy Method
            //Fetching the List of Even Numbers using PLINQ
            var evenNumbers2 = numbers
                .AsParallel() //Parallel Processing
                .Where(x => x % 2 == 0)
                .OrderBy(x => x) //Sort the Elements in Ascending Order
                .ToList();

            Console.WriteLine("\nEven Numbers Between 1 and 20 using OrderBy");
            foreach (var number in evenNumbers2)
            {
                Console.WriteLine(number);
            }

            Console.ReadKey();
        }
    }
}
```

output:
with the AsOrdered method, we are getting the data in its original order of the sequence. With the OrderBy method, we are getting the data in ascending order:
```csharp
Even Numbers Between 1 and 20 using AsOrdered
2
6
4
10
12
20
18
14
8
16

Even Numbers Between 1 and 20 using OrderBy

2
4
6
8
10
12
14
16
18
20
```

### Maximum Degree Of Parallelism and Cancellation Token With PLINQ

Both of these things can also be achieved in PLINQ:

```csharp
using System;
using System.Linq;
using System.Threading;

namespace ParallelLINQDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating an instance of CancellationTokenSource
            var CTS = new CancellationTokenSource();

            //Setting the time when the token is going to cancel the Parallel Operation
            CTS.CancelAfter(TimeSpan.FromMilliseconds(200));

            //Creating a Collection of integer numbers
            var numbers = Enumerable.Range(1, 20);
            
            //Fetching the List of Even Numbers using PLINQ
            var evenNumbers = numbers
                .AsParallel() //Parallel Processing
                .AsOrdered() //Original Order of the numbers
                .WithDegreeOfParallelism(2) //Maximum of two threads can process the data
                .WithCancellation(CTS.Token) //Cancel the operation after 200 Milliseconds
                .Where(x => x % 2 == 0) //This logic will execute in parallel
                .ToList();

            Console.WriteLine("Even Numbers Between 1 and 20");
            foreach (var number in evenNumbers)
            {
                Console.WriteLine(number);
            }
            
            Console.ReadKey();
        }
    }
}
```
### **Points to Remember When Working with PLINQ:**

There are several things to keep in mind when using PLINQ:

- **Ordering:** PLINQ processes elements in parallel and doesn’t guarantee the order of the results unless you explicitly ask for it using the `AsOrdered` method. Preserving order can add overhead and potentially reduce performance gains.
- **Side Effects:** Queries should be side-effect-free. Since PLINQ can process multiple elements simultaneously, using functions that have side effects can result in unexpected behavior or race conditions.
- **Exception Handling:** When an exception is thrown in a PLINQ query, it is wrapped in an AggregateException. This exception contains all the individual exceptions thrown during the execution of the query.
- **Performance:** Not all queries run faster in parallel. PLINQ has overhead, so for small collections or fast operations, the overhead might outweigh the benefits of parallelism. Measuring performance for sequential and parallel versions of your query is generally a good idea.
- **Forcing Parallelism:** You can force PLINQ to parallelize the query using the WithExecutionMode method with ParallelExecutionMode.ForceParallelism is not recommended unless you are certain that parallelism will improve performance.
- **Degree of Parallelism:** You can specify the number of processors to use in a PLINQ query by using the WithDegreeOfParallelism method. This can be useful for fine-tuning performance, particularly if you want to leave some cores free for other tasks.

PLINQ is particularly useful when working with large collections or performing complex processing where tasks can be executed in parallel. It is a powerful tool, but it should be used judiciously and tested to ensure that it provides performance benefits in each specific case.

### **When to Use Parallel LINQ in C#?**

Parallel LINQ (PLINQ) in C# should be used when you have computationally intensive queries that can benefit from parallel execution to improve performance. Here are some scenarios where PLINQ is particularly useful:

- **Large Data Sets:** When processing large data collections, PLINQ can significantly reduce the time it takes by dividing the work across multiple threads.
- **CPU-Intensive Operations:** If your LINQ queries involve complex calculations or CPU-bound processing, PLINQ can help distribute the load across multiple cores.
- **Multi-Core Processors:** If the environment where your application is running has multiple CPU cores available, PLINQ can utilize these cores to perform operations in parallel.
- **Long-Running Queries:** For operations that naturally take a long time to execute, parallelizing the work can lead to better application responsiveness, especially in desktop or server environments.



## Concepts and Mechanisms of Parallel class and related classes

**Always benchmark the efficient of the mechanisms to see if implementing parallelism actually increases performance**


### `ParallelOptions` Class

**This class provides options to limit the number of concurrently executing loop methods.**

#### Properties

##### 1.` int ` `MaxDegreeOfParallelism` :  
the number of concurrent operations run by Parallel method calls that are passed this `ParallelOptions` instance. A positive property value limits the number of concurrent operations to the set value. If it is -1, there is no limit on the number of concurrently running operations. It will throw `ArgumentOutOfRangeException` if the property is being set to zero or to a value that is less than -1. -1 is the default value which sets that there is no limitation of the concurrent tasks to be executed.**** 

```csharp
static void Main(string[] args)

{

//Limiting the maximum degree of parallelism to 2

var options = new ParallelOptions()

{

MaxDegreeOfParallelism = 2

};

int n = 10;

Parallel.For(0, n, options, i =>

{

Console.WriteLine(@"value of i = {0}, thread = {1}",

i, Thread.CurrentThread.ManagedThreadId);

Thread.Sleep(10);

});

Console.WriteLine("Press any key to exist");

Console.ReadLine();

}
```

**As per the industry standard, we need to set the Maximum Degree of Parallelism to the number of processors available in the machine minus 1. In order to check the number of processors on your machine, open Task Manager and then select the Performance tab and select CPU as shown in the below image. **

**But we are not developing applications to run on our machine. We are developing the application for the client and we don’t know the number of logical processors on the client’s machine. So, we should not hard code the value. Instead, C# provides **`Environment.ProcessorCount`** property will give us the number of logical processors on the machine on which the application is running. So, we need to set the Maximum Degree of Parallelism in C# as `Environment.ProcessorCount - 1.**

##### 2. **public T`askScheduler TaskScheduler` {get; set;}:** It is used to get or set the `TaskScheduler` associated with this `ParallelOptions` instance. Setting this property to null indicates that the current scheduler should be used. It returns the task scheduler that is associated with this instance.

##### 3. **public `CancellationToken CancellationToken` {get; set;}:** It is used to get or set the `CancellationToken` associated with this `ParallelOptions` instance. It returns the token that is associated with this instance.

In order to Cancel the Parallel Operations in C#, first, we need to create an instance of `ParallelOptions `class and then we need to create an instance of `CancellationTokenSource` and then we need to set the `CancellationToken` properties of `ParallelOptions` instance to the token of the `CancellationTokenSource` instance:

```csharp
class Program

{

	static void Main(string[] args)
	
	{
	
		//Create an Instance of CancellationTokenSource
		
		var CTS = new CancellationTokenSource();
		
		//Set when the token is going to cancel the parallel execution
		
		CTS.CancelAfter(TimeSpan.FromSeconds(5));
		
		//Create an instance of ParallelOptions class
		
		var parallelOptions = new ParallelOptions()
		
		{
			
			MaxDegreeOfParallelism = 2,
			
			//Set the CancellationToken value
			
			CancellationToken = CTS.Token
			
		};
		
		try
		
		{
		
			Stopwatch stopwatch = new Stopwatch();
			
			stopwatch.Start();
			
			//Passing ParallelOptions as the first parameter
			
			Parallel.Invoke(
			
			parallelOptions,
			
			() => DoSomeTask(1),
			
			() => DoSomeTask(2),
			
			() => DoSomeTask(3),
			
			() => DoSomeTask(4),
			
			() => DoSomeTask(5),
			
			() => DoSomeTask(6),
			
			() => DoSomeTask(7)
			
			);
			
			stopwatch.Stop();
			
			Console.WriteLine($"Time Taken to Execute all the Methods : {stopwatch.ElapsedMilliseconds/1000.0} Seconds");
			
		}
		
		//When the token cancelled, it will throw an exception
		
		catch(Exception ex)
		
		{
		
			Console.WriteLine(ex.Message);
			
		}
		
		finally
		
		{
		
			//Finally dispose the CancellationTokenSource and set its value to null
			
			CTS.Dispose();
			
			CTS = null;
		
		}
		
		Console.ReadLine();
		
	}
		
	static void DoSomeTask(int number)
		
	{
		
	Console.WriteLine($"DoSomeTask {number} started by Thread {Thread.CurrentThread.ManagedThreadId}");
	
	//Sleep for 2 seconds
		
	Thread.Sleep(TimeSpan.FromSeconds(2));
		
	Console.WriteLine($"DoSomeTask {number} completed by Thread {Thread.CurrentThread.ManagedThreadId}");
		
	}

}
```

When you run the application, please observe the output carefully. Here, it started the execution parallelly by using two threads. It will continue execution until the token is canceled i.e. for 4 seconds. As soon as the token is canceled, the parallel execution stopped and it will throw the token canceled exception which is handled by the catch block, and in the catch block, we just print the exception message that is what you see in the last statement of the output.

Same Thing In a Parallel For Each Loop:

```csharp
static void Main(string[] args)

{

	//Create an Instance of CancellationTokenSource
	
	var CTS = new CancellationTokenSource();
	
	//Set when the token is going to cancel the parallel execution
	
	CTS.CancelAfter(TimeSpan.FromSeconds(5));
	
	//Create an instance of ParallelOptions class
	
	var parallelOptions = new ParallelOptions()
	
	{
	
		MaxDegreeOfParallelism = 2,
		
		//Set the CancellationToken value
		
		CancellationToken = CTS.Token
	
	};
	
	try
	
	{
	
		List<int> integerList = Enumerable.Range(0, 20).ToList();
		
		Parallel.ForEach(integerList, parallelOptions, i =>
		
		{
	
		Thread.Sleep(TimeSpan.FromSeconds(1));
		
		Console.WriteLine($"Value of i = {i}, thread = {Thread.CurrentThread.ManagedThreadId}");
		
		});
		
	}
	
	//When the token canceled, it will throw an exception
	
	catch (Exception ex)
	
	{
	
		Console.WriteLine(ex.Message);
	
	}
	
	finally
	
	{
	
		//Finally dispose the CancellationTokenSource and set its value to null
		
		CTS.Dispose();
		
		CTS = null;
		
		}
		
		Console.ReadLine();

	}

}
```

Same thing with a Parallel For Loop:

```csharp
class Program

{

	static void Main(string[] args)
	
	{
	
		//Create an Instance of CancellationTokenSource
		
		var CTS = new CancellationTokenSource();
		
		//Set when the token is going to cancel the parallel execution
		
		CTS.CancelAfter(TimeSpan.FromSeconds(5));
		
		//Create an instance of ParallelOptions class
		
		var parallelOptions = new ParallelOptions()
		
		{
		
			MaxDegreeOfParallelism = 2,
			
			//Set the CancellationToken value
			
			CancellationToken = CTS.Token
		
		};
		
		try
		
		{
		
			Parallel.For(1, 21, parallelOptions, i => {
			
			Thread.Sleep(TimeSpan.FromSeconds(1));
			
			Console.WriteLine($"Value of i = {i}, thread = {Thread.CurrentThread.ManagedThreadId}");
			
			});
		
		}
		
		//When the token canceled, it will throw an exception
		
		catch (Exception ex)
		
		{
		
			Console.WriteLine(ex.Message);
		
		}
		
		finally
		
		{
		
			//Finally dispose the CancellationTokenSource and set its value to null
			
			CTS.Dispose();
			
			CTS = null;
		
		}
		
		Console.ReadLine();
	
	}

}
```
### Parallel For

#### Parallel For Loop

Executes iterations of a loop in parallel on multiple threads, potentially improving performance by utilizing multiple processors or cores more effectively. This approach is especially beneficial when executing CPU-bound operations.

When we execute a non parallel for loop we know the sequence of the results before we even execute the code (determinism), that happens because the iterations in the non parallel for loop are executed sequentially. The parallel for loop will display the results of different sequences every time because it's iterations are executed simultaneously, therefore it is impossible to know what iteration will finish first.

```csharp
static void Main(string[] args)

{

Console.WriteLine("C# Parallel For Loop");

//It will start from 1 until 10

//Here 1 is the start index which is Inclusive

//Here 11 us the end index which is Exclusive

//Here number is similar to i of our standard for loop

//The value will be store in the variable number

Parallel.For(1, 11, number => {

Console.WriteLine(number);

});

Console.ReadLine();

}
```


Parallel for loops should only be used if:
	1. The Next iteration does not need state updates from this iteration. (Iterations are independent from each other)
	2. The Operations being done by the iterators are heavy/expensive enough to actually increase the performance of our code instead of making it slower. (using multiple simultaneous cores is more expensive than just one, initially.)

```csharp
  
static void Main(string[] args)

{

Console.WriteLine("C# For Loop");

int number = 10;

for (int count = 0; count < number; count++)

{

//Thread.CurrentThread.ManagedThreadId returns an integer that

//represents a unique identifier for the current managed thread.

Console.WriteLine($"value of count = {count}, thread = {Thread.CurrentThread.ManagedThreadId}");

//Sleep the loop for 10 miliseconds

Thread.Sleep(10);

}

Console.WriteLine();

Console.WriteLine("Parallel For Loop");

Parallel.For(0, number, count =>

{

Console.WriteLine($"value of count = {count}, thread = {Thread.CurrentThread.ManagedThreadId}");

//Sleep the loop for 10 miliseconds

Thread.Sleep(10);

});

Console.ReadLine();

}
```

Output from parallel for loop:
```
Parallel For Loop
value of count = 0 ; thread =1
value of count = 1; thread =1
value of count = 2; thread =3
\value of count = 4; thread =5
value of count = 6; thread =4
value of count = 8; thread =6
value of count = 7; thread =1
value of count = 5; thread =3
value of count = 9; thread =5
value of count = 3; thread =4
```




#### Parallel For Each

In a standard` Foreach` loop, each iteration processes a single item from the collection and will only process all the items one by one. However, the Parallel `Foreach `method executes multiple iterations simultaneously on different processors or processor cores. This may open the possibility of synchronization problems. So, the loop is ideally suited to processes where each iteration is independent.

Syntax:
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static void Main()
    {
        int[] data = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
        Parallel.ForEach(data, (item, loopState) =>
        {
            Console.WriteLine($"Processing item {item} on thread {Task.CurrentId}");

            // Simulate a condition where you want to break out of the loop early
            if (item == 5)
            {
                Console.WriteLine("Breaking out of the loop early.");
                loopState.Break(); // Breaks out of the parallel loop
            }

            // Your processing logic here
            // ...
        });

        Console.ReadLine();
    }
}
```

- The first parameter is the collection of objects that will be enumerated. This can be any collection that implements `IEnumerable<T>`.
- The second parameter accepts an Action delegate, usually expressed as a lambda expression that determines the action to take for each item in the collection.


Most of what applies to parallel for loops also apply to for each loops


#### When To Use Parallel For Loops

- **Independent Operations:** Ideal for scenarios where you have a collection of items to process, and each item can be processed independently without affecting the others.
- **CPU-Bound Tasks:** Best suited for CPU-intensive tasks that can benefit from parallel execution. For example, complex calculations on elements of a large array.
- **Performance Optimization:** When a performance bottleneck in a loop processes many items or performs time-consuming operations, profiling indicates that parallelization could help.
- **Large Collections:** Useful when dealing with large collections where the performance gains from concurrent processing justify parallelization overhead.
- **Non-Blocking Operations:** When the operations within the loop do not involve blocking calls (like I/O operations). Blocking calls in a parallel loop can negate the benefits of parallelism and lead to thread pool starvation.
#### Parallel For **Considerations and Best Practices**

- **Thread Safety:** Ensure that your loop code is thread-safe. Avoid modifying shared data without proper synchronization mechanisms like locks, concurrent collections, or other thread synchronization techniques.
- **Order of Execution:** The iterations of a Parallel For loops do not run in a guaranteed order, and different iterations may run simultaneously. This is different from a regular for loop where each iteration runs sequentially.
- **Exception Handling:** Handling exceptions in a Parallel For loops can be more complex. You might use a `ParallelLoopState` object to stop or break the loop, and an `AggregateException` may be thrown if multiple iterations throw exceptions.
- **Performance:** While P`arallel.For`  and `Parallel.ForEach` can improve performance for CPU-bound operations, it might not be beneficial for I/O-bound operations. Additionally, for very short or simple loop bodies, the overhead of parallelization may outweigh its benefits.
- **Cancellation Support:** `Parallel.For` and `Parallel.ForEach` support task cancellation using the `CancellationToken` class. This can be useful for long-running operations that need to be stopped under certain conditions.
- **Return Values and Loop Control:** `Parallel.For` and `Parallel.ForEach` returns, a `ParallelLoopResult` which can be used to determine if the loop ran to completion or was stopped/prematurely exited. You can also control the loop execution using the `ParallelLoopState` object in the loop body.
- - **Avoid Shared State:** Ensure that the body of the loop does not depend on a shared state or has appropriate synchronization mechanisms to handle shared state safely.
- - **Balance Between Parallelism and Overhead:** Over-parallelizing (especially with many lightweight tasks) can lead to excessive context switching and thread management overhead, reducing overall performance.

#### **Alternatives To Parallel For and For each loops

- **PLINQ:** For operations on collections, PLINQ (Parallel LINQ) can be a more straightforward alternative, allowing LINQ queries to run in parallel.
- **Asynchronous Programming:** For I/O-bound operations, consider using asynchronous programming (async and await) instead of parallel loops.






### `ParallelLoopState`

You will need a `ParallelLoopState` instance to break or stop a parallel loop. In this context, “break” means stopping the execution of any further iterations. The loop will still complete the current iterations that are already in progress, but it won't start new ones after the `Break()` method is called. The `Stop()` method is similar to `Break()` but has a more aggressive behavior. It not only stops the loop but also requests that other iterations, even those in progress, stop as soon as possible.

`BreakLoopState` and `StopLoopState` are both instances of `ParallelLoopState`.

```csharp
static void Main()

{

var BreakSource = Enumerable.Range(0, 1000).ToList();

int BreakData = 0;

Console.WriteLine("Using loopstate Break Method");

Parallel.For(0, BreakSource.Count, (i, BreakLoopState) =>

{

BreakData += i;

if (BreakData > 100)

{

BreakLoopState.Break();

Console.WriteLine("Break called iteration {0}. data = {1} ", i, BreakData);

}

});

Console.WriteLine("Break called data = {0} ", BreakData);

var StopSource = Enumerable.Range(0, 1000).ToList();

int StopData = 0;

Console.WriteLine("Using loopstate Stop Method");

Parallel.For(0, StopSource.Count, (i, StopLoopState) =>

{

StopData += i;

if (StopData > 100)

{

StopLoopState.Stop();

Console.WriteLine("Stop called iteration {0}. data = {1} ", i, StopData);

}

});

Console.WriteLine("Stop called data = {0} ", StopData);

Console.ReadKey();
```








### `Parallel.Invoke()`

The Parallel Invoke method in C# is used to launch multiple tasks/methods that are going to be executed in parallel

It is recommended to do a performance measurement before selecting whether you want to execute methods parallelly or sequentially. That is because parallel code might perform worse than sequential code.

`Parallel.Invoke` doesn't return any result or completion status. It simply runs the provided actions in parallel and then proceeds to the next line of code after all actions have completed.

If you need to perform parallel execution with more fine-grained control over tasks and their results, you may want to use the `Task` class directly or explore other constructs like `Task.WhenAll` for asynchronous tasks.

#### Because of `Parallel.Invoke()` instead of calling these 3 methods one by one regularly, this code runs 3 times faster:

```csharp
public class Program

{

	static void Main()
	
	{
	
		Stopwatch stopWatch = new Stopwatch();
		
		stopWatch.Start();
		
		//Calling Three methods Parallely
		
		Parallel.Invoke(
		
		Method1, Method2, Method3
	
		);
	
		stopWatch.Stop();
		
		Console.WriteLine($"Parallel Execution Took {stopWatch.ElapsedMilliseconds} Milliseconds");
		
		Console.ReadKey();
	
	}
	
	static void Method1()
	
	{
	
		Thread.Sleep(200);
		
		Console.WriteLine($"Method 1 Completed by Thread={Thread.CurrentThread.ManagedThreadId}");
		
	}
	
	static void Method2()
	
	{
		
		Thread.Sleep(200);
		
		Console.WriteLine($"Method 2 Completed by Thread={Thread.CurrentThread.ManagedThreadId}");
		
	}
	
	static void Method3()
	
	{
	
		Thread.Sleep(200);
		
		Console.WriteLine($"Method 3 Completed by Thread={Thread.CurrentThread.ManagedThreadId}");
		
	}

}
```

It goes without saying that you can pass anonymous methods and lambda functions as parameters to this method.

#### You can also pass a `ParallelOptions` class instance to this method as it's first parameter:

```csharp
ParallelOptions parallelOptions = new ParallelOptions

{

	MaxDegreeOfParallelism = 3

};

//parallelOptions.MaxDegreeOfParallelism = System.Environment.ProcessorCount - 1;

//Passing ParallelOptions as the first parameter

Parallel.Invoke(

	parallelOptions,
	
	() => DoSomeTask(1),
	
	() => DoSomeTask(2),
	
	() => DoSomeTask(3),
	
	() => DoSomeTask(4),
	
	() => DoSomeTask(5),
	
	() => DoSomeTask(6),
	
	() => DoSomeTask(7)

);

//... rest of the code
```

Now  at any given point in time, no more than three tasks are running in the above program

#### **Invoking Methods with Input and Return Type using `Parallel.Invoke`:**

```csharp
public class Program

{

	static void Main()

	{
	
		int intResult = 0;
		
		string strResult = string.Empty;
		
		//Calling Three methods Parallely
		
		Parallel.Invoke(
		
		() => intResult = Method1(),
		
		() => strResult = Method2("Pranaya"),
		
		() => Method3(100)
	
		);
	
		Console.WriteLine($"Method1 Result: {intResult}");
		
		Console.WriteLine($"Method2 Result: {strResult}");
		
		Console.WriteLine($"Parallel Execution Completed");
		
		Console.ReadKey();
	
	}
	
	static int Method1()
	
	{
	
		Task.Delay(200);
		
		Console.WriteLine($"Method 1 Completed by Thread={Thread.CurrentThread.ManagedThreadId}");
		
		return 100;
	
	}
	
	static string Method2(string name)
	
	{
	
		Task.Delay(200);
		
		Console.WriteLine($"Method 2 Completed by Thread={Thread.CurrentThread.ManagedThreadId}");
		
		return "Hello:" + name;
	
	}
	
	static void Method3(int number)
	
	{
	
		Task.Delay(200);
		
		Console.WriteLine($"Method 3 Completed by Thread={Thread.CurrentThread.ManagedThreadId}");
		
	}

}
```