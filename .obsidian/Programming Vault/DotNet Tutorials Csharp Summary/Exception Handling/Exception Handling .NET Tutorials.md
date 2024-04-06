
# Things to remember:
Runtime errors, compile time errors, `ExceptionManager` (one of the services of common language runtime), the 2 ways in which you deal with exceptions (Exception Handling),Consequences when exception manager identify an error, 4 properties of the Exception class + `InnerException` property, how and why you build user-defined exceptions, try-catch-finally block

# Explanation of the Subject:

The 2 ways of dealing with exceptions is one: writing code that does not raise exceptions in the first place and two: try-catch-finally block. Sometimes using try-catch-finally blocks to control the flow of your program is acceptable, sometimes it is just disgusting and should not be done. Ultimately you are the one that will decide on a case-by-case scenario. It is consensus that using try-catch to control the flow is usually just bad pattern.

System exceptions are the built-in exceptions. Application exceptions are the user-defined one's, even do there is `ApplicationException `class it should not use as base class for your user-defined exceptions, you should use The primordial Exception class for your user-defined exceptions.

When user defining an Exception override the 3 constructors always and the properties if needed. The Exception class is also
the Father of all other exception classes, which is why any exception thrown in a try block can be caught by the catch (Exception ex){} block (this is the default). It is the general catch block by the way and has to always come last, otherwise any error that is thrown is going to be caught by it making the specific catch blocks that come after it useless.

When an error is identified by the` ExceptionManager` it will find the exception class that matches that error and instantiate that exception class an instance of an exception class raises an event (which is what pops up those windows in your screen containing the `Message`, `StackTrace`, `HelpLink`, `Source` and properties) it also terminates the program in the line where the error was found, that unless the error is thrown inside a try block a caught by a catch block. The finally block will execute regardless of the error being caught or not and is used mostly to do the cleanup of unmanaged resources unmanaged resources are the ones that are not manage by the .NET runtime environment: CLR. Unmanaged resources are the resources that are written in languages that are not compiled into IL, like unmanaged C++, C, Java, Rust, etc. C++ may or may not be managed by the CLR.

Inner exceptions are a property of the Exception class that holds a instance of the exception that caused the current exception, if there is one.  `ArggregateException` class is a class that has a unique quality. It's has a ` InnerException's` property that hold's a `ReadOnlyCollection<Exceptions>` which allows you to catch more than one error caught in the same try block, something that happens a lot in programs with concurrency (Asynchronous Programming, Parallelism, Multithreading and so on).
