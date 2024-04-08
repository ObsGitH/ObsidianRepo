
# Asynchronous Programming Patterns

## Retry Pattern

The retry pattern is a design pattern used in software development to handle transient faults or failures that may occur during the execution of a specific operation. Transient faults are typically temporary and may be caused by factors such as network issues, temporary unavailability of a resource, or a momentary overload on a service. The retry pattern aims to increase the robustness and reliability of a system by automatically retrying an operation that has failed, with the expectation that the issue causing the failure will be resolved over time.



### Key Concepts of the Retry Pattern:

1. **Operation:**
    
    - The operation is the specific action or method that you want to execute. It could be anything from making a network request, calling a service, or accessing a resource.
2. **Transient Fault:**
    
    - A transient fault is a temporary failure that is expected to be resolved after a short period. Examples include network timeouts, temporary service unavailability, or resource contention.
3. **Retry Policy:**
    
    - The retry policy defines the rules for when and how many times the operation should be retried. It specifies the maximum number of retry attempts, the delay between retries, and possibly other conditions under which a retry should occur.

### Why Use the Retry Pattern?

- **Improved Reliability:**
    
    - It helps improve the reliability and resilience of a system by allowing it to recover from transient faults without manual intervention.
- **Reduced Impact of Transient Issues:**
    
    - Instead of immediately failing an operation due to a transient issue, the system can make multiple attempts to complete the operation, reducing the impact of temporary failures.
- **Enhanced User Experience:**
    
    - Users may not even notice transient issues if the system can automatically recover and successfully complete the operation after a few retries.

### How to Implement the Retry Pattern:

Here's a general approach to implementing the retry pattern:

1. **Define Retry Policy:**
    
    - Decide on a retry policy that includes parameters such as the maximum number of retry attempts, the delay between retries, and possibly other conditions that trigger a retry.
2. **Wrap the Operation:**
    
    - Wrap the target operation inside a retry loop. The loop will execute the operation and handle any transient faults by retrying according to the defined policy.
3. **Handle Retries:**
    
    - Implement the retry logic within the loop, checking for transient faults. If a transient fault occurs, wait for the specified delay and retry the operation. Repeat this process until the maximum number of retries is reached or the operation succeeds.
4. **Exponential Backoff:**
    
    - Consider using exponential backoff for the delay between retries. This means that the delay increases exponentially with each retry, providing a balance between allowing time for the transient fault to resolve and avoiding overloading the system with immediate retries.

### Example Implementation in C#:

Here's a simple example of implementing the retry pattern in C#:

```csharp
public class RetryPolicy 
{     
	public static async Task<T> ExecuteWithRetry<T>(Func<Task<T>> operation, int maxRetries = 3, TimeSpan delayBetweenRetries = default)     
	{         
		int retries = 0;          
		while (true)         
		{             
			try             
			{                 
				return await operation();             
			}             
			catch (TransientException ex) // Define your own transient exception type             
			{                 
				retries++;                  
				if (retries > maxRetries)                 
				{                     
					// Maximum number of retries reached, propagate the exception
					throw;                 
				}                  
				if (delayBetweenRetries != TimeSpan.Zero)                 
				{                     
					// Introduce delay between retries                     
					await Task.Delay(delayBetweenRetries);                 
				}             
			}         
		}     
	} 
}  
public class TransientException : Exception { public TransientException(string message) : base(message) { } }
```


Usage example:

```csharp
try 
{     
	await RetryPolicy.ExecuteWithRetry(         async () =>         {
	    // Your operation that may encounter transient faults             
	await SomeService.CallAsyncMethod();}, maxRetries: 3, delayBetweenRetries: TimeSpan.FromSeconds(1)); 
	}
	catch (TransientException ex) {     
		// Handle or log the exception after retries are exhausted
		Console.WriteLine($"Operation failed after retries: {ex.Message}"); 
		}
```

In this example, the `ExecuteWithRetry` method wraps the target asynchronous operation. If a `TransientException` occurs (you can replace this with your own transient exception type), the method will retry the operation up to the specified maximum number of retries, with a delay between retries. The operation is retried until it succeeds or the maximum number of retries is reached. Adjust the parameters according to the specific requirements of your application.

### **Adapt Transient Fault Handling:**

Ensure that your asynchronous operation properly throws and catches `TaskCanceledException` or other specific exceptions indicating transient faults.

For example, if you are using an external library that throws a `HttpRequestException` for transient network errors, you may need to catch that specific exception type in your retry logic.

`public static async Task<string> GetDataWithRetryAsync() {     return await ExecuteWithRetryAsync(         async () =>         {             // Your asynchronous operation that may encounter transient faults             return await ExternalLibrary.GetDataAsync();         },         maxRetries: 3,         delayBetweenRetries: TimeSpan.FromSeconds(1)     ); }`

### **Exponential Backoff with Async:**

If desired, you can incorporate exponential backoff in the delay between retries, similar to the synchronous example.

```csharp
public static async Task<T> ExecuteWithExponentialBackoffAsync<T>(     Func<Task<T>> operation,     int maxRetries = 3,     TimeSpan initialDelay = default) 
{     
	int retries = 0;      
	while (true)     
	{         
		try         
		{             
			return await operation();         
		}         
		catch (TransientException ex) // Define your own transient exception type         
		{             
			retries++;              
			if (retries > maxRetries)             
			{                 
				// Maximum number of retries reached, propagate the exception                 
				throw;             
			}             
		    TimeSpan delay = CalculateExponentialBackoff(initialDelay, retries);             
			if (delay != TimeSpan.Zero)             
			{                 
				// Introduce delay between retries                 
				await Task.Delay(delay);             
			}         
		}     
	} 
}  

private static TimeSpan CalculateExponentialBackoff(TimeSpan initialDelay, int retries) 
{     
	// Calculate exponential backoff using a formula (e.g., doubling the delay each time)     
	return initialDelay == TimeSpan.Zero ? TimeSpan.Zero : TimeSpan.FromMilliseconds(Math.Pow(2, retries - 1) * initialDelay.TotalMilliseconds); 
}
```


### **Handle Cancellation:**

When using `Task.Delay` for retry delays, be aware that the delay can be canceled if the operation is canceled. Ensure that your asynchronous operations support cancellation and that the retry logic respects cancellation tokens.


```csharp
public static async Task<T> ExecuteWithRetryAsync<T>(     Func<CancellationToken, Task<T>> operation,     int maxRetries = 3,     TimeSpan delayBetweenRetries = default,     CancellationToken cancellationToken = default) 
{     
	int retries = 0;      
	while (true)     
	{         
		try         
		{             
			return await operation(cancellationToken);         
		}         
		catch (TransientException ex) // Define your own transient exception type         
		{             
			retries++;              
			if (retries > maxRetries)             
			{                 
				// Maximum number of retries reached, propagate the exception                 
				throw;             
			}              
			if (delayBetweenRetries != TimeSpan.Zero)             
			{                 
				// Introduce delay between retries                 
				await Task.Delay(delayBetweenRetries, cancellationToken);             
			}         
		}     
	} 
}
```


By adapting the retry pattern for asynchronous programming, you can make your asynchronous operations more resilient to transient faults and improve the overall reliability of your system. Adjust the parameters, exception handling, and backoff strategies based on the specific needs of your application and the characteristics of the transient faults you expect to encounter.


## When Should I Use The Retry Pattern

The retry pattern is a useful tool in scenarios where you anticipate transient faults or temporary failures that may occur during the execution of an operation. Here are some situations where using the retry pattern is appropriate:

1. **Network Operations:**
    
    - When making network requests, transient faults such as temporary connectivity issues, timeouts, or intermittent service unavailability may occur. Retrying network operations can help improve the resilience of your application.
2. **Database Operations:**
    
    - Database connections or queries may encounter transient issues, such as network glitches or temporary unavailability of the database server. Retrying database operations can help handle these transient faults.
3. **External Service Integration:**
    
    - When integrating with external services or APIs, transient issues on the service side (e.g., rate limiting, temporary unavailability) may occur. Retrying the integration operation can provide a better chance of success.
4. **File I/O and Disk Operations:**
    
    - Reading or writing files and performing disk-related operations may encounter transient issues, such as temporary unavailability of storage or intermittent file system errors. Retrying these operations can improve reliability.
5. **Concurrency and Resource Contention:**
    
    - In scenarios with resource contention or synchronization issues, introducing retries may help address transient conflicts and contention, especially in distributed systems.
6. **Cloud Services:**
    
    - When working with cloud services, transient faults like temporary server overloads, brief service outages, or throttling might occur. Retrying operations can help your application adapt to dynamic cloud environments.
7. **Asynchronous Processing:**
    
    - In scenarios where you have asynchronous processing or background tasks, transient faults can still occur. Retrying asynchronous operations can help improve the robustness of background processing.
8. **Deterministic Operations:**
    
    - The retry pattern is most effective when applied to idempotent operations—operations that produce the same result regardless of how many times they are executed. Ensure that the retried operation does not have unintended side effects when repeated.
9. **Fault Tolerance Requirements:**
    
    - In systems where fault tolerance is a critical requirement, the retry pattern can be part of an overall strategy to handle and recover from transient faults without user intervention.
10. **Improved User Experience:**
    
    - Retrying operations transparently in the background can help provide a better user experience by minimizing the impact of transient issues and avoiding immediate failures.

## Possible Problems With The Retry Pattern

1. **Deadlocks:**
    
    - In scenarios where resources are contended, introducing retries may increase the risk of deadlocks. Carefully review and minimize the use of synchronous locks within asynchronous retry blocks to avoid deadlocks.
2. **Retrying Non-Transient Faults:**
    
    - If the retry logic is not correctly designed to identify and handle only transient faults, it may attempt to retry non-transient faults indefinitely, leading to unnecessary load on the system and potential performance degradation.
