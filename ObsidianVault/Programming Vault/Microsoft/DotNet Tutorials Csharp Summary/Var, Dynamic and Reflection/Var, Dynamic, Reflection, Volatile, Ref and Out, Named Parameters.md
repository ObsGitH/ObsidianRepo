You dont need to memorize how to write reflection in c#. It is literally written here whenever you need it. What you need is to understand reflection.
# Reflection

Resume: 
Reflection allows you to see the metadata of an assembly. You can: create an instance of a type, bind the type to an existing object, get the type from an existing object and invoke its methods and access its fields and properties. All of this dynamically aka at runtime.

To use reflection you need to: load the `.dll` assembly with `Assembly.Load(assemblyPath)` , import/reference the Reflection library, get the `Type` of the class, create an instance of the `Type` with `Activator.CreateInstance(Type type)`, the the `Type` of the object and then browse the object's metadata with `GetFields(), GetProperties(), GetMethods() and GetMembers()` or call the object's members dynamically with `.InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args)`.

If you did all of this successfully just read the real time uses of reflection an you do not need to read the rest. Go to Dynamic. 

## **What is the Purpose of Reflection in C#?**

Reflection is needed when you want to determine or inspect the content of an assembly. Here, content means the metadata of an assembly like what are the methods in that assembly, what are the properties in that assembly, are they public, are they private, etc.
## **How to Implement Reflection in C#?**

### Step 1

Create a new solution an inside that solution create a new class library project and a console application project.

code inside library project (Class1.cs):

```csharp
using System;
namespace SomeClassLibrary
{
    public class Class1
    {
        public int X;
        private int Y;
        public int P1 { get; set; }
        private int P2 { get; set; }
        public void Method1()
        {
            Console.WriteLine("Method1 Invoked");
        }
        private void Method2()
        {
            Console.WriteLine("Method2 Invoked");
        }
    }
}
```

Now, build the Class Library Project. And once you build the Class Library Project an assembly (with extension .DLL) will be generated inside the Project’s **bin=> Debug** location

 Copy the location of the assembly:

**D:\Projects\ReflectionDemo\SomeClassLibrary\bin\Debug**

remove the Class Library Project from the solution.

### Step 2

Import the reflection namespace. So, basically, first, we need to import the Reflection namespace and then we need to get the type of the object and once we get the type of the object, then we can go and browse the metadata i.e. browse the methods, properties, variables, etc.

Then in the instance of the `Type` class use these methods to get metadata information of the assembly.

Some of the useful methods are as follows:

1. **GetFields():** It returns all the public fields of the current System.Type.
2. **GetProperties():** It returns all the public properties of the current System.Type.
3. **GetMethods():** It returns all the public methods of the current System.Type.
4. **GetMembers():** It returns all the public members of the current System.Type.

```csharp
using System;
//Step1: Import the Reflection namespace
using System.Reflection;

namespace ReflectionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Browse the Properties, Methods, variables of SomeClassLibrary Assembly

            //Step2: Get the Type

            //Get the Assembly Reference
            var MyAssembly = Assembly.LoadFile(@"D:\Projects\ReflectionDemo\SomeClassLibrary\bin\Debug\SomeClassLibrary.dll");

            //Get the Class Reference
            var MyType = MyAssembly.GetType("SomeClassLibrary.Class1");

            //Create an instance of the type
            dynamic MyObject = Activator.CreateInstance(MyType);

            //Get the Type of the Instance
            Type parameterType = MyObject.GetType();

            //Step3: Browse the Metadata

            //To Get all Public Fields/variables
            Console.WriteLine("All Public Fields");
            foreach (MemberInfo memberInfo in parameterType.GetFields())
            {
                Console.WriteLine(memberInfo.Name);
            }

            //To Get all Public Methods
            Console.WriteLine("\nAll Public Methods");
            foreach (MemberInfo memberInfo in parameterType.GetMethods())
            {
                Console.WriteLine(memberInfo.Name);
            }

            //To Get all Public Properties
            Console.WriteLine("\nAll Public Properties");
            foreach (MemberInfo memberInfo in parameterType.GetProperties())
            {
                Console.WriteLine(memberInfo.Name);
            }

            Console.ReadKey();
        }
    }
}
```

