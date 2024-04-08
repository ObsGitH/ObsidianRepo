
##### **What is COM?**

COM stands for Component Object Model. The COM is one of Microsoft’s Technologies. Using this technology, we can develop Windows applications as well as Web applications. In COM, VB is the programming language that is used to implement Windows applications and ASP is the technology used to implement Web applications.

##### **What are the disadvantages of COM?**

1. Incomplete object-oriented programming means it will not support all the features of OOPs.
2. Platform dependent means COM applications can run on only Windows OS.

To overcome the above problems, the DOT NET Framework comes into the picture.


##### **What .NET Represents?**

NET stands for Network Enabled Technology. In .NET, dot (.) refers to object-oriented and NET refers to the internet. So the complete .NET means through object-oriented we can implement internet-based applications.

##### **What is a Framework?**

A Framework is a Software. Or you can say a framework is a collection of many small technologies integrated together to develop applications that can be executed anywhere.

##### **What is .NET Framework?**

According to Microsoft, .NET Framework is a software development framework for building and running applications on Windows. The .NET Framework is part of the .NET platform (.NET, .NET core, Xamarin/Mono), a collection of technologies for building apps for Linux, macOS, Windows, iOS, Android, and more.

##### **Different Types of .NET Framework.**

1. **.NET Framework** is the original implementation of .NET. It supports running websites, services, desktop apps, and more on Windows.
2. **.NET** is a cross-platform implementation for running websites, services, and console apps on Windows, Linux, and macOS. .NET is open source on GitHub. .NET was previously called .NET Core.
3. **Xamarin/Mono** is a .NET implementation for running apps on all the major mobile operating systems, including iOS and Android.

#### **What does the .NET Framework Provide?**

1. **BCL** (Base Class Libraries)
2. **CLR** (Common Language Runtime)


##### **.NET Framework Base Class Libraries:**

resume: are located in the GAC at **%windir%assembly** and you can find very important classes, structs, interfaces and APIs that really are the foudation of the .NET framework. (Like the definition of the predefined primitive and complex data types for example.)

The .NET Framework Class Libraries are designed by Microsoft. Without Class Libraries, we can’t write any code in .NET. So, Class Libraries are also known as the Building block of .NET Programs. These are installed into the machine when we installed the .NET framework. Class Libraries contains pre-defined classes and interfaces and these classes and interfaces are used for the purpose of application development. The Class Library also provides a set of APIs and types for common functionality. It provides types for strings, dates, numbers, etc.


the default location for the Global Assembly Cache is **%windir%\Microsoft.NET\assembly**.

In earlier versions of the .NET Framework, the default location is **%windir%\assembly**.


(this is done by the .NET execution engine/runtime compiler/CLR )
**In the .NET framework, the Code is Compiled Twice.**

1. In the 1st compilation, the source code is compiled by the respective language compiler and generates the intermediate code which is known as **MSIL (Microsoft Intermediate Language)** or **IL (Intermediate language code)** Or **Managed Code**.
2. In the 2nd compilation, **MSIL** code is converted into **Native code** (native code means code specific to the Operating system so that the code is executed by the Operating System ) and this is done by **CLR**.

##### **What is exactly DOTNET?**

.NET is a framework tool that supports many programming languages and many technologies.

Technologies supported by the .NET framework are as follows

1. ASP.NET (Active Server Pages.NET)
2. ADO.NET (Active Data Object.NET)
3. WCF (Windows Communication Foundation)
4. WPF (Windows Presentation Foundation)
5. WWF (Windows Workflow Foundation)
6. AJAX (Asynchronous JavaScript and XML).  I think you use this with ASP.NET I am not sure do.
7. LINQ (Language Integrated Query)
8. Entity Framework


##### **How is a .NET Application Compiled and Run?**

source code written in .NET supported programming languages are all compiled with their respective compilers to IL code thanks to the CTS(Common Type System) and CLS(Common Language Specification) provided by the CLR(Common Language Runtime) which is the runtime environment of the .NET framework, the intermediate language code is stored in .exe(executable) or .dll(dynamically linked library) files called assemblies.

When you run the application this IL code is, again,  compiled by the JIT (Just-In-Time) compiler (also another part of the CLR). This time the result of the compilation will be Native Code/Binary code which is code understandable by the computer at the hardware level.

CTS ensures that all data types from all .NET supported programming languages can be compiled into the same IL code data type. int from c# and INTEGER from VB are both converted into INT32 in IL code for example.

CLS allows you to write source code that is interoperable between all supported .NET supported programming languages by forcing you to follow Common Language Specifications which are syntax specifications (specifications to write your program in a certain way) that can be enforced in all .NET supported programming languages (if you want to) and are specifications that satisfy every single .NET supported programming language. 

If followed, the CLS will allow you to write one program using many different .NET supported programming languages at once.


Both CTS and CLS are crucial for achieving interoperability between different programming languages in .NET.
##### **How to check whether the code is CLS Compliant or not in .NET Framework**

**Step1: Import the System namespace as**  
**using System;**

**Step2: Add the following CLSCompliant attribute at the bottom of this file and set its value to true**:  **[assembly: CLSCompliant(true)]**

You can decorate an assembly, namespace, class, data member with this attribute. Whatever thing you decorate with it will have to follow the CLS.


##### **Why Partial Compiled Code or Why Not Fully Compiled Code?**

The reason is very simple. We don’t know in what kind of environment .NET Code is going to be run (for example, Windows XP, Windows 7, Windows 10, Windows 11, Windows Server, etc.). We also don’t know the CPU configuration, Machine Configuration, Security Configuration, etc. of the corresponding operating system where we want to run our .NET Applications.

Therefore IL  code is partially compiled, and at runtime, this Microsoft Intermediate language (MSIL) or Intermediate language (IL) code is compiled to Machine-Specific instructions or you can say binary code based on underlying Operating System, CPU, Machine Configuration, etc. by the CLR in .NET Framework.


##### **Common Language Runtime (CLR) in .NET Framework:**

1. Security Manager
2. JIT Compiler
3. Memory Manager
4. [**Garbage Collector**](https://dotnettutorials.net/lesson/garbage-collector/)
5. [**Exception Manager**](https://dotnettutorials.net/lesson/exception-handling-csharp/)
6. [**Common Language Specification (CLS)**](https://dotnettutorials.net/lesson/common-language-specification/)
7. [**Common Type System (CTS)**](https://dotnettutorials.net/lesson/common-type-system/)


##### **Security Manager:**

There are basically two components to managing security. They are as follows:

1. **CAS (Code Access Security)**:  checks if the current user has the privildeges required to allow him access to a certain assembly/code
2. **CV (Code Verification)**: checks the code/assembly rights/authorities to see if it can be executed safely by your OS.

Memory Manager:

Responsible for the memory allocation of objects and variables. The reason why this is done at runtime is because the memory architecture of the program will be different in different Operating Systems since the same data types have require different amounts of memory(bytes/bits) in different Operating Systems.


Garbage Collector:

A repetitive background process thread that is responsible for the memory deallocation of objects stored in the heap memory. The deallocation of values and variables in the Stack is done procedurally (LIFO procedure) and does not need the interference of the Garbage Collector

Exception Manager:

Responsible for creating instances of Exception classes when an  runtime error happens and throwing that instance. Which leads to an event that pops-up in your screen plus abruptly terminates the program.

Is responsible for given the control of the program flow to the catch and finally blocks.

All of the other components of the CLR were already explained.