3. **Increased Resource Utilization:**
    
    - Introducing retries may lead to increased resource utilization, especially if the retries are immediate and frequent. This can exacerbate the issues causing transient faults and potentially overload the system.
4. **Exponential Backoff Issues:**
    
    - While exponential backoff can be effective in avoiding immediate retries, it may also lead to longer overall execution times. Developers need to strike a balance between avoiding immediate retries and not waiting excessively long, especially when dealing with time-sensitive operations.
5. **Unintended Delay:**
    
    - The use of asynchronous operations and retries may introduce unintended delays, affecting the overall responsiveness of the application. Carefully choose retry strategies based on the nature of the operations and user expectations.
6. **Infinite Retry Loops:**
    
    - Incorrectly implemented retry logic may result in infinite retry loops, especially if the conditions for stopping the retries are not well-defined. This can lead to high resource consumption and a degradation of system performance.
7. **Cancellation Handling:**
    
    - Ensure proper handling of cancellation tokens. If an operation is canceled, the associated retries should also be canceled. Incorrectly handling cancellation may result in retries continuing even after the main operation is canceled.
8. **Logging and Monitoring:**
    
    - Logging and monitoring are crucial for identifying issues related to retries. Without proper logging, it may be challenging to diagnose the cause of failures and assess the effectiveness of the retry strategy.
9. **Maintainability:**
    
    - Complex retry logic may be harder to maintain and understand over time. Clear documentation and code comments can help, but developers should be cautious about introducing unnecessary complexity.
10. **Duplicated Retries:**
    
    - In scenarios where multiple components or layers of the application implement retry logic independently, there is a risk of duplicated retries. Consider centralizing retry logic to avoid redundant and potentially conflicting retry attempts.
11. **Non-Idempotent Operations:**
    
    - If the retried operation is not idempotent (i.e., repeating the operation has side effects), retries may lead to unintended consequences or data corruption. Ensure that retried operations can safely be repeated without causing issues.
12. **Impact on Resource Providers:**
    
    - When dealing with external services or resource providers, consider the impact of retries on those services. Excessive retries can put additional load on external systems, affecting their performance and potentially violating usage policies.
- 

## Only One Pattern in Asynchronous Programming

##### **Only One Pattern in C# Asynchronous Programming:**

Sometimes we will have multiple tasks, and all the tasks give us the same information, and we only want to use the first one to finish and cancel the rest. For that, we can use a pattern (Only One Pattern) that uses the cancellation token.
![[Only1Pattern.webp]]
![[Only1Pattern2.webp]]

##### **Generic Only One Pattern in C# Asynchronous Programming:**

For a better understanding, please have a look at the following image.
![[GenericOnly1Pattern.webp]]

##### **OnlyOne Pattern with Different Methods in C#:**

As of now, we are using our Only One Pattern to make the same operation over a collection. But We may not want that always. Maybe we have two different methods that we want to run at the same time, but we want to cancel one method after the other method finishes. This is also possible using Only One Pattern in C#.


```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using System.Linq;

`namespace AsynchronousProgramming
`{
    `class Program
    `{
        `static void Main(string[] args)
        {
		    `SomeMethod();
            `Console.ReadKey();
	     }

        public static async void SomeMethod()
        {
            //Calling two Different Method using Generic Only One Pattern

            var content = await GenericOnlyOnePattern(
                  //Calling the HelloMethod
                  (ct) => HelloMethod("Pranaya", ct),
                  //Calling the GoodbyeMethod
                  (ct) => GoodbyeMethod("Anurag", ct)
                  );

            //Printing the result on the Console
            Console.WriteLine($"{content}");
        }

        public static async Task<T> GenericOnlyOnePattern<T>(params Func<CancellationToken, Task<T>>[] functions)
        {
            var cancellationTokenSource = new CancellationTokenSource();
            var tasks = functions.Select(function => function(cancellationTokenSource.Token));
            var task = await Task.WhenAny(tasks);
            cancellationTokenSource.Cancel();
            return await task;
        }
        
        public static async Task<string> HelloMethod(string name, CancellationToken token)
        {
            var WaitingTime = new Random().NextDouble() * 10 + 1;
            await Task.Delay(TimeSpan.FromSeconds(WaitingTime));

            string message = $"Hello {name}";
            return message;
        }

        public static async Task<string> GoodbyeMethod(string name, CancellationToken token)
        {
            var WaitingTime = new Random().NextDouble() * 10 + 1;
            await Task.Delay(TimeSpan.FromSeconds(WaitingTime));

            string message = $"Goodbye {name}";
            return message;
        }
    }
}
```

## Anti-Patterns

Anti-patterns in asynchronous programming refer to common practices or coding patterns that may seem acceptable at first glance but can lead to issues such as decreased performance, code complexity, and maintenance challenges. Here are some anti-patterns in asynchronous programming:

1. **Async Void Methods:**
    
    - Defining asynchronous methods with a return type of `void` (e.g., `async void SomeMethod()`) is an anti-pattern, unless they are being used in event handlers. Asynchronous void methods cannot be awaited, and any exceptions thrown inside them cannot be caught using standard exception handling mechanisms.
    
```csharp
// Anti-pattern
async void SomeMethod()
{
    // ...
}

```
    
    Instead, prefer using `async Task` or `async Task<T>` for asynchronous methods that can be awaited.
    
2. **Fire-and-Forget Tasks:**
    
    - Ignoring the task returned by an asynchronous method is known as a "fire-and-forget" pattern. While this can be suitable for certain scenarios, it can lead to unhandled exceptions and make it challenging to track the completion or failure of the asynchronous operation.
    
 ```csharp
// Anti-pattern
private void FireAndForget()
{
    _ = SomeAsyncMethod(); // Ignoring the returned task
}

```
    
    It's often better to at least log or handle exceptions appropriately when using fire-and-forget patterns.
    
3. **Blocking on Async Code:**
    
    - Using synchronous blocking calls like `Task.Wait` or `Task.Result` on asynchronous code can lead to deadlocks, reduced performance, and defeat the purpose of asynchronous programming.
    
```csharp
// Anti-pattern
public void BlockingCall()
{
    Task<int> result = SomeAsyncMethod();
    int value = result.Result; // Blocking call
}

```
    
    Instead, use `await` to asynchronously wait for the completion of the task.
    
4. **Excessive Parallelism:**
    
    - Creating too many concurrent tasks or parallelizing operations without considering resource limitations can lead to thread pool exhaustion and decreased performance.
    
```csharp
// Anti-pattern
private async Task ProcessItemsAsync(List<Item> items)
{
    // Creating a new task for each item
    var tasks = items.Select(item => ProcessItemAsync(item));
    await Task.WhenAll(tasks);
}

```
    
    It's important to carefully manage parallelism and consider using tools like `SemaphoreSlim` to limit the number of concurrent tasks.
    
5. **Using Task.Result in Synchronous Code:**
    
    - In synchronous code, blocking on an asynchronous method using `Task.Result` or `Task.GetAwaiter().GetResult()` can lead to deadlocks or thread pool issues.
    
```csharp
// Anti-pattern
public void BlockingCallInSyncCode()
{
    int result = SomeAsyncMethod().Result; // Blocking call in synchronous code
}

```
    In synchronous code, consider making the calling method asynchronous as well or using `async` and `await` appropriately.
    
6. **Nested Async Void Methods:**
    
    - Avoid nesting asynchronous `async void` methods, as it can make exception handling and debugging challenging.
```csharp
// Anti-pattern
async void OuterMethod()
{
    await InnerMethod(); // Nested async void
}

```
    
    Instead, prefer using `async Task` for methods that are meant to be awaited.
    
7. **Ignoring Task.ReturnUncompletedTask:**
    
    - Ignoring the return value of `Task.Run` or similar methods can lead to unexpected behavior, especially if the task is long-running or represents a critical operation.
    
```csharp
// Anti-pattern
public void IgnoringTaskRun()
{
    Task.Run(() => SomeLongRunningOperation()); // Ignoring the returned task
}

```
    
    Always consider capturing and awaiting the returned task when using `Task.Run` or similar methods.
    
8. **Incomplete Exception Handling:**
    
    - Failing to properly handle exceptions in asynchronous code can result in unobserved exceptions. Use `try`/`catch` blocks or the `Task.Exception` property to handle exceptions appropriately.
    
```csharp
// Anti-pattern
private async Task HandleExceptionAsync()
{
    try
    {
        await SomeAsyncMethod();
    }
    // Missing catch block
}