In the output of this code get_P1 and set_P1 are the setter and getter methods of the public property P1.

The `Type` class instance(MyType variable) also has some very relevant properties like Name, FullName and Namespace

## **How to Invoke Methods Dynamically Using Reflection in C#?**

Another good feature of using Reflection is that we can invoke the members of an assembly in C# using Reflection. You are going to need the `InvokeMember()` method for that.


```csharp
using System;
//Step1: Import the Reflection namespace
using System.Reflection;

namespace ReflectionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Browse the Properties, Methods, variables of SomeClassLibrary Assembly

            //Step2: Get the type

            //Get the Assembly Reference
            var MyAssembly = Assembly.LoadFile(@"D:\Projects\ReflectionDemo\SomeClassLibrary\bin\Debug\SomeClassLibrary.dll");

            //Get the Class Reference
            var MyType = MyAssembly.GetType("SomeClassLibrary.Class1");

            //Create an instance of the type
            dynamic MyObject = Activator.CreateInstance(MyType);

            //Get the Type of the class
            Type parameterType = MyObject.GetType();

            //Step3: Browse the Metadata

            //To Get all Public Fields/variables
            Console.WriteLine("All Public Members");
            foreach (MemberInfo memberInfo in parameterType.GetMembers())
            {
                Console.WriteLine(memberInfo.Name);
            }

            Console.WriteLine("\nInvoking Method1");

            parameterType.InvokeMember("Method1",
                                        BindingFlags.Public | 
                                        BindingFlags.InvokeMethod | 
                                        BindingFlags.Instance,
                                        null, MyObject, null
                                      );
            
            Console.ReadKey();
        }
    }
}
```

This method of invocation is completely done at runtime (Late Biding). If the method exists at runtime, it will invoke the method else it will throw an exception. That means Reflection in C# does the complete dynamic invocation of the method during runtime.

### **`InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args)`:**

1. **name**: The string containing the name of the constructor, method, property, or field member to invoke. In our case it is Method1.
2. **invokeAttr**: A bitmask comprised of one or more System.Reflection.BindingFlags that specify how the search is conducted. The access can be one of the BindingFlags such as Public, NonPublic, Private, InvokeMethod, GetField, and so on. The type of lookup need not be specified. If the type of lookup is omitted, BindingFlags.Public | BindingFlags.Instance | BindingFlags.Static is used.
3. **binder**: An object that defines a set of properties and enables binding, which can involve the selection of an overloaded method, coercion of argument types, and invocation of a member through reflection. -or- A null reference to use the System.Type.DefaultBinder. Note that explicitly defining a System.Reflection.Binder objects may be required for successfully invoking method overloads with variable arguments. Here, we are passing a null value.
4. **target**: The object on which to invoke the specified member. In our example, the object is MyObject.
5. **args**: An array containing the arguments to pass to the member to invoke. As our method does not take any arguments, we pass null here.


## Real Time Uses of Reflection

1. If you are creating applications like Visual Studio Editors where you want to show internal details i.e. Metadata of an object using Intelligence.
2. In unit testing sometimes we need to invoke private methods to test whether the private members are working properly or not.
3. Sometimes we would like to dump properties, methods, and assembly references to a file or probably show it on a screen.
4. Late binding can also be achieved by using Reflection in C#. We can use reflection to dynamically create an instance of a type, about which we don’t have any information at compile time. So, Reflection enables us to use code that is not available at compile time.
5. Consider an example where we have two alternate implementations of an interface. You want to allow the user to pick one or the other using a config file. With reflection, you can simply read the name of the class whose implementation you want to use from the config file and instantiate an instance of that class. This is another example of late binding using reflection.
6. With Reflection, we can dynamically (at runtime/Late biding)create an instance of a type, bind the type to an existing object, get the type from an existing object and invoke its methods and access its fields and properties.


# Dynamic Type 


dynamic programming languages : data type checking happens at runtime. (late biding)

strongly-tiped programming languages/static languages: data type checking actually happens at compile time. (early biding/ static biding)

Sometime we do not know the data type of an object until runtime, so it is very useful to bypass the compile time type checking and check the type of said object at runtime.  Since we don't know the data type of that object until runtime, the methods and properties also need to be invoked at runtime. (How would invoke a method of an unknown data-type?) 

C# is both dynamic and static at the same time.

C# will raise an compile-time error if you try to store a string inside a int. (static type-checking)

The dynamic keyword bypasses the compile-time type checking.

```csharp
	static void Main(string[] args)
	{
		dynamic str = "Hello";
	}
```

If I tried to use Visual Studio reflection (`Intelligense`) on the str variable by typing: str. literally nothing would appear on my screen because there is no way to know the members of a data type that  can only be type checked at runtime don't you agree? 

```csharp
	static void Main(string[] args)
	{
		dynamic str = "Hello";
		str++;
	}
```

because the str variable is dynamic this will not thrown any errors... well at compile time. 

That is because, at runtime, the moment that the data type of the str variable can be defined the type-checking will happen. 

In this case it will thrown an erro for trying to increment a string type.

Internally, the dynamic keyword uses  **[Reflection]**. What you just have learned.

Built-in data types are type checked at compile time.

Type checking is enforced no matter what in C#. If not early it happens late at compile time.

So, based on the assigned value, with the dynamic data type, the CLR will decide the data type at runtime and then enforce type checking and type safety at runtime.

Conversion from early biding data-types to dynamic data types happens implicitly. 

This is true even with complex types like Customer, Employee, etc. They can also be converted to dynamic types. 

```csharp
using System;
namespace DynamicDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Convert from int to dynamic
            int int1 = 50;
            dynamic dynamic1 = int1; //Explicit cast not required
            Console.WriteLine($"int1 = {int1} & dynamic1 = {dynamic1}");

            //Convert from dynamic to int
            dynamic dynamic2 = 100;
            int int2 = dynamic2; //Explicit cast not required
            Console.WriteLine($"int2 = {int2} & d2 = {dynamic2}");

            Console.ReadKey();
        }
    }
}
```

No errors in this code whatsoever.


## Dynamic Type as Parameter

 it is also possible to use dynamic type as a method parameter so that it can accept any type of value at run time.
 
```csharp
using System;
namespace DynamicDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Calling DisplayValue Function with different types of values
            DisplayValue("Dynamic in C#"); //String
            DisplayValue(true); //Boolean
            DisplayValue(5000); //Integer
            DisplayValue(111.50); //Double
            DisplayValue(DateTime.Now); //Date

            Console.ReadKey();
        }

        public static void DisplayValue(dynamic val)
        {
            Console.WriteLine(val);
        }
    }
}
```

## Useful Dynamic Type Practical Utility

1. **Simplifies Processing of JSON API Data:** In general, when an API returns JSON data, we normally create another strong type class in our application and map the JSON data to that strongly typed class. However, in some scenarios where we do not want to create yet another strongly type class but still want to be able to consume and process the JSON data, we can make use of dynamic type in C#.
2. **Interoperate with other languages like IronRuby or IronPython:** Dynamic in C# makes it possible to interoperate with other programming languages like IronRuby or IronPython. If you are wondering, why do we need to interoperate with other programming languages? Well, to use features of other languages that C# doesn’t support.



## Dynamic Type Making Reflection the Easiest Shit Ever

With dynamic type in C#, it is very easy to write reflection code which in turn makes the code more readable and maintainable.

using reflection from the tap instead of from the dynamic keyword:

```csharp
using System;
namespace DynamicDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Calculator calculator = new Calculator();

            //Using Reflection to Invoke the Add method. 
            var result = calculator.GetType().InvokeMember("Add",
                System.Reflection.BindingFlags.InvokeMethod,
                null,
                calculator,
                new object[] { 10, 20 });

            Console.WriteLine($"Sum = {result}");

            Console.ReadKey();
        }
    }

    public class Calculator
    {
        public int Add(int number1, int number2)
        {
            return number1 + number2;
        }
    }
}
```