```
    
    Always include comprehensive exception handling in your asynchronous code.
# Task Based Programming

## Task Class

### Overview
  
In C#, a `Task` is a fundamental component of the Task Parallel Library (TPL) and is part of the asynchronous programming model introduced in .NET. A `Task` represents an asynchronous operation that may or may not return a result. It is used to perform work on a separate thread, allowing the main (calling) thread to continue with other tasks concurrently. You can pass an action delegate to the `Task` class constructor to create a task that did not start, and a `Func` delegate for `Task<T>`.

Here are key characteristics and features of the `Task` class:

1. **Asynchronous Execution:**
    
    - A `Task` represents an operation that can be performed asynchronously. It allows you to execute code concurrently without blocking the calling thread.
2. **Result or Exception:**
    
    - A `Task` can return a result of type `Task<TResult>` or represent an operation that doesn't return a result with `Task`. The result can be accessed using the `Result` property or awaited in an asynchronous method.
3. **Task Status:**
    
    - A `Task` can be in various states such as `WaitingToRun`, `Running`, `WaitingForActivation`, `Canceled`, `Faulted`, or `RanToCompletion`. The `Status` property provides information about the current state.
4. **Cancellation:**
    
    - Tasks can be associated with a `CancellationToken` to support cancellation. If the cancellation token is canceled, the associated task may be canceled as well.
5. **Continuations:**
    
    - Tasks can have continuations, which are tasks that run upon the completion of the original task. This enables chaining of asynchronous operations.
6. **Error Handling:**
    
    - Exceptions thrown during the execution of a task are stored, and they can be observed or handled using the `Exception` property or the `await` keyword.
7. **TaskCompletionSource:**
    
    - The `TaskCompletionSource<T>` class can be used to create and control the completion of a `Task` manually.
8. **Task Schedulers:**
    
    - The `Task` class uses task schedulers to determine where and how a task runs. The default scheduler uses the ThreadPool, but custom schedulers can be used for specific scenarios.


### Methods

#### 1. **`Task.Run(Action action)`**

- **Description:**
    
    - Creates and starts a task for the specified action delegate.
- **Example:**
    
    `Task.Run(() => Console.WriteLine("Task is running."));`
    

#### 2. **`Task.Factory.StartNew(Action action)`**

- **Description:**
    
    - Creates and starts a task using the `Task.Factory.StartNew` method.
- **Example:**
    
    `Task.Factory.StartNew(() => Console.WriteLine("Task is running."));`
    

#### 3. **`Task.Factory.StartNew(Action action, CancellationToken cancellationToken)`**

- **Description:**
    
    - Creates and starts a task with a cancellation token using the `Task.Factory.StartNew` method.
- **Example:**
    
    `CancellationTokenSource cts = new CancellationTokenSource(); Task.Factory.StartNew(() => Console.WriteLine("Task is running."), cts.Token);`
    

#### 4. **`Task.FromResult<TResult>(TResult result)`**

- **Description:**
    
    - Creates a completed task with a specified result.
- **Example:**
    
    `Task<int> completedTask = Task.FromResult(42);`
    

#### 5. **`Task.Wait()`**

- **Description:**
    
    - Blocks the calling thread (the thread that calls the method) until the task completes execution.
- **Example:**
    
    `Task myTask = Task.Run(() => Console.WriteLine("Task is running.")); myTask.Wait();`
    

#### 6. **`Task.Wait(int millisecondsTimeout)`**

- **Description:**
    
    - Blocks the calling thread until the task completes execution or the specified timeout elapses.
- **Example:**
    
    `Task myTask = Task.Run(() => Console.WriteLine("Task is running.")); bool completed = myTask.Wait(1000); // Wait for 1 second`
    

#### 7. **`Task.Wait(TimeSpan timeout)`**

- **Description:**
    
    - Blocks the calling thread until the task completes execution or the specified timeout elapses.
- **Example:**
    
    `Task myTask = Task.Run(() => Console.WriteLine("Task is running.")); bool completed = myTask.Wait(TimeSpan.FromSeconds(2)); // Wait for 2 seconds`
    

#### 8. **`Task.WaitAll(params Task[] tasks)`**

- **Description:**
    
    - Blocks the calling thread until all the provided tasks complete execution.
- **Example:**
    
    `Task task1 = Task.Run(() => Console.WriteLine("Task 1 is running.")); Task task2 = Task.Run(() => Console.WriteLine("Task 2 is running.")); Task.WaitAll(task1, task2);`
    

#### 9. **`Task.WaitAny(params Task[] tasks)`**

- **Description:**
    
    - Blocks the calling thread until any of the provided tasks completes execution.
- **Example:**
    
    csharpCopy code
    
    `Task task1 = Task.Run(() => Console.WriteLine("Task 1 is running.")); Task task2 = Task.Run(() => Console.WriteLine("Task 2 is running.")); int completedTaskIndex = Task.WaitAny(task1, task2);`
    

#### 10. **`Task.ContinueWith(Action<Task> continuationAction)`**

- **Description:**
    
    - Creates a continuation task that executes when the antecedent task completes.
- **Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"));`
    

#### 11. **`Task.ContinueWith(Action<Task> continuationAction, CancellationToken cancellationToken)`**

- **Description:**
    
    - Creates a continuation task with a cancellation token that executes when the antecedent task completes.
- **Example:**
    
    `CancellationTokenSource cts = new CancellationTokenSource(); Task<int> antecedent = Task.Run(() => 42); Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"), cts.Token);`
    

#### 12. **`Task.Delay(int millisecondsDelay)`**

- **Description:**
    
    - Creates a task that completes after a specified time delay.
- **Example:**
    
    csharpCopy code
    
    `await Task.Delay(1000); // Delay for 1 second`
    

#### 13. **`Task.Delay(TimeSpan delay)`**

- **Description:**
    
    - Creates a task that completes after a specified time delay.
- **Example:**
    
    
    `await Task.Delay(TimeSpan.FromSeconds(2)); // Delay for 2 seconds`

### Thread Pool

**Thread Pool in C#** is a collection of **threads** that can perform several tasks in the background. Once a thread completes its task, it is returned back to the thread pool. This reusability of threads prevents an application from creating many threads, ultimately using less memory consumption.








## Examples

#### Basic Use of Task Class
```csharp
`using System;
`using System.Threading;
`using System.Threading.Tasks;

`namespace TaskBasedAsynchronousProgramming
`{
    `class Program
    `{
        `static void Main(string[] args)
        `{
            `Console.WriteLine($"Main Thread : {Thread.CurrentThread.ManagedThreadId} Statred");
            `Action actionDelegate = new Action(PrintCounter);
            `Task task1 = new Task(actionDelegate);
            `//You can directly pass the PrintCounter method as its signature is same as Action delegate
            `//Task task1 = new Task(PrintCounter);
            `task1.Start();
            `Console.WriteLine($"Main Thread : {Thread.CurrentThread.ManagedThreadId} Completed");
            `Console.ReadKey();
        `}

        static void PrintCounter()
        {
            Console.WriteLine($"Child Thread : {Thread.CurrentThread.ManagedThreadId} Started");
            for (int count = 1; count <= 5; count++)
            {
                Console.WriteLine($"count value: {count}");
            }
            Console.WriteLine($"Child Thread : {Thread.CurrentThread.ManagedThreadId} Completed");
        }
    }
`}
```

#### `Task.Factory` Property Usage
The Factory property of the Task class will return an instance of the `TaskFactory` object. The `TaskFactory` class has one method called `StartNew`, which will require an Action delegate as a parameter. So, we can create an instance of Action delegate and pass that instance as a parameter to this `StartNew` method.

Creating the task object using the Factory property, which will start automatically, which means it will start executing the method immediately. Here, we don’t need to call the Start method.

```csharp
`using System;
`using System.Threading;
`using System.Threading.Tasks;

`namespace TaskBasedAsynchronousProgramming
`{
    `class Program
    `{
        `static void Main(string[] args)
        `{
            `Console.WriteLine($"Main Thread : {Thread.CurrentThread.ManagedThreadId} Statred");
            `Task task1 =  Task.Factory.StartNew(PrintCounter); 
            `Console.WriteLine($"Main Thread : {Thread.CurrentThread.ManagedThreadId} Completed");
            `Console.ReadKey();
        `}

        static void PrintCounter()
        {
            Console.WriteLine($"Child Thread : {Thread.CurrentThread.ManagedThreadId} Started");
            for (int count = 1; count <= 5; count++)
            {
                Console.WriteLine($"count value: {count}");
            }
            Console.WriteLine($"Child Thread : {Thread.CurrentThread.ManagedThreadId} Completed");
        }
    }
}
```

#### Creating Task Object with `Task.Run()`

```csharp
`using System;
`using System.Threading;
`using System.Threading.Tasks;

`namespace TaskBasedAsynchronousProgramming
`{
    `class Program
    `{
        `static void Main(string[] args)
        `{
            `Console.WriteLine($"Main Thread : {Thread.CurrentThread.ManagedThreadId} Statred");
            `Task task1 = Task.Run(() => { PrintCounter(); });
            `Console.WriteLine($"Main Thread : {Thread.CurrentThread.ManagedThreadId} Completed");
            `Console.ReadKey();
        `}

        static void PrintCounter()
        {
            Console.WriteLine($"Child Thread : {Thread.CurrentThread.ManagedThreadId} Started");
            for (int count = 1; count <= 5; count++)
            {
                Console.WriteLine($"count value: {count}");
            }
            Console.WriteLine($"Child Thread : {Thread.CurrentThread.ManagedThreadId} Completed");
        }
    }
}
```

From a performance point of view, **Task.Run or Task.Factory.StartNew** methods are preferable to create and start executing the tasks asynchronously. But, if you want the task creation and execution separately, you need to create the task separately by using the Task class and then call the Start method to start the task execution when required.




## Task Chaining

### Overview

#### 1. **Continuation Tasks:**

- **Definition:**
    
    - A continuation task is a task that runs after the completion of another task (antecedent task). It represents a logical follow-up operation.
- **Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"));`
    

#### 2. **`ContinueWith` Method:**

- **Usage:**
    
    - The `ContinueWith` method is used to create a continuation task. It takes an `Action<Task>` parameter, representing the action to be executed when the antecedent task completes.
- **Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"));`
    

#### 3. **Task Result Propagation:**

- **Behavior:**
    
    - The result of the antecedent task is automatically propagated to the continuation task. If the antecedent task has a result of type `TResult`, the continuation task receives that result.
- **Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task<string> continuation = antecedent.ContinueWith(task => $"Result: {task.Result}");`
    

#### 4. **Multiple Continuations:**

- **Behavior:**
    
    - You can create multiple continuations for a single antecedent task. Each continuation represents a separate operation to be performed when the antecedent task completes.
- **Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task continuation1 = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}")); Task continuation2 = antecedent.ContinueWith(task => Console.WriteLine($"Squared Result: {task.Result * task.Result}"));`
    

#### 5. **Chaining with `ContinueWith`:**

- **Chaining Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task<string> continuation = antecedent.ContinueWith(task => $"Result: {task.Result}")
    

#### 6. **Task Scheduler:**

- **Default Scheduler:**
    
    - By default, continuation tasks run on the ThreadPool. You can specify a different task scheduler when creating the continuation task using the `TaskContinuationOptions` enumeration.
- **Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"),                                            TaskContinuationOptions.OnlyOnRanToCompletion | TaskContinuationOptions.LongRunning);`
    

#### 7. **Error Handling:**

- **Handling Exceptions:**
    
    - Continuation tasks allow you to handle exceptions from the antecedent task. Exceptions can be observed using the `Exception` property of the continuation task.
- **Example:**
    
    `Task<int> antecedent = Task.Run(() => { throw new Exception("An error occurred."); }); Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"),                                            TaskContinuationOptions.OnlyOnRanToCompletion); Task errorHandling = continuation.ContinueWith(task => Console.WriteLine($"Error: {task.Exception.InnerException}"),                                                TaskContinuationOptions.OnlyOnFaulted);`
    

#### 8. `**TaskCompletionSource` for Manual Chaining:**

- **Manual Chaining:**
    
    - In some cases, you may use `TaskCompletionSource<T>` to manually create and complete tasks in a chained fashion.
- **Example:**
    
    `TaskCompletionSource<int> tcs = new TaskCompletionSource<int>();  Task<int> antecedent = tcs.Task;  Task<string> continuation = antecedent.ContinueWith(task => $"Result: {task.Result}");  // Manually complete the antecedent task tcs.SetResult(42);`
    

#### 9. **`async` and `await` in Chaining:**

- **`async` Continuations:**
    
    - When using `async` and `await` in continuation tasks, the asynchronous workflow continues seamlessly, providing a more natural way to express asynchronous operations.
- **Example:**
    
    `async Task<int> PerformAsyncOperation() {     // Async operation     await Task.Delay(1000);     return 42; }  
    `Task<int> antecedent = PerformAsyncOperation();  
    `Task<string> continuation = antecedent.ContinueWith(async task => {     int result = await AnotherAsyncOperation(task.Result);     return $"Result: {result}"; });`
    

#### 10. **Cancellation in Chaining:**

- **Cancellation Example:**
    `CancellationTokenSource cts = new CancellationTokenSource();

	`Task<int> antecedent = Task.Run(() => 42, cts.Token);

	`Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"),
    `TaskContinuationOptions.OnlyOnRanToCompletion, CancellationToken.None,TaskScheduler.Default);

	`Task cancellationHandling = continuation.ContinueWith(task => Console.WriteLine("Task was canceled."),TaskContinuationOptions.OnlyOnCanceled,CancellationToken.None,TaskScheduler.Default);




Task chaining is a powerful technique in C# asynchronous programming, enabling the creation of complex workflows and expressing the logic of sequential asynchronous operations more clearly. Understanding how to use continuation tasks effectively is crucial for developing robust and maintainable asynchronous code.



### Continuation Tasks

While working with asynchronous programming, it is very common to invoke one asynchronous operation from another asynchronous operation passing the data once it completes its execution. This is called continuations and in the traditional approach, this has been done by using the callback method which is a little difficult to understand.

But with the introduction of Task Parallel Library (TPL), the same functionality can be achieved very easily by using continuation tasks. In simple words, we can define a continuation task as an asynchronous task that is going to be invoked by another task (i.e. known as the antecedent).

#### **Creating a continuation for a single antecedent**

`using System;
`using System.Threading.Tasks;

`namespace TaskBasedAsynchronousProgramming
`{
    `class Program
    `{
        `static void Main(string[] args)
        `{
            `Task<string> task1 = Task.Run(() =>
            `{
                `return 12;
            `}).ContinueWith((antecedent) =>
            `{
                `return $"The Square of {antecedent.Result} is: {antecedent.Result * antecedent.Result}";
            `});
            `Console.WriteLine(task1.Result);
            
            Console.ReadKey();
        }
    }
`}

#### **Scheduling Different Continuation Tasks**

`using System;
`using System.Threading.Tasks;