using dynamic keyword to achieve reflection:

```csharp
using System;
namespace DynamicDemo
{
    class Program
    {
        static void Main(string[] args)
        {
	        //implicitly, what is happening here is what is happening in the code above this one.
            dynamic calculator = new Calculator();
            var result = calculator.Add(10, 20);
            Console.WriteLine($"Sum = {result}");

            Console.ReadKey();
        }
    }

    public class Calculator
    {
        public int Add(int number1, int number2)
        {
            return number1 + number2;
        }
    }
}
```


## Limitations

In most situations, it is not advisable to use the dynamic type unless you are integrating with a dynamic language or another framework where types are not known at compile time. Because the compiler does not know what type the dynamic variable will eventually become, it’s unable to offer method or property code hints in Visual Studio.

# Var Keyword

Introduced to declare the **Implicitly Typed Local Variables** without specifying an explicit type. The type of local variables will automatically be determined by the compiler based on the right-hand side assigned value of the initialization statement

When we use var keyword the compiler deduces the type of the variable implicitly. The left-hand-side data type will be defined by the compiler during the generation of IL (Intermediate Language) code i.e. at the time of compilation (static/Early biding). Translation: The variables declared with var will have strongly typed data types when they are compiled to intermediate language.

Putting it in simple words, the var keyword is not something like an object which can point to any other data during run time. Once the data type is confirmed by looking at the right-hand side data, it will only point to the valid data as per the data type.

The most important point that you need to keep in mind is that with the var keyword in C#, type checking and type safety are enforced at compile-time only. IT IS NOT DYNAMIC!!!

If var was dynamic `Intelligence` would not work on 'var type' (there is no such thing as a var type) variables.

With the var keyword, the code becomes short and sweet and also becomes readable as shown in the below code. 

The downside is that it can make the code less readable

**The var keyword in C# is used to hold the result of a method whose type is not known such as an anonymous method, LINQ expressions, or generic types.**
##### **When to use the var keyword in C#?**

The var keyword can be used in the for loop, for each loop, using statements, anonymous types, LINQ, and other places.

##### Thigs to remember about var keyword

- The variables declared using the var keyword must be declared and initialized in the same statement else we will get a compile-time error. Because var needs the right hand side (initialization) to define the data type of the variable.
- The variables declared using the var keyword cannot be initialized will a null value else we will get a compile-time error.
- We cannot initialize the multiple implicitly-typed variables using the var keyword in the same statement. like: `var x = 10, y = 20` is not legal. (Because of the first point)
- The var keyword is not allowed to use as a field type at the class level.
	Like:
	```csharp
	public Class Sample
	{
		var x = 20;
	}
	```
## Declaring Anonymous Type with Var keyword

```csharp
using System;
namespace VarKeywordDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Using var keyword to declare Anonymous Type
            //After new keyword we have not specified the type type and hence
            //it becomes an Anonymous Type
            var student = new { Id = 1001, Name = "Pranaya" };
            Console.WriteLine($"Id: {student.Id} Name: {student.Name} ");
            Console.ReadKey();
        }
    }
}
```
## **Var Keyword used in LINQ and Anonymous Types in C#:**

```csharp
using System;
using System.Linq;
namespace VarKeywordDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Use LINQ with Anonymous Type
            string[] stringArray = { "Anurag", "Pranaya", "Raj", "James", "Sara", "Priyanka" };

            //Return names that are greater than 5 characters
            //using LINQ Query Syntax
            object names = from name in stringArray
                           where name.Length > 5
                           select new
                           {
                               name,
                               name.Length
                           };

            //Return names that are greater than 5 characters
            //Method Syntax
            object names2 = stringArray.Where(name => name.Length > 5).
                                          Select(name => new
                                          {
                                              name,
                                              name.Length
                                          });

        }
    }
}
```

when trying to use `Intelligence` on the name object you will not get reflection for the name and name.Length  properties of this object.

What we can do is, we need to define our own class with two properties for Name and Length. And then we need to use that custom class in the LINQ query as shown in the below code.

This can be very boring and time consuming some times. So the var keyword provides an alternative option:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
namespace VarKeywordDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Use LINQ with Anonymous Type
            string[] stringArray = { "Anurag", "Pranaya", "Raj", "James", "Sara", "Priyanka" };

            //Return names that are greater than 5 characters
            //using LINQ Query Syntax
            var names = from name in stringArray
                           where name.Length > 5
                           select new
                           {
                               Name = name,
                               Length = name.Length
                           };

            //Return names that are greater than 5 characters
            //Method Syntax
            var names2 = stringArray.Where(name => name.Length > 5).
                                          Select(name => new
                                          {
                                              Name =name,
                                              Length = name.Length
                                          });

            //Accessing the Data using Foreach Loop
            foreach (var item in names)
            {
                Console.WriteLine($"Name={item.Name} and Length = {item.Length}");
            }
            Console.ReadKey();
        }
    }
}
```

now the item variable activates `Intelligence` without the need for us to create a custom class for this anonymous object:

```csharp
	new {
		Name = name,
		Length = name.Length
	}
```

If you click in the names variable you will see that it's type is: Anonymous Types.

**So, in situations like this where we don’t know what kind of properties or columns the LINQ query is going to return i.e. anonymous type, we can use the var keyword. If you use the object, then you will have boxing and unboxing which affect the performance as well as you will not get any intelligence support. With var, we don’t have performance issues as boxing and unboxing are not there as well as we will get Intelligence support and compile-time error checking.**


## Var Keyword for Foreach Loops

```csharp
using System;
using System.Collections.Generic;
namespace VarKeywordDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            // List of Strings
            List<string> nameList = new List<string> { "Anurag", "Pranaya", "Raj", "James", "Sara", "Priyanka" };

            //Using var Keyword in Foreach Loop
            foreach (var name in nameList)
            {
                Console.WriteLine(name);
            }
              
            Console.ReadKey();
        }
    }
}
```



## Var Keyword with For Loops

```csharp
using System;
namespace VarKeywordDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            // Using var Keyword in For Loop
            for (var index = 1; index <= 5; index++)
            {
                Console.WriteLine(index);
            }

            Console.ReadKey();
        }
    }
}
```

# Dynamic vs Var

## Dynamic

- late bounded/checked at runtime/dynamically evaluated
- at runtime, the variable dynamically goes and uses reflection internally and tries to invoke the Method or property dynamically.
- Bypasses compile time type checking.
- We can declare dynamic type class properties (What about other data member?)
- dynamic type allows the type of value assigned to be changed. That means if we assign an integer value to a var variable, we can also assign a string value or another type of value.
- We can use a dynamic data type as function’s return type or a function or a parameter of a function in C#
## Var

- early bounded/checked at compile time/statically evaluated
- `Intelligence` reflection works because the type is known at compile time.
- Does not bypass compile time type checking
- Cannot declare var keyword data members
- var does not allow the type of value assigned to be changed. That means if we assign an integer value to a var variable, we cannot assign a string or another type of value.
- We can not use the var keyword either as a function’s return type or a function or a parameter of a function in C#


# Dynamic vs Reflection

Both dynamic and reflection use dynamic invocation. So, we should understand in what scenarios we need to use dynamic and in what other scenarios we need to use reflection.


Reflection should be used for: inspecting meta data, Invoking Public members and Invoking Private members

Dynamic should be used for: Invoking public members and Caching

Dynamic has priority over reflection when it comes to invoking public members because it is way easier and convenient to implement than old-school reflection.

Reflection should be used for Inspection of meta data and invocation of private data members.

1. **Inspect Metadata:** Reflection can inspect the metadata but dynamic can’t inspect the metadata of an assembly.
2. **Invoking Public Members:** We can invoke the public members of an object using both reflection and dynamic. It is recommended to use dynamic because of its simplicity and easy-to-use feature.
3. **Invoking Private Members:** We can invoke the private members of an object using reflection but cannot invoke the private members of an object using dynamic.
4. **Caching:** We can cache using dynamic but not with reflection.


# Volatile Keyword

The Volatile Keyword in C# is one of the not discussed keywords. You can also say that not talked keyword or unknown keyword in C# language. More than 95% time, you will never use this keyword. But in case you are developing multi-threaded applications, and if you want to handle concurrency in a better manner, then you can use this volatile keyword.

It indicates that a field might be modified by multiple threads that are executing at the same time. Which will make the compiler, the runtime system, and even the hardware to maybe rearrange reads and writes to memory locations for performance reasons.

Fields that are declared volatile are excluded from certain kinds of optimizations.

## Volatile Keyword Solving Concurrency Issue in Multithreaded Algorithm

```csharp
using System;
using System.Threading;