`namespace TaskBasedAsynchronousProgramming
`{
    `class Program
    `{
        `static void Main(string[] args)
        `{
            `Task<int> task = Task.Run(() =>
            `{
                return 10;
            `});

            task.ContinueWith((i) =>
            {
                Console.WriteLine("TasK Canceled");
            }, TaskContinuationOptions.OnlyOnCanceled);

            task.ContinueWith((i) =>
            {
                Console.WriteLine("Task Faulted");
            }, TaskContinuationOptions.OnlyOnFaulted);


            var completedTask = task.ContinueWith((i) =>
            {
                Console.WriteLine("Task Completed");
            }, TaskContinuationOptions.OnlyOnRanToCompletion);

            completedTask.Wait();

            Console.ReadKey();
        }
    }
`}



#### 1. **`Task.ContinueWith` Method:**

- **Syntax:**
    
    `Task ContinueWith(Action<Task> continuationAction);`
    
- **Description:**
    
    - The `ContinueWith` method creates a continuation task that executes the specified action when the antecedent task completes.
- **Example:**

    `Task<int> antecedent = Task.Run(() => 42); Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"));`


#### 2. **Chaining Continuations:**

- **Chaining Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task<string> continuation = antecedent.ContinueWith(task => $"Result: {task.Result}").ContinueWith(previousTask => Console.WriteLine(previousTask.Result));`
    
- **Explanation:**
    
    - Multiple `ContinueWith` calls can be chained to create a sequence of operations. Each continuation is executed in order after the antecedent task completes.

#### 3. **Accessing Result of Antecedent Task:**

- **Result Access Example:**
    
    `Task<int> antecedent = Task.Run(() => 42); Task<string> continuation = antecedent.ContinueWith(task => $"Result: {task.Result}");`
    
- **Explanation:**
    
    - The result of the antecedent task can be accessed within the continuation task using the `Result` property of the `Task` class.

#### 4. **Exception Handling:**

- **Handling Exceptions:**
    
    `Task<int> antecedent = Task.Run(() => { throw new Exception("An error occurred."); });  Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"),                                            TaskContinuationOptions.OnlyOnRanToCompletion);  Task errorHandling = continuation.ContinueWith(task => Console.WriteLine($"Error: {task.Exception.InnerException}"),                                                TaskContinuationOptions.OnlyOnFaulted);`
    
- **Explanation:**
    
    - Exception handling in continuation tasks allows you to specify actions to be taken when the antecedent task completes with an exception.

#### 5. **Task Scheduler and Options:**

- **Task Scheduler Example:**
    
    `Task<int> antecedent = Task.Run(() => 42);  Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"),                                            TaskContinuationOptions.OnlyOnRanToCompletion | TaskContinuationOptions.LongRunning);`
    
- **Explanation:**
    
    - You can specify task scheduler and options using the `TaskContinuationOptions` enumeration when creating continuation tasks.

#### 6. **`async` and `await` in Continuations:**

- **Example with `async` Continuations:**
    
    `async Task<int> PerformAsyncOperation() {     await Task.Delay(1000);     return 42; }  Task<int> antecedent = PerformAsyncOperation();  Task<string> continuation = antecedent.ContinueWith(async task => {     int result = await AnotherAsyncOperation(task.Result);     return $"Result: {result}"; });`
    
- **Explanation:**
    
    - You can use `async` and `await` in continuation tasks, providing a more natural way to express asynchronous operations.

#### 7. **Cancellation in Continuations:**

- **Cancellation Example:**
    
    `CancellationTokenSource cts = new CancellationTokenSource();  Task<int> antecedent = Task.Run(() => 42, cts.Token);  Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"),                                            TaskContinuationOptions.OnlyOnRanToCompletion,                                            CancellationToken.None,                                            TaskScheduler.Default);  Task cancellationHandling = continuation.ContinueWith(task => Console.WriteLine("Task was canceled."),                                                      TaskContinuationOptions.OnlyOnCanceled,                                                      CancellationToken.None,                                                      TaskScheduler.Default);`
    
- **Explanation:**
    
    - Cancellation tokens can be associated with continuation tasks to handle scenarios where the antecedent task is canceled.

#### 8. `**TaskCreationOptions`:**

- **`TaskCreationOptions` Example:**
    
    `Task<int> antecedent = Task.Run(() => 42, TaskCreationOptions.LongRunning);  Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"),                                            TaskScheduler.Default);`
    
- **Explanation:**
    
    - The `TaskCreationOptions` enumeration can be used to specify options for creating the continuation task.

#### 9. **`TaskContinuationOptions Enum`:**

- **Example with `TaskContinuationOptions`:**
    
    `Task<int> antecedent = Task.Run(() => 42);  Task continuation = antecedent.ContinueWith(task => Console.WriteLine($"Result: {task.Result}"),                                            TaskContinuationOptions.OnlyOnRanToCompletion |                                            TaskContinuationOptions.ExecuteSynchronously);`
    
- **Explanation:**
    
    - `TaskContinuationOptions` can be used to control the conditions under which the continuation task runs.


## Attaching Child Task To Parent Task

#### **What are Detached Child Tasks in C#?**

A detached child task is a task that executes independently of its parent. If any exception is thrown by the child task, then that is not going to be handled by the Parent task. And finally, the Parent Task status does not depend on the Child Task status.

In most of the scenarios, it is recommended to use detached child tasks, because their relationships with other tasks or parent tasks are less complex. This is the reason why tasks created inside the parent tasks are detached by default, and if you want, you need to explicitly specify `**TaskCreationOptions.AttachedToParent`** option to create an attached child task.

`using System;
`using System.Threading;
`using System.Threading.Tasks;
`public class Program
`{
    `public static void Main()
    `{
        `Console.WriteLine("Main Method Started");

        //Creating the Parent Task
        var parentTask = Task.Factory.StartNew(() => {
            Console.WriteLine("Outer Task Started");

            //Creating the Child Task
            var childTask = Task.Factory.StartNew(() => {
                Console.WriteLine("Child Task Started.");
                Thread.Sleep(5000);
                Console.WriteLine("Child Task Completed");
            });

            Console.WriteLine("Outer Task Completed");
        });

        //Waiting for the Parent Task to completed. Not the Child Task
        parentTask.Wait();
        Console.WriteLine("Main Method Completed");
        Console.ReadKey();
    }
`}

The main task is not waiting for the child task to complete its execution.

#### How to Make Parent Task wait for Detached Child Task

If the child task is represented by `**Task<TResult>**` object rather than a Task object, then you can ensure that the parent task will wait for the child task to complete its execution by accessing the `**Task<TResult>.Result**` property of the child task even though it is a detached child task. The reason for this is the Result property will block the parent task until the child task completes its execution.

A task may create any number of detached  child tasks.

If a detached child task throws an exception, then that exception is not going to be propagated to the Parent Task. (You will not be able to catch that exception with await keyword or try catch blocks on the parent task, only in the child task itself)

`using System;
`using System.Threading;
`using System.Threading.Tasks;
`public class Program
`{
    `public static void Main()
    `{
        `Console.WriteLine("Main Method Started");

        //Creating the Parent Task
        var parentTask = Task<string>.Factory.StartNew(() => {
            Console.WriteLine("Outer Task Started");

            //Creating the Child Task
            var childTask = Task<string>.Factory.StartNew(() => {
                Console.WriteLine("Child Task Started");
                Thread.Sleep(5000);
                Console.WriteLine("Child Task Completed");
                return "Task Completed";
            });
            
            // Parent Task will wait for detached Child Task to complete its execution
            return childTask.Result;
        });

        //Here, parentTask.Result will block the Main thread, till the Parent task complete its execution
        Console.WriteLine($"Parent Task Returned: {parentTask.Result}");
        Console.WriteLine("Main Method Completed");
        Console.ReadKey();
    }
`}


#### What is a Attached Child Task in C#?

An attached child task is a nested task that is created with **`TaskCreationOptions.AttachedToParent`** option. In this case, the parent will wait for the Child task to complete its execution. If any exception is thrown by the child task, then that is going to be handled by the Parent task. And finally, the Parent task status depends on the Child task status.

A task may create any number of attached  child tasks.

If an attached child task throws an exception, then that exception is automatically propagated to the parent task and back to the thread that waits or tries to access the task’s **`Task<TResult>`**.`Result` property. Therefore, by using attached child tasks, you can handle all exceptions at just one point.

`using System;
`using System.Threading;
`using System.Threading.Tasks;
`public class Program
`{
    `public static void Main()
    `{
        `Console.WriteLine("Main Method Started");

        //Creating the Parent Task
        var parentTask = Task.Factory.StartNew(() => {
            Console.WriteLine("Outer Task Started");

            //Creating the Child Task
            var childTask = Task.Factory.StartNew(() => {
                Console.WriteLine("Child Task Started");
                Thread.Sleep(5000);
                Console.WriteLine("Child Task Completed");
            }, TaskCreationOptions.AttachedToParent);

            Console.WriteLine("Outer Task Completed");
        });

        //Waiting for the Parent Task to completed.
        parentTask.Wait();
        Console.WriteLine("Main Method Completed");
        Console.ReadKey();
    }
}

The point that you need to remember is that a Child task can only attach to its parent if its parent does not prohibit attached child tasks. Parent tasks can explicitly prevent child tasks from attaching to them by specifying the **`TaskCreationOptions.DenyChildAttach`** option

The Parent tasks implicitly prevent child tasks from attaching to them if they are created by calling the **`Task.Run`** method.

## Advantages and Disadvantages of Task Based Programming

##### **Advantages of Task-Based Asynchronous Programming in C#:**

Here are some key advantages of using TAP (Task-Based Asynchronous Programming):

###### **Simplified Code Structure:**

- **Readability:** Task-Based Asynchronous Programming allows for writing asynchronous code similar in structure to synchronous code, improving readability.
- **Maintainability:** The code is easier to maintain as it avoids the complexity of callbacks and manual thread management in older models.

###### **Improved Scalability and Performance:**

- **Efficient Resource Utilization:** Asynchronous operations free up the calling thread (typically UI or server threads) to handle other tasks. This improves resource utilization and responsiveness, particularly in UI applications or web services.
- **Scalability:** TAP can enhance the scalability of applications, especially those that handle many concurrent I/O-bound operations.

###### **Language and Framework Support:**

- **Language Integration:** Task-based asynchronous Programming is seamlessly integrated with C# language features, notably async and await keywords, making asynchronous programming more intuitive.
- **Framework Compatibility:** It’s fully supported across the .NET ecosystem, including newer versions of .NET Core and .NET 5/6, ensuring compatibility and ease of use.

###### **Exception Handling:**

- **Simplified Exception Handling:** Exceptions in asynchronous methods can be caught and handled using standard try-catch blocks, unlike older patterns that require more complex handling.

###### **Composability and Flexibility:**

- **Composable Operations:** Tasks can be easily combined and composed. For instance, you can await multiple tasks concurrently using Task.WhenAll, or await the first task to complete using Task.WhenAny.
- **Cancellation Support:** Task-Based Asynchronous Programming supports cancellation using the CancellationToken class, allowing for responsive cancellation of asynchronous operations.

###### **Unified Model for Asynchronous Operations:**

- **Consistency:** TAP provides a unified approach for all asynchronous operations, whether CPU-bound or I/O-bound, creating consistency in how asynchronous code is written and understood across different applications.

###### **Progress Reporting:**

- **Progress Feedback:** The model supports progress reporting out of the box, which is particularly useful in UI applications where you need to update the UI to reflect the progress of an asynchronous operation.

##### **Disadvantages of Task-Based Asynchronous Programming in C#:**

- **Complexity in Error Handling:** Asynchronous programming can make error handling more complex. Exceptions thrown in asynchronous methods are captured and placed on the Task, and they need to be handled using await or by examining the Task object. Unobserved exceptions can lead to unhandled exceptions.
- **Potential for Deadlocks:** Misusing asynchronous and await, especially with Task.Result or Task.Wait(), can lead to deadlocks, particularly in UI applications or when blocking on asynchronous code.
- **Resource Management:** Asynchronous operations can lead to more complex resource management scenarios. Ensuring that resources are properly disposed of or that certain operations are thread-safe adds complexity to the code.
- **Scalability Issues:** Asynchronous programming is great for scalability; improper use (like creating too many tasks or not using I/O-bound asynchronous APIs correctly) can consume many resources and degrade performance.
- **Debugging Difficulty:** Debugging asynchronous code can be more difficult than synchronous code. The execution flow is not linear, making it harder to follow and understand, especially when dealing with multiple concurrent asynchronous operations.
- **Overhead:** Some overhead is associated with managing the state and context of asynchronous operations. For very small operations, the overhead of setting up the asynchronous operation might outweigh its benefits.
- **Improper Usage:** It’s easy to misuse asynchronous programming by applying it where it’s not needed, leading to unnecessary complexity. Not every operation benefits from being made asynchronous.
## `TaskCompletionSource<T>` Class


`   TaskCompletionSource<T>` is a class in C# that represents an asynchronous operation whose completion is controlled explicitly by the developer. It is part of the Task-based Asynchronous Pattern (TAP) and is particularly useful when you need to create and manage asynchronous operations manually.

### Purpose of `TaskCompletionSource<T>`:

The main purpose of `TaskCompletionSource<T>` is to provide a way to produce and complete a `Task<T>` manually. It allows you to create a task and set its result or exception, essentially giving you control over the completion of an asynchronous operation.

### Key Members of `TaskCompletionSource<T>`:

1. **`TaskCompletionSource<T>()`:**
    
    - Constructor: Initializes a new instance of the `TaskCompletionSource<T>` class.
2. **`Task<T> Task`:**
    
    - Property: Gets the `Task<T>` that is controlled by this `TaskCompletionSource<T>`.
3. **`SetResult(T result)`:**
    
    - Method: Sets the result for the associated task. This method transitions the task to the `RanToCompletion` state.
4. **`SetException(Exception exception)`:**
    
    - Method: Sets the exception for the associated task. This method transitions the task to the `Faulted` state.
5. **`SetCanceled()`:**
    
    - Method: Sets the canceled status for the associated task. This method transitions the task to the `Canceled` state.

#### Example Usage:

Here's a simple example to illustrate how `TaskCompletionSource<T>` can be used to create and complete a `Task<T>` manually:

using System;
using System.Threading;
using System.Threading.Tasks;

`class Program
`{
    `static async Task Main()
    `{
        `Task<int> customTask = CustomAsyncOperation();

        // Simulate some work
        await Task.Delay(2000);

        // Complete the customTask with a result
        CustomAsyncOperationCompletion.SetResult(42);

        int result = await customTask;
        Console.WriteLine($"Result: {result}");
    }

    static TaskCompletionSource<int> CustomAsyncOperationCompletion = new TaskCompletionSource<int>();

    static async Task<int> CustomAsyncOperation()
    {
        // Simulate some asynchronous work
        await Task.Delay(1000);

        // Return the result when ready
        return await CustomAsyncOperationCompletion.Task;
    }
`}


In this example:

- `CustomAsyncOperation` simulates an asynchronous operation with a delay.
- The `CustomAsyncOperationCompletion` instance of `TaskCompletionSource<int>` is used to control the completion of the custom asynchronous operation.
- The `SetResult(42)` call in the `Main` method signals the completion of the operation with a result of 42.
- The result is then awaited and printed to the console.


### Constructor Explanation

#### `TaskCompletionSource<T>(object state, TaskCreationOptions creationOptions)`

- **Parameters:**
    
    - `state` (optional): An object that represents data to be associated with the `TaskCompletionSource<T>`.
    - `creationOptions`: A `TaskCreationOptions` enumeration value that controls the behavior of the underlying task.
- **Description:**
    
    - Initializes a new instance of the `TaskCompletionSource<T>` class with both state information and specified task creation options.
- **Use Case:**
    
    - If you want to combine state information and customize the behavior of the underlying `Task<T>`, you can use this constructor.

#### Considerations:

1. **State Object:**
    
    - The `state` parameter allows you to associate additional data with the `TaskCompletionSource<T>`. This data is typically accessed through the `Task.AsyncState` property of the resulting task.
2. **TaskCreationOptions:**
    
    - The `TaskCreationOptions` parameter allows you to control various aspects of the underlying `Task<T>`. For example, you can specify whether the task should be long-running or attached to the parent task.
3. **Default Constructor:**
    
    - If you don't need additional state information or customized creation options, you can use the default constructor without any parameters.

Here's an example demonstrating the use of the `TaskCompletionSource<T>` constructor with state and creation options:

using System;
using System.Threading.Tasks;

`class Program
`{
    `static async Task Main()
    `{
        `TaskCompletionSource<int> tcs = new TaskCompletionSource<int>(42, TaskCreationOptions.AttachedToParent);

        Task<int> customTask = tcs.Task;

        int result = await customTask;
        Console.WriteLine($"Result: {result}");
    }
`}

In this example, the `TaskCompletionSource<T>` is created with a state object of `42` and the `TaskCreationOptions.AttachedToParent` option. This demonstrates how you can leverage both state information and creation options when initializing a `TaskCompletionSource<T>`.

### Methods
#### 1. `SetResult(T result)`

- **Parameter:**
    
    - `result`: The result value to be stored in the completed task.
- **Description:**
    
    - Sets the result for the associated task, transitioning it to the `RanToCompletion` state. Any continuation tasks waiting for the completion of the original task will execute with the specified result.
- **Use Case:**
    
    - Call this method when the asynchronous operation represented by the task is successful, and you have a result to propagate to the awaiting code.

#### 2. `SetException(Exception exception)`

- **Parameter:**
    
    - `exception`: The exception to be stored in the completed task.
- **Description:**
    
    - Sets the exception for the associated task, transitioning it to the `Faulted` state. Any continuation tasks waiting for the completion of the original task will observe this exception.
- **Use Case:**
    
    - Call this method if the asynchronous operation encounters an error or exception that needs to be communicated to awaiting code.

#### 3. `SetCanceled()`

- **Description:**
    
    - Sets the canceled status for the associated task, transitioning it to the `Canceled` state. Any continuation tasks waiting for the completion of the original task will observe a cancellation.
- **Use Case:**
    
    - Call this method when the asynchronous operation is canceled, and you want to propagate the cancellation to awaiting code.

### Considerations:

1. **Single Call:**
    
    - Each of these methods should be called only once. After a completion method is called, subsequent calls to other completion methods will result in an `InvalidOperationException`.
2. **Completion State:**
    
    - The choice of which completion method to call (`SetResult`, `SetException`, or `SetCanceled`) depends on the outcome of the asynchronous operation.
3. **Synchronous vs. Asynchronous:**
    
    - These methods can be called synchronously or asynchronously, depending on your scenario. If the completion happens synchronously, the awaiting code continues execution immediately; otherwise, it waits asynchronously.
### Use Cases:

1. **Adapting Synchronous Code:**
    
    - `TaskCompletionSource<T>` can be used to wrap synchronous code and expose it as an asynchronous operation.
2. **Event-Driven Asynchronous Patterns:**
    
    - It is useful in scenarios where asynchronous operations are event-driven and need to be manually completed based on certain conditions or events.
3. **Creating Custom Asynchronous Operations:**
    
    - When you need to create custom asynchronous operations that don't fit neatly into the existing asynchronous programming model, `TaskCompletionSource<T>` provides a way to craft such operations.
4. **Interop with Callback-Based APIs:**
    
    - It can be used to interoperate with older, callback-based asynchronous APIs by converting them to tasks.

### Considerations:

1. **Error Handling:**
    
    - Ensure proper error handling by using `SetException` to signal failure conditions.
2. **Cancellation:**
    
    - Consider using `SetCanceled` to handle cancellation scenarios.
3. **Lifetime Management:**
    
    - Be aware of the lifetime of the associated task. Once the task is completed, attempting to set its result or exception again will throw an `InvalidOperationException`.
4. **Thread Safety:**
    
    - `TaskCompletionSource<T>` is generally safe for use in multithreaded scenarios, but proper synchronization practices should be followed if shared among multiple threads.
5. **Avoid Deadlocks:**
    
    - Avoid synchronous blocking within the context of the asynchronous operation, as it can lead to deadlocks.

`TaskCompletionSource<T>` is a powerful tool in asynchronous programming, providing flexibility when dealing with custom asynchronous scenarios. However, it should be used judiciously, and care must be taken to ensure proper error handling and synchronization.



### Examples
#### How To Control The Result Of A Task

##### **Example to Understand How to Control the Result of a Task in C#?**

Let us understand this with an example. Let’s create a method that will return a task, but it will be a task in which we will control its status. For a better understanding, please have a look at the below image. Here, we have created one method which is returning a Task and taking a string input value. First, we created an instance of the **TaskCompletionSource** class using one of the overloaded versions of the Constructor. Then we are checking the string value using if-else statements. If the input string value is 1 then we are calling the **SetResult** method on the TaskCompletionSource instance, this method will set the state of the Task (the task holds by the TaskCompletionSource object) to RanToCompletion. Next, if the string value is 2, then we are calling the **SetCanceled** method which will set the state of the Task to Canceled. If the value is neither 2 nor 3, then we are calling the **SetException** method by passing an exception object which will set the state of the Task to Faulted. Finally, we are returning the task by calling the Task property of the TaskCompletionSource class.

Next, in order to check whether the task is completed, faulted, or canceled, we are going to use the following three properties of the Task class.

1. **IsCompleted { get; }:** It returns true if the task has been completed; otherwise false.
2. **IsCanceled { get; }:** It returns true if the task has been completed due to being canceled; otherwise false.
3. **IsFaulted { get; }:** It returns true if the task has thrown an unhandled exception; otherwise false.

using System;
using System.Threading;
using System.Threading.Tasks;
using System.Linq;

`namespace AsynchronousProgramming
`{
  ``  class Program
    `{
      ``  static void Main(string[] args)
    ``    {
      ``      Console.WriteLine("Enter a number between 1 and 3");
    ``        string value = Console.ReadLine();
      ``      SomeMethod(value);
    ``        Console.ReadKey();
      ``  }

        public static async void SomeMethod(string value)
        {
            var task = EvaluateValue(value);
            Console.WriteLine("EvaluateValue Started");
            try
            {
                Console.WriteLine($"Is Completed: {task.IsCompleted}");
                Console.WriteLine($"Is IsCanceled: {task.IsCanceled}");
                Console.WriteLine($"Is IsFaulted: {task.IsFaulted}");
                await task;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            Console.WriteLine("EvaluateValue Completed");
        }

        public static Task EvaluateValue(string value)
        {
            //Creates an object of TaskCompletionSource with the specified options.
            //RunContinuationsAsynchronously option Forces the task to be executed asynchronously.
            var TCS = new TaskCompletionSource<object>(TaskCreationOptions.RunContinuationsAsynchronously);

            if (value == "1")
            {
                //Set the underlying Task into the RanToCompletion state.
                TCS.SetResult(null);
            }
            else if(value == "2")
            {
                //Set the underlying Task into the Canceled state.
                TCS.SetCanceled();
            }
            else
            {
                //Set the underlying Task into the Faulted state and binds it to a specified exception.
                TCS.SetException(new ApplicationException($"Invalid Value : {value}"));
            }

            //Return the task associted with the TaskCompletionSource
            return TCS.Task;
        }
    }
}

#### How to Return A Value

using System;
using System.Threading;
using System.Threading.Tasks;
using System.Linq;

namespace AsynchronousProgramming
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Enter a number between 1 and 3");
            string value = Console.ReadLine();
            SomeMethod(value);
            Console.ReadKey();
        }

        public static async void SomeMethod(string value)
        {
            var task = EvaluateValue(value);
            Console.WriteLine("EvaluateValue Started");
            try
            {
                Console.WriteLine($"Is Completed: {task.IsCompleted}");
                Console.WriteLine($"Is IsCanceled: {task.IsCanceled}");
                Console.WriteLine($"Is IsFaulted: {task.IsFaulted}");
                var result = await task;
                Console.WriteLine($"Result: {result}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
            }
            Console.WriteLine("EvaluateValue Completed");
        }

        public static Task<string> EvaluateValue(string value)
        {
            //Creates an object of TaskCompletionSource with the specified options.
            //RunContinuationsAsynchronously option Forces the task to be executed asynchronously.
            var TCS = new TaskCompletionSource<string>(TaskCreationOptions.RunContinuationsAsynchronously);

            if (value == "1")
            {
                //Set the underlying Task into the RanToCompletion state.
                TCS.SetResult("Task Completed");
            }
            else if(value == "2")
            {
                //Set the underlying Task into the Canceled state.
                TCS.SetCanceled();
            }
            else
            {
                //Set the underlying Task into the Faulted state and binds it to a specified exception.
                TCS.SetException(new ApplicationException($"Invalid Value : {value}"));
            }

            //Return the task associted with the TaskCompletionSource
            return TCS.Task;
        }
    }
}

### Possible Implementation Problems

1. **Incorrect Use of Completion Methods:**
    
    - Calling multiple completion methods (`SetResult`, `SetException`, or `SetCanceled`) on the same `TaskCompletionSource<T>` instance can lead to an `InvalidOperationException`. Ensure that only one completion method is called.
2. **Not Completing the Task:**
    
    - Forgetting to call any of the completion methods could leave the associated task in a pending state, causing any awaiting code to hang indefinitely. Ensure that you call one of the completion methods to signal the completion of the task.
3. **Synchronous Completion:**
    
    - If the completion of the task happens synchronously (i.e., within the same method or thread), be cautious about potential deadlocks or unintended blocking. Ensure that you understand the flow of control and avoid synchronous completions if they could lead to issues.
4. **Handling Exceptions:**
    
    - When using `SetException`, be sure to provide meaningful exception information. A lack of proper error handling may make it difficult for downstream code to diagnose and handle errors.
5. **Lifetime Management:**
    
    - Ensure that the lifetime of the `TaskCompletionSource<T>` aligns with the expected duration of the asynchronous operation. Creating unnecessary instances or keeping instances alive for too long can lead to resource leaks.
6. **Thread Safety:**
    
    - While `TaskCompletionSource<T>` is generally thread-safe, care must be taken when accessing or completing the associated task from multiple threads. Use proper synchronization mechanisms if necessary.
7. **CancellationToken Handling:**
    
    - If your asynchronous operation involves cancellation, ensure that you handle `CancellationToken` appropriately. Calling `SetCanceled` without considering cancellation scenarios may lead to unexpected behavior.
8. **Error Propagation:**
    
    - Be mindful of how errors are propagated through the task. If the underlying asynchronous operation encounters multiple errors, consider how you want to handle and report these errors in the resulting task.
9. **Testing Complexity:**
    
    - Testing asynchronous code that involves `TaskCompletionSource<T>` can be more complex compared to synchronous code. Be thorough in your testing to cover various scenarios, including completion, cancellation, and error cases.
10. **Avoiding Busy Waiting:**
    
    - In scenarios where you are waiting for an external event to complete the task, consider avoiding busy waiting (e.g., continuous polling) as it can lead to resource wastage. Instead, use event-driven or asynchronous patterns.
11. **Race Conditions:**
    
    - When dealing with multiple asynchronous operations, race conditions can occur if not properly managed. Ensure that the sequence of operations and completions align with your intended logic.





## `ValueTask<T>`

### Overview

**`ValueTask’s `mission is performance. The idea is to apply `ValueTask` in high-demand scenarios where there really is a measurable benefit. `ValueTask` is a struct. This implies that it is a value type, unlike Task which is a reference type.**

The official documentation tells us that we can use `ValueTask` or `ValueTask<T>` in the following two conditions.

1. **Condition1**: When the result of the operation is most likely available synchronously, but may be available asynchronously. Relates to asynchronous methods that have the possibility of being executed synchronously.

![[SyncAndAsync.webp]]
If we know that this method is going to return synchronously almost always, then perhaps we should consider using `ValueTask`.


1. **Condition2**: When the operation is used so frequently that the cost of using `Task` or `Task<T>` is significant. In order to improve something in terms of performance, we first need to measure it. Then from the data, we can make decisions and after making a change, we need to take another measurement and see whether the system has really improved or not. This means that unless you have done a previous measurement of the performance of the system, you should not introduce the use of `ValueTask`. So, in general, we should prefer the use of `Task` or `Task<T>`, and only if the performance analysis justifies it, you should switch to using `ValueTask` or `ValueTask<T>`.

`ValueTask<T>` is a type in C# that represents a task that produces a result of type `T`. It is part of the `System.Threading.Tasks` namespace and is primarily used in asynchronous programming to represent an asynchronous operation that may or may not allocate a new task object.

The `ValueTask<T>` type is designed to be more efficient than the traditional `Task<T>` in scenarios where the asynchronous operation might complete synchronously or with very low overhead. This is particularly useful for performance-critical code, such as high-throughput networking or I/O-bound operations.

Here are some key points about `ValueTask<T>`:

1. **Struct Type:** Unlike `Task<T>`, which is a class, `ValueTask<T>` is a struct. This can lead to reduced memory allocations, as it doesn't always allocate a heap object to represent the asynchronous operation.
    
2. **Avoids Allocations in Synchronous Cases:** When the asynchronous operation is already completed or can be completed synchronously, `ValueTask<T>` can avoid allocating a new task object, providing potential performance benefits.
    
3. **Task-Like API:** `ValueTask<T>` implements the same `Task`-like API, so you can use it in async/await patterns just like `Task<T>`.
    
4. **Use Cases:** `ValueTask<T>` is particularly useful in scenarios where the cost of task allocation and management is significant, and the asynchronous operation frequently completes synchronously.
    

Here's an example of using `ValueTask<T>` in an asynchronous method:

```csharp
public async ValueTask<int> MyAsyncMethod()
{
    // Some synchronous code

    // Check a condition that determines whether the asynchronous operation is completed synchronously.
    if (someCondition)
    {
        // Return a completed ValueTask with a result.
        return 42;
    }

    // Perform asynchronous operation
    await SomeAsyncOperation();

    // Return the result of the asynchronous operation
    return result;
}

```

It's worth noting that while `ValueTask<T>` can provide performance benefits in specific scenarios, it's not always the best choice. It's important to consider the characteristics of your specific use case and profile your application to determine whether the use of `ValueTask<T>` is appropriate.

### Limitations Of  `ValueTask<T>`

1. The first one is that they cannot be Cache.
2. You cannot await it multiple times.
3. It does not support multiple continuations.
4. It is not thread-safe with an arbitrary number of threads capable of concurrently registering continuations.
5. They do not support a blocking model where the asynchronous method can block the current thread instead of releasing it using await operator.

### `ValueTask<T>` Problems

1. **Complexity and Correct Usage:**
    
    - Using `ValueTask<T>` correctly requires a good understanding of asynchronous programming and the specific scenarios where it can provide benefits. Incorrect usage may lead to unexpected behavior or decreased performance.
2. **Avoiding Unnecessary Boxings:**
    
    - `ValueTask<T>` is a struct, but if it is boxed (converted to a reference type), some of the potential performance benefits may be lost. Be cautious about operations or patterns that could lead to unnecessary boxing.
3. **Synchronous Completion Overhead:**
    
    - If the asynchronous operation frequently completes synchronously, the overhead of creating and managing a `ValueTask<T>` instance might be unnecessary, and it may be more efficient to use a simple return type or `Task<T>`.
4. **Garbage Collection Considerations:**
    
    - Because `ValueTask<T>` is a struct, it doesn't involve heap allocation as much as `Task<T>` does. However, excessive usage of `ValueTask<T>` could lead to more stack allocations, which might impact garbage collection behavior in some scenarios.
5. **Limited Framework Support:**
    
    - While `ValueTask<T>` is part of the .NET Standard and .NET Core/.NET 5+ platforms, certain frameworks or libraries may not fully support or optimize for `ValueTask<T>`. Compatibility with third-party libraries could be a consideration.
6. **Context Capturing:**
    
    - When using `await` with `ValueTask<T>`, the capturing of synchronization context might lead to unexpected behavior in certain UI or ASP.NET contexts. Consider using `ConfigureAwait(false)` to avoid capturing the context if it's not needed.
7. **Async Method Signature Changes:**
    
    - If you switch from using `Task<T>` to `ValueTask<T>` in method signatures, be aware that this change is breaking for existing consumers of your API. It might require updates in dependent code.
8. **Profiler Compatibility:**
    
    - Some performance profilers may not handle `ValueTask<T>` as gracefully as `Task<T>`. Profiling your code and ensuring compatibility with your chosen profiler is advisable.


## **Canceling Non-Cancellable Tasks with `TaskCompletionSource<T>` 

We are going to see a pattern through which we can Cancel any non-cancellable task in a simple way. When we talk about non-Cancellable tasks, we mean asynchronous methods which do not receive a cancellation token. This is useful when we don’t want to schedule a time out, but rather we want to have a task that does nothing, but we want to be able to cancel.

```csharp
using System;
using System.Threading.Tasks;
using System.Threading;

namespace AsynchronousProgramming
{
    public static class TaskExtensionMethods
    {
        public static async Task<T> WithCancellation<T>(this Task<T> task, CancellationToken cancellationToken)
        {
            var TCS = new TaskCompletionSource<object>(TaskCreationOptions.RunContinuationsAsynchronously);

            using (cancellationToken.Register(state =>
            {
                ((TaskCompletionSource<object>)state).TrySetResult(null);
            },TCS))
            {
                var resultTask = await Task.WhenAny(task, TCS.Task);
                if(resultTask == TCS.Task)
                {
                    throw new OperationCanceledException(cancellationToken);
                }

                return await task;
            };
        }
    }
}
```

```csharp
using System;

using System.Threading.Tasks;

using System.Threading;

namespace AsynchronousProgramming

{

	class Program
	
	{
	
		static CancellationTokenSource cancellationTokenSource;
		
		static void Main(string[] args)
		
		{
		
			SomeMethod();
			
			CancelToken();
			
			Console.ReadKey();
			
			}
			
			public static async void SomeMethod()
			
			{
			
			cancellationTokenSource = new CancellationTokenSource();
			
			try
			
			{
			
			var result = await Task.Run(async () =>
			
			{
			
			await Task.Delay(TimeSpan.FromSeconds(5));
			
			Console.WriteLine("Operation was Successful");
			
			return 7;
			
			}).WithCancellation(cancellationTokenSource.Token);
			
			}
			
			catch (Exception EX)
			
			{
			
			Console.WriteLine(EX.Message);
		
			}
		
			finally
			
			{
			
				cancellationTokenSource.Dispose();
				
				cancellationTokenSource = null;
			
			}

		}

		public static void CancelToken()
		
		{
		
		cancellationTokenSource?.Cancel();
		
		}

	}

}
```

## Stream for Asynchronous Programming

### Overview 

We can use asynchronous steams to create IEnumerable that generates data asynchronously. For this, we can use the IAsyncEnumerable interface. As its name implies IAsyncEnumerable is the asynchronous version of IEnumerable. Therefore, it allows us to perform iterations where the operations are asynchronous.

1. First, we need to use async in the method signature.
2. Second, we need to use `IAsyncEnumerable<T>` interface that is implemented by `Task` and `Task<T>` classes.
3. Third, within the method body, somewhere we need to use await operator.

The benefit is that we are using an asynchronous operation, which means that we are not blocking any threads.

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace AsynchronousProgramming
{
    class Program
    {
        static async Task Main(string[] args)
        {
            await foreach(var name in GenerateNames())
            {
                Console.WriteLine(name);
            }

            Console.ReadKey();
        }

        private static async IAsyncEnumerable<string> GenerateNames()
        {
            yield return "Anurag";
            yield return "Pranaya";
            await Task.Delay(TimeSpan.FromSeconds(3));
            yield return "Sambit";
        }
    }
}
```


Asynchronous streams, introduced in C# 8.0, provide a powerful and convenient way to work with sequences of data asynchronously. They allow you to consume and produce data in an asynchronous and lazy manner. Here's everything you need to know about asynchronous streams:

### Basics:

1. **Syntax:**
    
    - Asynchronous streams are defined using the `IAsyncEnumerable<T>` interface and the `await foreach` statement.
    
```csharp
async IAsyncEnumerable<int> MyAsyncStream()
{
    for (int i = 0; i < 10; i++)
    {
        await Task.Delay(100); // Simulate asynchronous work
        yield return i;
    }
}

// Consume the asynchronous stream
await foreach (var item in MyAsyncStream())
{
    Console.WriteLine(item);
}

```
    
2. **`IAsyncEnumerable<T>`:**
    
    - `IAsyncEnumerable<T>` is the interface that represents an asynchronous stream. It's similar to `IEnumerable<T>`, but it allows asynchronous operations when iterating over the elements.
3. **`await foreach`:**
    
    - The `await foreach` statement is used to iterate over elements of an asynchronous stream. It automatically handles the asynchronous nature of the stream, awaiting each element as it becomes available.

### Creating Asynchronous Streams:

1. **Async Iteration:**
    
    - Use `yield return` in an asynchronous method to produce elements in an asynchronous stream. The method must return `IAsyncEnumerable<T>`.
    
```csharp
async IAsyncEnumerable<int> GenerateAsyncStream()
{
    for (int i = 0; i < 10; i++)
    {
        await Task.Delay(100); // Simulate asynchronous work
        yield return i;
    }
}

```
    
2. **Cancellation Support:**
    
    - Asynchronous streams can be cancelled using a `CancellationToken`.
    

```csharp
async IAsyncEnumerable<int> GenerateAsyncStreamWithCancellation(CancellationToken cancellationToken = default)
{
    for (int i = 0; i < 10; i++)
    {
        await Task.Delay(100, cancellationToken); // Simulate asynchronous work
        yield return i;
    }
}

```
    

### Consuming Asynchronous Streams:

1. **`await foreach`:**
    
    - Use `await foreach` to asynchronously iterate over the elements of an asynchronous stream.
    

```csharp
await foreach (var item in GenerateAsyncStream())
{
    Console.WriteLine(item);
}

```
    
2. **Async Method Consumption:**
    
    - You can consume asynchronous streams within asynchronous methods.
    

```csharp
async Task ConsumeAsyncStream()
{
    await foreach (var item in GenerateAsyncStream())
    {
        Console.WriteLine(item);
    }
}

```
    

### Error Handling:

1. **Try-Catch:**
    
    - Handle exceptions within the asynchronous stream using a try-catch block.
    
```csharp
async IAsyncEnumerable<int> GenerateAsyncStreamWithException()
{
    for (int i = 0; i < 10; i++)
    {
        if (i == 5)
            throw new InvalidOperationException("Exception in the stream");
        
        await Task.Delay(100); // Simulate asynchronous work
        yield return i;
    }
}

```


```csharp
try
{
    await foreach (var item in GenerateAsyncStreamWithException())
    {
        Console.WriteLine(item);
    }
}
catch (InvalidOperationException ex)
{
    Console.WriteLine($"Exception caught: {ex.Message}");
}

```
    

### Benefits:

1. **Lazy Evaluation:**
    
    - Asynchronous streams support lazy evaluation, meaning elements are produced and consumed on-demand.
2. **Efficient Memory Usage:**
    
    - Asynchronous streams are memory-efficient, especially when working with large datasets, as they allow for asynchronous and on-the-fly generation of elements.
3. **Cancellation Support:**
    
    - Asynchronous streams naturally support cancellation, making them suitable for scenarios where cancellation is essential.
4. **Integration with Existing Async Code:**
    
    - Asynchronous streams seamlessly integrate with other asynchronous programming features, such as `async` and `await`, making it easy to work with asynchronous APIs.

### Limitations:

1. **Requires C# 8.0 or Later:**
    
    - Asynchronous streams were introduced in C# 8.0, so you need a compatible compiler and runtime.
2. **Limited Language Support:**
    
    - Not all language constructs that work with synchronous collections are available for asynchronous streams.
3. **Platform Support:**
    
    - While asynchronous streams work well in many scenarios, some platforms and libraries may not fully support them.

Asynchronous streams are a valuable addition to C# for handling asynchronous sequences of data, providing a convenient and efficient way to work with asynchronous operations in a streaming fashion. They are particularly useful in scenarios where lazy evaluation and asynchronous processing are essential.


### How to Cancel Asynchronous Streams

Asynchronous Streams can be canceled by creating a condition that breaks the loop when met. 
But there are other interesting ways to cancel asynchronous streams.




### **Canceling Through IAsyncEnumerable – EnumeratorCancellation:**

```csharp
using System;
using System.Collections.Generic;
using System.Runtime.CompilerServices;
using System.Threading;
using System.Threading.Tasks;

namespace AsynchronousProgramming
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var namesEnumerable = GenerateNames();
            await ProcessNames(namesEnumerable);
            Console.ReadKey();
        }

        private static async Task ProcessNames(IAsyncEnumerable<string> namesEnumerable)
        {
            var CTS = new CancellationTokenSource();
            CTS.CancelAfter(TimeSpan.FromSeconds(5));

            try
            {
                await foreach (var name in namesEnumerable.WithCancellation(CTS.Token))
                {
                    Console.WriteLine($"{name} - Processed");
                }
            }
            catch(Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            finally
            {
                CTS.Dispose();
                CTS = null;
            }
        }

        private static async IAsyncEnumerable<string> GenerateNames([EnumeratorCancellation] CancellationToken token = default)
        {
            yield return "Anurag";
            await Task.Delay(TimeSpan.FromSeconds(3), token);
            yield return "Pranaya";
            await Task.Delay(TimeSpan.FromSeconds(3), token);
            yield return "Sambit";
            await Task.Delay(TimeSpan.FromSeconds(3), token);
            yield return "Rakesh";
        }
    }
}
```


## Yield Keyword

In C#, the `yield` keyword is used in the context of iterators and allows you to create custom iterators for collections. It is used to define an iterator block, which is a method, property, or indexer that contains one or more `yield` statements.

Here's a breakdown of how `yield` works:

1. **Iterator Block:**
    
    - When a method contains the `yield` keyword, it becomes an iterator block. The method is then able to yield a sequence of values to the caller without having to create an entire collection in memory.
2. **Stateful Execution:**
    
    - The `yield` keyword enables stateful execution of the iterator block. When the iterator is called, it starts executing the method until it encounters a `yield` statement. The state of the method is preserved, and the yielded value is returned to the caller.
3. **Suspend and Resume:**
    
    - When a `yield` statement is encountered, the method's state is suspended, and the yielded value is returned to the caller. The next time the iterator is called, it resumes execution right after the last `yield` statement.
4. **Iterating Over Collections:**
    
    - The primary use of `yield` is to simplify the implementation of custom iterators for collections. It allows you to create sequences on-the-fly without explicitly constructing and storing the entire sequence in memory.

		```csharp
		public IEnumerable<int> CountUpTo(int max)
		{
		    for (int i = 1; i <= max; i++)
		    {
		        yield return i;
		    }
		}
		
		// Usage:
		foreach (var number in CountUpTo(5))
		{
		    Console.WriteLine(number);
		}
		
		```
    
    In this example, `CountUpTo` is an iterator method that generates a sequence of numbers from 1 to a specified maximum using the `yield` keyword. The `foreach` loop then iterates over this sequence without the need to create a collection explicitly.
    
5. **Lazy Evaluation:**
    
    - The `yield` keyword enables lazy evaluation of the sequence. Values are produced one at a time as needed, which can be more memory-efficient than eagerly creating the entire sequence.
6. **Multiple Yields:**
    
    - An iterator method can contain multiple `yield` statements, allowing you to yield values in different parts of the method based on certain conditions.
    
```csharp
		public IEnumerable<int> EvenNumbersUpTo(int max)
		{
		    for (int i = 2; i <= max; i += 2)
		    {
		        yield return i;
		    }
		}

```
    

In summary, the `yield` keyword in C# is a powerful tool for creating custom iterators that produce sequences of values on-the-fly, supporting lazy evaluation and simplifying the implementation of iterable collections. It enhances code readability and can lead to more efficient memory usage, especially when working with large datasets.