namespace VolatileKeywordDemo
{
    class Program
    {
        //Loop Varible
        private bool _loop = true;

        static void Main(string[] args)
        {
            //Calling the SomeMethod in a Multi-threaded manner
            Program obj1 = new Program();
            Thread thread1 = new Thread(SomeMethod);
            thread1.Start(obj1);

            //Pauses for 20 MS
            Thread.Sleep(20);

            //Setting the _loop value as false
            obj1._loop = false;
            Console.WriteLine("Step2:- _loop value set to False");
            Console.ReadKey();
        }

        //Simple Method
        public static void SomeMethod(object obj1)
        {
            Program obj = (Program)obj1;
            Console.WriteLine("Step1:- Entered into the Loop");
            while(obj._loop)
            {

            }
            Console.WriteLine("Step3:- Existed From the Loop");
        }
    }
}
```

Now, let us run the above code in Release mode and see the output.

output: it will enter into the loop, after 20 milliseconds it will set the _loop variable value to false. But even after the loop value is set to False, the while loop is not exited. That means the thread (thread1) is still thinking that the _loop variable value is True. It means the value that we set inside the Main method (setting _loop variable to False) is not getting reflected inside the thread1 (i.e. inside the SomeMethod).

In order to understand why we are facing these concurrency issues, we need to understand the memory architecture of the above program.

We have two threads i.e. Main thread executing our application including the Main method, and thread2 executing the SomeMethod. And the variable _loop will be stored inside the Main memory and this variable is accessed by both threads. The Main memory will keep track of the _loop variable value. Here, the Main thread sets the _loop value to True. So, inside the Main memory, the _loop variable value will be Ture.

See, in order to improve the efficiency, these threads do not access the Main memory directly, rather they have their own local memory which is in sync with the main memory. Let us say the thread1 local memory is LM1 and the Main thread local memory is LM2. These local memories will have that loop variable. And there is a synchronization happening here and there between the main memory and the local memory of the threads.

__loop variable will be set to true in LM1, LM2 and the Main Memory at first.

After some time, the Main thread sets the _loop values to false. LM2 and Main Memory now have the __loop variable holding the value false. Which means the local thread memory (LM1) still has __loop variable holding the value true.

The reason is that the local memory of Thraed1 and Main memory, have not got a sync. Because of this reason, the updated data by the Main thread was not visible to Thread1.

LM1 is does not synchronize with the Main Memory, that happens because the Main Memory never synchronizes with local memory.

### To solve this problem just mark the __loop variable with volatile keyword

```csharp
using System;
using System.Threading;

namespace VolatileKeywordDemo
{
    class Program
    {
        //Loop Varible
        private volatile bool _loop = true;

        static void Main(string[] args)
        {
            //Calling the SomeMethod in a Multi-threaded manner
            Program obj1 = new Program();
            Thread thread1 = new Thread(SomeMethod);
            thread1.Start(obj1);

            //Pauses for 20 MS
            Thread.Sleep(20);

            //Setting the _loop value as false
            obj1._loop = false;
            Console.WriteLine("Step2:- _loop value set to False");
            Console.ReadKey();
        }

        //Simple Method
        public static void SomeMethod(object obj1)
        {
            Program obj = (Program)obj1;
            Console.WriteLine("Step1:- Entered into the Loop");
            while(obj._loop)
            {

            }
            Console.WriteLine("Step3:- Existed From the Loop");
        }
    }
}
```

Now whenever the while loop accesses this _loop variable, first, it will go and sync this local memory _loop variable data with the Main memory _loop variable data and then it will execute the loop.

So, you need to use the volatile keyword while you are doing multi-thread applications and especially when you accessing data that is concurrently updated by different threads and you want that updated data to be used by other threads. The volatile keyword ensures that the data you are accessing is up to date or you can say it is in sync with the main memory.

Both in C# and Java, the keyword volatile tells the compiler that the value of the variable must never be cached as its value may change outside of the scope of the program itself. The compiler will then avoid any optimizations that may result in problems if the variable changes “outside of its control”.

### **Why do we run the application in Release mode?**

See, by default, Debug includes debugging information in the compiled files (allowing easy debugging) while release usually has optimizations enabled. So, when you are developing an application, for easy debugging you need to use Debug. But, while deploying the application to the server, for better performance we need to publish the files in release mode.

If you run the the previous code without the volatile keyword in Debug mode the code will work just fine. That is because the optmizations were not applied to your program. 

# Ref and Out Keywords

##### **Difference1:** **Updating the Ref and Out variables Inside the Method**

When we call a method with the “out” variable, the method has to update the out variable inside the function and it is mandatory. But this is not mandatory if you are using the ref variable.

So, the first point that you need to keep in mind is that, if you are declaring some out variables, then it is mandatory or compulsory to initialize or update the out variables inside the method body else we will get a compiler error. But with the ref, updating the ref variable inside a method is optional.


##### **Difference2: Initializing the Ref and Out variables while passing to the Method**

When we are passing the ref parameter as arguments, it is mandatory to initialize the ref parameter before passing it to the method else we will get compile time error. This is because with the ref parameter, updating the value inside the method is optional. So, before passing the ref parameter, it should be initialized. On the other hand, initializing an out parameter is optional.

##### Other things

The point that you need to remember is that the ref keyword is used to pass data pass in bi-directional and the out keyword is used to pass the data only in unidirectional i.e. returning the data.

You need to use the ref parameters when you want to pass some values to the function and you expect the values to be modified or updated by the function and give you back.

With the OUT parameter, we are only expecting the output from the method. We don’t want to give any input. So, we need to use the out parameter, when we don’t want to pass any value to the function and we expect the function should and must update the variable and return the value.

##### **Changes to OUT Parameter in C# 7:**

The Out Parameter in C# never carries value into the method definition. So, it is not required to initialize the out parameter while declaring. So, here initializing the out parameter is useless.

With the introduction of C# 7, now it is possible to declare the out parameters directly within the method. Which eliminate the need to split the usage of the C# out variable into two parts.

```csharp
using System;
namespace RefvsOutDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Use of out in C#
            Add(10, 20, out int Number);
            Console.WriteLine(Number);
            Console.ReadKey();
        }
        
        public static void Add(int num1, int num2, out int Result)
        {
            Result = num1 + num2;
        }
    }
}
```

# Named Parameters

Allow you pass values to parameters in a user-defined order instead of the predefined order.

You can combine passing parameters in a regular way with passing named parameters as long as the named parameters come after the regular parameters.

The power of named parameters is honed when combined with Optional Parameters.

Just look at this shit:

```csharp

static void Main(string[] args)
{
	Method("Efssdf", 34, thePersonReadingThisIsGay: false);
}

static void Method(string name, int age, string job = "nothing", bool isGay = false, bool thePersonReadingThisIsGay = true)
{

}
```

powerful shit. Every optional parameter that you do not want to pass a value to can be skipped with named parameters.