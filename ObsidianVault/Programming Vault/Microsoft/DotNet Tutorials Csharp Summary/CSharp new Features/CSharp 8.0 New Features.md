

# 1. **Readonly Structs

## Read Only Fields

 If the readonly modifier is used with a value type field, then the field is immutable. And if the readonly modifier is used with a reference type field, then the readonly modifier prevents the field from being replaced by the different instances of the reference type.

## Preview

The readonly keyword is a C# modifier that is used to limit access to all the data members of a struct. If the readonly modifier is used in the declaration of a struct, then:

1. The members of the struct are read-only.
2. None of the members can have setters i.e. they only have getters.
3. A parameterized constructor must be used to initialize the data members of a struct.
4. The struct is immutable.
5. The “this” variable cannot be changed in any other method except the constructor. That means the struct readonly members can only be changed through the constructor.

If you do not want to declare the whole structure type as read-only, then you can apply the readonly modifier to the structure member. When we apply the struct member as readonly, then those members don’t modify the whole state of the structure. 

Example of a structure:

```csharp
using System;
namespace ReadOnlyStructsDemo
{
    //Creating a Structure using the struct
    //struct in C# are value type
    public struct Rectangle
    {
        public double Height { get; set; }
        public double Width { get; set; }

        //Property Expression Bodied Member
        public double Area => (Height * Width);

        //Constructor Initializing the Height and Width Properties
        public Rectangle(double height, double width)
        {
            Height = height;
            Width = width;
        }

        //Overriding the Object Class ToString() Method to return the Height, Width, and Area
        //But ToString Method is not chaning the Values of Height, Width, and Area Properties
        public readonly override string ToString()
        {
            return $"(Total area for height: {Height}, width: {Width}) is {Area}";
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            //Creating an Instance of the Rectangle Class
            Rectangle rectangle = new Rectangle(10, 20);

            //Get the Height of Rectangle
            Console.WriteLine("Height: " + rectangle.Height);

            //Get the Width of Rectangle
            Console.WriteLine("width: " + rectangle.Width);

            //Get the Area of Rectangle
            Console.WriteLine("Rectangle Area: " + rectangle.Area);

            //Get the Height, Width, and Area of Rectangle
            //In this case, it will internally invoke the override ToString Method
            Console.WriteLine("Rectangle: " + rectangle);
            Console.ReadKey();
        }
    }
}
```

the ToString() method doesn’t modify the state of the structure, so we can add the readonly modifier to the declaration of the ToString() method as shown in the below example.

**Call to Non-Readonly member ‘Rectangle.Area.get’ from a ‘readonly’ member results in an implicit copy of ‘this’**

The compiler warns you when it needs to create an implicit copy of this. The Area property doesn’t change state, so we can fix this warning by adding the readonly modifier to the declaration of the Area

```csharp
using System;
namespace ReadOnlyStructsDemo
{
    //Creating a Structure using the struct
    //struct in C# are value type
    public struct Rectangle
    {
        public double Height { get; set; }
        public double Width { get; set; }

        //Property Expression Bodied Member
        public readonly double Area => (Height * Width);

        //Constructor Initializing the Height and Width Properties
        public Rectangle(double height, double width)
        {
            Height = height;
            Width = width;
        }

        //Overriding the Object Class ToString() Method to return the Height, Width, and Area
        //But ToString Method is not chaning the Values of Height, Width, and Area Properties
        public readonly override string ToString()
        {
            return $"(Total area for height: {Height}, width: {Width}) is {Area}";
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            //Creating an Instance of the Rectangle Class
            Rectangle rectangle = new Rectangle(10, 20);

            //Get the Height of Rectangle
            Console.WriteLine("Height: " + rectangle.Height);

            //Get the Width of Rectangle
            Console.WriteLine("width: " + rectangle.Width);

            //Get the Area of Rectangle
            Console.WriteLine("Rectangle Area: " + rectangle.Area);

            //Get the Height, Width, and Area of Rectangle
            //In this case, it will internally invoke the override ToString Method
            Console.WriteLine("Rectangle: " + rectangle);
            Console.ReadKey();
        }
    }
}
```

The auto-implemented getters are read-only in the Auto-implemented Properties. In other properties you need to explicitly say that this property, getter or setter does not change state.


## Readonly Struct

In readonly structure, we declare the structure with the readonly modifier and readonly structure indicates that the given structure is immutable. When you create a readonly structure, it is necessary to use a readonly modifier with its fields, if you do not do this, then the compiler will give an error.

Readonly struct:

```csharp
using System;
namespace ReadOnlyStructsDemo
{
    //Creating a Structure using the struct
    //struct in C# are value type
    public readonly struct Rectangle
    {
        public readonly double Height { get; }
        public readonly double Width { get; }

        //Property Expression Bodied Member
        public double Area => (Height * Width);

        //Constructor Initializing the Height and Width Properties
        public Rectangle(double height, double width)
        {
            Height = height;
            Width = width;
        }

        //Overriding the Object Class ToString() Method to return the Height, Width, and Area
        //But ToString Method is not chaning the Values of Height, Width, and Area Properties
        public override string ToString()
        {
            return $"(Total area for height: {Height}, width: {Width}) is {Area}";
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            //Creating an Instance of the Rectangle Class
            Rectangle rectangle = new Rectangle(10, 20);

            //Get the Height of Rectangle
            Console.WriteLine("Height: " + rectangle.Height);

            //Get the Width of Rectangle
            Console.WriteLine("width: " + rectangle.Width);

            //Get the Area of Rectangle
            Console.WriteLine("Rectangle Area: " + rectangle.Area);

            //Get the Height, Width, and Area of Rectangle
            //In this case, it will internally invoke the override ToString Method
            Console.WriteLine("Rectangle: " + rectangle);
            Console.ReadKey();
        }
    }
}
```
# 2. **Default Interface Methods or Virtual Extension Methods (Same Thing)**

Please use this feature carefully. Otherwise, it can easily lead to violating the **single responsibility principles.(One of the SOLID principles of OOP design)** !!! I Need to learn SOLID.

Before C# 8.0 interfaces only contain the declaration of the members (Methods, Properties, Events, and Indexers), but from C# 8.0, it is possible to add members as well as their implementation within the interface.

Now we are allowed to add a method with its implementation within the interface without breaking the existing implementation of the interface, such type of method is known as the default interface method (also known as the virtual extension method).

It allows us to add new functionality to the interfaces of our libraries and ensure backward compatibility with code written for older versions of those interfaces.

## **Allowed in the Interface in C#:**

1. A body for a method or indexer, property, or an event accessor
2. Private, protected, internal, public, virtual, abstract, sealed, static, extern
3. Static fields
4. Static methods, properties, indexers, and events.


### **Not allowed in the interface in C#:**

1. Instance state, instance fields, instance auto-properties (This might be incorrect. Idk if some of this are legal or if they are changing implicitly to static)
1. override keyword is currently not possible, but this might be changed in C# 9


## Modifiers

An interface with C# 8 is extended to accept modifiers such as protected, internal, public, and virtual. By default, the default methods of an interface are virtual. If you want then you can also make them sealed and private by using the sealed or private modifier.

 if you are not providing implementation to interface methods, then by default they are going to be abstract and public.
 
```csharp
using System;
namespace DefaultInterfaceMethodsDemo
{
    interface IDefaultInterfaceMethod
    {
        // By default, this method is virtual. The virtual keyword is not required here
        virtual void DefaultMethod()
        {
            Console.WriteLine("I am a default method in the interface!");
        }

        // By default, this method is abstract and public, so the abstract and public keyword not required here
        public abstract void Sum();
    }

    interface IOverrideDefaultInterfaceMethod : IDefaultInterfaceMethod
    {
        //Overriding DefaultMethod in Child Interface
        void IDefaultInterfaceMethod.DefaultMethod()
        {
            Console.WriteLine("I am an overridden default method!");
        }
    }

    class AnyClass : IDefaultInterfaceMethod, IOverrideDefaultInterfaceMethod
    {
        //Providing Implementation for Interface Abstract Method
        public void Sum()
        {
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            //Create an Instance of AnyClass and store it inside the IDefaultInterfaceMethod Parent Reference Variable
            IDefaultInterfaceMethod anyClass = new AnyClass();
            //Call to DefaultMethod will Execute from the IOverrideDefaultInterfaceMethod Interface
            anyClass.DefaultMethod();

            //Create an Instance of AnyClass and store it inside the IOverrideDefaultInterfaceMethod Parent Reference Variable
            IOverrideDefaultInterfaceMethod anyClassOverridden = new AnyClass();
            //Call to DefaultMethod will Execute from the IOverrideDefaultInterfaceMethod Interface
            anyClassOverridden.DefaultMethod();

            Console.ReadKey();
        }
    }
}
```

## Example

Interface with default implementation:
```csharp
using System;
namespace DefaultInterfaceMethodsDemo
{
    //Interface with Deafult Method
    interface IDefaultInterfaceMethod
    {
        //Default Implementation Method
        public void DefaultMethod()
        {
            Console.WriteLine("I am a default method in the interface!");
        }
    }

    //Class Inheriting from an Interface which have one Default Implementation Method
    class AnyClass : IDefaultInterfaceMethod
    {
        //AnyClass Have No Idea about the Default Implementation Interface Method
    }

    class Program
    {
        static void Main(string[] args)
        {
            //Create an Instance of Child class and store it on the Parent Class Reference Variable
            IDefaultInterfaceMethod anyClass = new AnyClass();

            //Call the Default Implementation Interface Method
            anyClass.DefaultMethod();
            Console.ReadKey();
        }
    }
}
```

If  you try this using a `anyClass` variable of `AnyClass` type an error will be raised:

```csharp
using System;
namespace DefaultInterfaceMethodsDemo
{
    //Interface with Deafult Method
    interface IDefaultInterfaceMethod
    {
        //Default Implementation Method
        public void DefaultMethod()
        {
            Console.WriteLine("I am a default method in the interface!");
        }
    }

    //Class Inheriting from an Interface which have one Default Implementation Method
    class AnyClass : IDefaultInterfaceMethod
    {
        //AnyClass Have No Idea about the Default Implementation Interface Method
    }

    class Program
    {
        static void Main(string[] args)
        {
            //Create an Instance of Child class and store it on the Child Class Reference Variable
            AnyClass anyClass = new AnyClass();

            //Call the Default Implementation Interface Method
            anyClass.DefaultMethod();
            Console.ReadKey();
        }
    }
}
```

This will thrown an error. This error proves that `AnyClass` does not have access to the default implementation of the `IDefaultInterfaceMethod` interface. Which is a really good and awesome thing!

If we use an abstract class instead of an interface, then using the `AnyClass `object, we can access the `DefaultMethod`.

You are not allowed to have explicit access modifiers in overriden interface methods

```csharp
using System;
namespace DefaultInterfaceMethodsDemo
{
    interface IDefaultInterfaceMethod
    {
        // By default, this method is virtual. The virtual keyword is not required here
        virtual void DefaultMethod()
        {
            Console.WriteLine("I am a default method in the interface!");
        }

        // By default, this method is abstract, so the abstract keyword not required here
        abstract void Sum();
    }

    interface IOverrideDefaultInterfaceMethod : IDefaultInterfaceMethod
    {
        //Overriding DefaultMethod in Child Interface
        //Access Specifier is not allowed
        public void IDefaultInterfaceMethod.DefaultMethod()
        {
            Console.WriteLine("I am an overridden default method!");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.ReadKey();
        }
    }
}
```

this generates a compile time error

##### **Diamond Problem with Multiple Inheritance in C#**

We can get the diamond problem, or ambiguity error because of allowing multiple inheritances with the default implementation in the interface method.

```csharp
using System;
namespace DefaultInterfaceMethodsDemo
{
    interface A
    {
        void Method();
    }
    interface B : A
    {
        void A.Method()
        {
            System.Console.WriteLine("I am From Interface B");
        }
    }
    interface C : A
    {
        void A.Method()
        {
            System.Console.WriteLine("I am From Interface C");
        }
    }
    class D : B, C
    {
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

**The interface member ‘A.Method()’ does not have the most specific implementation. Neither ‘B.A.Method()’, nor ‘C.A.Method()**. So, the compiler will get confused between the implementation from B and C interfaces and hence give you a compile-time error.

The .NET development team has decided to solve the Diamond Problem by taking the most specific override at runtime:

**Diamonds with Classes:** A class implementation of an interface member should always win over a default implementation in an interface, even if it is inherited from a base class. Default implementations are always a fallback only when the class does not have any implementation of that member at all.

Coming back to our example, the problem is that the most specific override cannot be inferred from the compiler. We solve this problem, by providing an implementation for the method “Method” in class D as shown in the below code, and now the compiler uses the class implementation to solve the diamond problem.

```csharp using System;
namespace DefaultInterfaceMethodsDemo
{
    interface A
    {
        void Method();
    }
    interface B : A
    {
        void A.Method()
        {
            System.Console.WriteLine("I am From Interface B");
        }
    }
    interface C : A
    {
        void A.Method()
        {
            System.Console.WriteLine("I am From Interface C");
        }
    }
    class D : B, C
    {
        // Now the compiler uses the most specific override, which is defined in the class D.
        void A.Method()
        {
            System.Console.WriteLine("I am from class D");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            C c = new D();
            c.Method();
            Console.ReadKey();
        }
    }
}
```

##### Before C# 8, it was mandatory to implement all the methods of an interface in a class unless that class is declared as an abstract class, and this might make your code DRY. (Dont Repeat Yourself software engineer principle). Look a this now:

```csharp
using System;
namespace DefaultInterfaceMethodsDemo
{
    //Creating an Enum to Store Log Message Type
    enum LogLevel
    {
        Information,
        Warning,
        Error
    }

    //Creating the ILogger Interface
    interface ILogger
    {
        //The following Method is Abstract by Default
        void WriteMeesage(LogLevel level, string message);

        //Interface Method with Default Implementation
        void WriteInformation(string message)
        {
            //Calling the WriteMeesage to Log the actual Information message
            //WriteMeesage Method is going to be executed from the Implementor Class
            WriteMeesage(LogLevel.Information, message);
        }

        //Interface Method with Default Implementation
        void WriteWarning(string message)
        {
            //Calling the WriteMeesage to Log the actual Warning message
            //WriteMeesage Method is going to be executed from the Implementor Class
            WriteMeesage(LogLevel.Warning, message);
        }

        //Interface Method with Default Implementation
        void WriteError(string message)
        {
            //Calling the WriteMeesage to Log the actual Error message
            //WriteMeesage Method is going to be executed from the Implementor Class
            WriteMeesage(LogLevel.Error, message);
        }
    }

    //Implementor Class 1
    //ConsoleLogger Class Inheriting from ILogger Interface
    class ConsoleLogger : ILogger
    {
        //Provide Implementation for the WriteMeesage Abstract Method of ILogger Interface
        public void WriteMeesage(LogLevel level, string message)
        {
            Console.WriteLine($"{level}: {message}");
        }
    }

    //Implementor Class 2
    //TraceLogger Class Inheriting from ILogger Interface
    class TraceLogger : ILogger
    {
        //Provide Implementation for the WriteMeesage Abstract Method of ILogger Interface
        public void WriteMeesage(LogLevel level, string message)
        {
            Console.WriteLine($"{level}: {message}");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            //Create an Instance of ConsoleLogger Class and
            //Store the instance in ILogger Parent Reference Variable
            ILogger consoleLogger = new ConsoleLogger();

            //Call to the WriteWarning Method will be Executed from the ILogger Interface
            //Internally WriteWarning Method will call the WriteMeesage Method of ConsoleLogger Class
            consoleLogger.WriteWarning("Console Logger Warning Message");

            //Call to the WriteInformation Method will be Executed from the ILogger Interface
            //Internally WriteInformation Method will call the WriteMeesage Method of ConsoleLogger Class
            consoleLogger.WriteInformation("Console Logger Information Message");

            //Create an Instance of TraceLogger Class and
            //Store the instance in ILogger Parent Reference Variable
            ILogger traceLogger = new TraceLogger();

	            //Call to the WriteWarning Method will be Executed from the ILogger Interface
            //Internally WriteWarning Method will call the WriteMeesage Method of TraceLogger Class
            traceLogger.WriteWarning("Trace Logger Warning Message");

            //Call to the WriteInformation Method will be Executed from the ILogger Interface
            //Internally WriteInformation Method will call the WriteMeesage Method of TraceLogger Class
            traceLogger.WriteInformation("Trace Logger Information Message");

            Console.ReadKey();
        }
    }
}
```

This language feature enables API authors to add methods to an interface in later versions without breaking source or binary compatibility with existing implementations of that interface. Existing implementations inherit the default implementation.

# 3. **Pattern Matching Enhancements**

Consider these new pattern matching features when your data and functionality are separate. 
Consider pattern matching when your algorithms depend on a fact other than the runtime type of an object.

Pattern matching is a flexible and powerful way of testing data against a series of conditions and performing some computations based on the condition met.

In addition to new patterns in new places, C# 8.0 adds recursive patterns. Recursive patterns are patterns that can contain other patterns. In C# 8, the .Net Development team has introduced the following Patterns.

## 1. **Switch Expressions**
```csharp
// New way to write switch statements. Way more concise and readable.
using System;
namespace Csharp8Features
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Enter Day");
            string day = Console.ReadLine();
			//day.ToUpper() is the value that we are going to match with a pattern.
            var message = day.ToUpper() switch
	        {  // => is called the goes to operator
                "SUNDAY" => "Weekend",
                "MONDAY" => "start of the weekday",
                "FRIDAY" => "end of the weekday",
                _=> "Invalid day" // the underscore represents the default block
            };

            Console.WriteLine($"{day} is {message}");
        }
    }
}
```
## 2. **Property Patterns**

property pattern tests whether an expression’s properties/fields match the values of specified properties/fields. Each corresponding property or field must match and the expression must not be null.

Old School Way:

```csharp
using System;
namespace Csharp8Features
{
    class Program
    {
        static void Main(string[] args)
        {
            Rectangle rectangle = new Rectangle { Length = 5, Height = 10 };
            IsSpecificRectanglee(rectangle);
        }

        public static void IsSpecificRectanglee(Rectangle rectangle)
        {
            //Older Version
            //if (rectangle.GetType() == typeof(Rectangle))
            //{
            //    var rect = (Rectangle)rectangle;
            //    if (rect.Length == 5 && rect.Height == 10)
            //    {
            //        Console.WriteLine($"Rectangle Length: {rectangle.Length} and Height: {rectangle.Height}");
            //    }
            //}

            //Older Version
            var rect = rectangle as Rectangle;
            if (rect != null)
            {
                if (rect.Length == 5 && rect.Height == 10)
                {
                    Console.WriteLine($"Rectangle Length: {rectangle.Length} and Height: {rectangle.Height}");
                }
            }
        }
    }
    public class Rectangle
    {
        public double Length { get; set; }
        public double Height { get; set; }
    }
}
```

New School Way:

```csharp
using System;
namespace Csharp8Features
{
    class Program
    {
        static void Main(string[] args)
        {
            Rectangle rectangle = new Rectangle {Length = 5, Height = 10};
            IsSpecificRectanglee(rectangle);
        }

        public static void IsSpecificRectanglee(Rectangle rectangle)
        {
            if (rectangle is Rectangle { Length: 5, Height: 10 } specificzRectangle)
            {
                Console.WriteLine($"Rectangle Length: {specificRectangle.Length} and Height: {specificRectangle.Height}");
            }
        }
    }
    public class Rectangle
    {
        public double Length { get; set; }
        public double Height { get; set; }
    }
}
```


## 3. **Tuple Patterns**

The Tuple Pattern in C# can be used to pattern match multiple input values. We can write the complex logic very clean using the Tuple Pattern. When we have multiple inputs and we need to match by the combination of inputs then Tuple Pattern is useful.

```csharp
using System;
namespace Csharp8Features
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(SportsLike("Cricket", "Football", "Swimming"));
            Console.WriteLine(SportsLike("Cricket", "Football", "Racing"));
        }

        static string SportsLike(string sport1, string sport2, string sport3)
        {
            var result = (sport1, sport2, sport3) switch
            {
                //matches if Cricket, Football, and Swimming given as input
                ("Cricket", "Football", "Swimming") => "I like Cricket, Football, and Swimming.",
                //matches if Cricket, Football, and Baseball given as input
                ("Cricket", "Football", "Baseball") => "I like Cricket, Football, and Baseball.",
                //matches if Hockey, Football, and Swimming given as input
                ("Hockey", "Football", "Swimming") => "I like Hockey, Football, and Swimming.",
                //matches if Table Tennis, Football, and Swimming given as input
                ("Table Tennis", "Football", "Swimming") => "I like Table Tennis, Football and Swimming.",
                //Default case
                (_, _, _) => "Invalid input!"
            };
            return result;
        }
    }
}
```

## 4. **Positional Patterns**
Positional pattern matching works with the help of deconstructors introduced in C# 7. Deconstructor allows setting its properties into discrete variables and allows the use of positional patterns in a compact way without having to name properties.

Deconstruction is a process of unpacking types into parts and storing them into new variables (object deconstruction). For example, a class can be split into its properties or a tuple can be split into its individual items. The Positional Pattern in C# 8 can deconstruct an input expression and then test if the resulting variables match against a pattern specified in parentheses**.

```csharp
using System;
namespace Csharp8Features
{
    class Program
    {
        static void Main(string[] args)
        {
            Rectangle rectangle = new Rectangle { Length = 20, Height = 40 };
            // Deconstruction of Rectangle Obj
            var (length, height) = rectangle;
            Console.WriteLine($"The rectangle Length: {length} and Height: {height}");
            //the underscore here is the discard patttern
            if (rectangle is (20, _) rect)
            {
                Console.WriteLine("The rectangle has a length of 20");
            }
        }
    }

    public class Rectangle
    {
        public double Length { get; set; }
        public double Height { get; set; }

		//This is a deconstructor
        public void Deconstruct(out double length, out double height)
        {
            length = Length;
            height = Height;
        }
    }
}
```

 the rectangle type is deconstructed and length is tested against the 20 patterns (a constant pattern) and height is tested against _ (the discard pattern). The pattern reflects the position each variable has within the Deconstruct method. So, the first value coming out of the Deconstruct method is the first value we are going to match on and so forth. Discards (_) can be used where any value is accepted at that position. So, the pattern will match if the rectangle has a length of 20 and a height of any value.


This feature is inspired by functional programming and uses a de-constructor. The deconstructor should be a public method named Deconstructor. It has out parameters for values that need to deconstruct.
# 4. **Using Declarations**

In C#, as a programmer, we use the using statement for declaring disposable variables (that means creating an instance of a class that implements the IDisposable interface) such as File I/O, Databases, Web Services, etc. The using declaration will ensure that the classes that implement the IDisposable interface will call their Dispose method to dispose of the object.
Same thing goes for `DisposeAsync()` and `IDisposableAsync`

C# 8 allow for using statements without introducing a new scope block. Also guarantees the calling of Dispose() even if the an exception is thrown.

the using statement looks like this under the hood:

```csharp
//with using block
using(var resource = new Resource())
{
	//using block body
	resource.ResourceUsing();
}

//under the hood
var resource - new Resource();
try
{
	//using block body
	resource.ResourceUsing();
}
finally
{
	resource.Dispose();
}
```

## Old School

Implementing the IDisposable in a user defined class and using a using block:
```csharp
using System;
namespace UsingDeclarationsDemo
{
    public class Program
    {
        public static void Main()
        {
            //Creating an Instance of Resource Class using the using declaration
            using (var resource = new Resource())
            {
                //Using the resource as part of the using block
                resource.ResourceUsing();
            } // resource.Dispose Method is called here automatically
            Console.WriteLine("Doing Some Other Task...");
            Console.ReadKey();
        }
    }

    //Creating a Class Inheriting from IDisposable Interface
    class Resource : IDisposable
    {
        public Resource()
        {
            Console.WriteLine("Resource Created...");
        }
        public void ResourceUsing()
        {
            Console.WriteLine("Resource Using...");
        }

        //Providing Implementation for Dispose of IDisposable Interface
        public void Dispose()
        {
            //Dispose resource
            //Performs application-defined tasks associated with freeing, releasing, or resetting
            //unmanaged resources
            Console.WriteLine("Resource Disposed...");
        }
    }
}
```

### Disposing Multiple Resources

```csharp

using (var whatever = new Resource())
{
	using (var whatever2 = new Resource())
	{
		whatever.Method();
		whatever2.Method2();
	}
}
```

## New School

Now we have Using Declarations. The curly brackets are no longer required: 

```csharp
using System;
namespace UsingDeclarationsDemo
{
    public class Program
    {
        public static void Main()
        {
            //Creating an Instance of Resource Class with the new using declaration
            using var resource = new Resource();

            //Using the resource 
            resource.ResourceUsing();

            Console.WriteLine("Doing Some Other Task...");
        }//Here, the resource.Dispose() Method is called automatically
    }

    //Creating a Class Inheriting from IDisposable Interface
    class Resource : IDisposable
    {
        public Resource()
        {
            Console.WriteLine("Resource Created...");
        }
        public void ResourceUsing()
        {
            Console.WriteLine("Resource Using...");
        }

        //Providing Implementation for Dispose of IDisposable Interface
        public void Dispose()
        {
            //Dispose resource
            //Performs application-defined tasks associated with freeing, releasing, or resetting
            //unmanaged resources
            Console.WriteLine("Resource Disposed...");
        }
    }
}
```
At the end of the scope of the method (which is here at the end of the Main method because there is no new scope block), the Dispose method is also going to be called automatically. 

Here also, the compiler creates a try/finally block enwrapping the Main method to make sure Dispose method is called.

### Disposing Multiple Resources

```csharp
public static void Main()
{
using var resource1 = new Resource();
using var resource2 = new Resource();
resource1.ResourceUsing();
resource2.ResourceUsing();
Console.WriteLine("Main Method End...");
}//Here, resource1.Dispose() and resource2.Dispose() Method is called automatically
```


### Dispose() Before Method Ends with Using Declarations

You can always call Dispose() yourself.

but you can create a code block using only curly brackets which generates a new scope and make a using declaration there:

```csharp
using System;
namespace UsingDeclarationsDemo
{
    public class Program
    {
        public static void Main()
        {
            using var resource1 = new Resource();
            resource1.ResourceUsing();

            //Creating a Block using only curly braces
            {
                using var resource2 = new Resource();
                resource2.ResourceUsing();
            }//Here, resource2.Dispose() Method is called automatically

            Console.WriteLine("Main Method End...");
        }//Here, resource1.Dispose() Method is called automatically
    }

    //Creating a Class Inheriting from IDisposable Interface
    class Resource : IDisposable
    {
        public Resource()
        {
            Console.WriteLine("Resource Created...");
        }
        public void ResourceUsing()
        {
            Console.WriteLine("Resource Using...");
        }

        //Providing Implementation for Dispose of IDisposable Interface
        public void Dispose()
        {
            //Dispose resource
            //Performs application-defined tasks associated with freeing, releasing, or resetting
            //unmanaged resources
            Console.WriteLine("Resource Disposed...");
        }
    }
}
```
# 5. **Static local functions**

a local function is a private function of a function whose scope is limited to the function where it is created. You can only call the local function from the parent function where it is created.

```csharp
using System;
namespace Csharp8Features
{
    public class LocalFunctions
    {
        public static void Main()
        {
            Calculate();
            //You cannot call the CalculateSum function
            //CalculateSum();
        }
        public static void Calculate()
        {
            int X = 20, Y = 30, Result;
            CalculateSum(X, Y);

            // Here CalculateSum is the local function of the Calculate function
            void CalculateSum(int Num1, int Num2)
            {
                Result = Num1 + Num2;
                Console.WriteLine($"Num1 = {Num1}, Num2 = {Num2}, Result = {Result}");
            }

            // Calling Local function
            CalculateSum(30, 10);
            CalculateSum(80, 60);
        }
    }
}
```

The `CalculateSum` function is able to access the Result variable. This enables the usage of variables such as Result within the local method. If the usage is accidental, this might lead to huge consequences. To overcome this problem static local functions are introduced in C# 8.

This ensures that the static local function does not reference any variable from the enclosing or surrounding scope.

This enables the developers to create a pure local function as it does not allow the usage of variables from enclosing types within it.

If the algorithm above was implemented with a static local function:
```csharp
using System;
namespace Csharp8Features
{
    public class LocalFunctions
    {
        public static void Main()
        {
            Calculate();
            //You cannot call the CalculateSum function
            //CalculateSum();
        }
        public static void Calculate()
        {
            int X = 20, Y = 30;
            CalculateSum(X, Y);

            // Here CalculateSum is the local function of the Calculate function
            static void CalculateSum(int Num1, int Num2)
            {
                int sum = Num1 + Num2;
                Console.WriteLine($"Num1 = {Num1}, Num2 = {Num2}, Result = {sum}");
            }

            // Calling Static Local function
            CalculateSum(30, 10);
            CalculateSum(80, 60);
        }
    }
}
```

Characteristics:

1. Local functions make the code more readable and prevent function calls by mistake, as a local function may only be called inside its outer function.
2. A static local function supports the async and unsafe modifiers.
3. C# 8.0 allows us to define multiple static local functions in the body of one function.
# 6. **Disposable ref structs**

Resume: ref structs are memory allocated to the stack and not to the heap but to do that they cannot implement any interface other than `IDisposable<T>` to avoid boxing (turning a value type to a reference type). Structures are always allocated in the stack for being value data types, but regular structures allow a structure instance to be allocated to the heap (like when an object has as struct instance for one of it's data members for example.). Ref structs enforce that all instances of it will be allocated to the stack no matter what.

From C# 7.2 onward, a struct can be declared with the ref keyword. This allows the instances of ref structs to be allocated on the stack and restricts them from moving onto the managed heap. However, this also enforces some limitations over the ref structs, the ref structs cannot implement any interface since it exposes them to boxing possibilities.

In C# 8.0, a special exception to the above limitation was made for the IDisposable interface. Ref structs can now be disposable without implementing the IDisposable interface, simply by having a Dispose method in them.

this throws an error CS8343 ref structs cannot implement interfaces
```csharp
using System;
namespace Csharp8Features
{
    public class DisposableRefStructs
    {
        public static void Main()
        {
            using (var rectangle = new Rectangle())
            {
                //Do Something
            }
        }
    }

    //Ref struct Cannot Implement the IDisposable Interface
    //hence cannot provide Implementation for Dispose method
    ref struct Rectangle : IDisposable
    {
        public void Dispose()
        {
        }
    }
}
```

This will implement IDisposable in the ref struct without throwing an error:
```csharp
using System;
namespace Csharp8Features
{
    public class DisposableRefStructs
    {
        public static void Main()
        {
            using var rectangle1 = new Rectangle();

            using (var rectangle2 = new Rectangle())
            {
                //Do Something
            }
        }
    }

    //Ref struct Cannot Implement the IDisposable Interface
    //hence cannot provide Implementation for Dispose method
    ref struct Rectangle 
    {
        public void Dispose() { }
    }
}
```
# 8. **Asynchronous streams**

Async Streams is the new feature in C# 8.0 which provides async support for handling streams or IEnumerable data.

`IAsyncEnumerable<T>`  has options and support for CancellationToken as well as ConfigureAwait.

Traits of an async stream:

1. It’s declared with the async modifier.
2. It returns an `IAsyncEnumerable<T>`.
3. The method contains yield return statements to return successive elements in the asynchronous stream.

Consuming an asynchronous stream requires you to add the await keyword before the foreach keyword when you enumerate the elements of the stream. Adding the await keyword requires the method that enumerates the asynchronous stream to be declared with the async modifier and to return a type allowed for an async method. Typically, that means returning a Task or `Task<TResult>`. It can also be a ValueTask or `ValueTask<TResult>`. A method can both consume and produce an asynchronous stream, which means it would return an `IAsyncEnumerable<T>`.

 The following code generates a sequence from 0 to 19, waiting 100 ms between generating each number.

```csharp
using System;
using System.Threading.Tasks;

namespace Csharp8Features
{
    public class NullableReferenceTypes
    {
        static async Task Main(string[] args)
        {
            await foreach (var number in GenerateSequence())
            {
                Console.WriteLine(number);
            }
        }
        public static async System.Collections.Generic.IAsyncEnumerable<int> GenerateSequence()
        {
            for (int i = 0; i < 20; i++)
            {
                await Task.Delay(100);
                yield return i;
            }
        }
    }
}
```

The yield method also contains the yield return statements to return successive elements in the asynchronous stream.

the async and await operators will  fetch in-memory first and then all those records will return as a combined list or combined collection to the invoker.

Using `IAsyncEnumerable` as soon as one item is fetched he will be processed and returned immediately without waiting for the other items. This can drastically improve the application performance and overall user experience in many cases.

Think about how long this would take for example:

```csharp
using System;
using System.Collections.Generic;
using System.Threading;

namespace Csharp8Features
{
    public class AsynchronousStreams
    {
        static void Main(string[] args)
        {
            Console.WriteLine($"Start: {DateTime.Now.ToLongTimeString()}");
            var numbers = GetNumbers(1, 10);
            Console.WriteLine($"End: {DateTime.Now.ToLongTimeString()}");
            Console.ReadKey();
        }
        public static IEnumerable<int> GetNumbers(int start, int end)
        {
            var random = new Random();
            var returnList = new List<int>();

            for (int i = start; i < end; i++)
            {
                returnList.Add(i);
                Thread.Sleep(millisecondsTimeout:random.Next(500, 1500));
            }

            return returnList;
        }
    }
}
```
This takes 10 second to complete.

If we do it with Async Streams it takes 8 seconds to complete:

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Csharp8Features
{
    public class AsynchronousStreams
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine($"Start: {DateTime.Now.ToLongTimeString()}");
            var numbersAsync = GetNumbersAsync(1, 10);
            await foreach (var number in numbersAsync)
            {
                Console.WriteLine(number);
            }
            Console.WriteLine($"End: {DateTime.Now.ToLongTimeString()}");
            Console.ReadKey();
        }

        public static async IAsyncEnumerable<int> GetNumbersAsync(int start, int end)
        {
            var random = new Random();
           
            for (int i = start; i < end; i++)
            {
                await Task.Delay(random.Next(500, 1500));
                yield return i;
            }
        }
    }
}
```

Keep in mind Delay is random. The point is that Async Stream will always be faster the non Async Streams.

## Cancellation Tokens With Async Streams

```csharp
using System;
using System.Collections.Generic;
using System.Runtime.CompilerServices;
using System.Threading;
using System.Threading.Tasks;

namespace Csharp8Features
{
    public class AsynchronousStreams
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine($"Start: {DateTime.Now.ToLongTimeString()}");
            var cancellationTokenSource = new CancellationTokenSource();
            cancellationTokenSource.CancelAfter(millisecondsDelay:3000);
            var numbersAsync = GetNumbersAsync(1, 10, cancellationTokenSource.Token);
            await foreach (var number in numbersAsync)
            {
                Console.WriteLine(number);
            }
            Console.WriteLine($"End: {DateTime.Now.ToLongTimeString()}");
            Console.ReadKey();
        }

        public static async IAsyncEnumerable<int> GetNumbersAsync(int start, int end, [EnumeratorCancellation]CancellationToken token = default)
        {
            var random = new Random();
           
            for (int i = start; i < end; i++)
            {
                if (token.IsCancellationRequested)
                {
                    Console.WriteLine("Cancellation has been requested...");
                    // Perform cleanup if necessary.
                    //...
                    // Terminate the operation.
                    break;
                }
                await Task.Delay(random.Next(500, 1500));
                yield return i;
            }
        }
    }
}
```

## `ConfigureAwait `Method


The `ConfigureAwait` method Configures how awaits on the tasks returned from an async iteration are performed. true to capture and marshal back to the current context; otherwise, false.

Do you remember that a thread is a small part of a process but with it's own execution context? that is the context here in this article of the obsidian note.

It indicates that the continuation after the asynchronous operation should not necessarily run on the original context (like the UI context in a UI application). Instead, it can run on a different context, typically a thread from the thread pool. This can be useful to avoid potential deadlocks or improve performance in certain scenarios.

**When `ConfigureAwait()` is not specified (or `ConfigureAwait(true)` is used), the continuation will attempt to run on the original context, which can be important in UI applications to update the user interface from the correct thread.**

```csharp
using System;
using System.Collections.Generic;
using System.Runtime.CompilerServices;
using System.Threading;
using System.Threading.Tasks;

namespace Csharp8Features
{
    public class AsynchronousStreams
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine($"Start: {DateTime.Now.ToLongTimeString()}");
            var cancellationTokenSource = new CancellationTokenSource();
            cancellationTokenSource.CancelAfter(millisecondsDelay:3000);
            var numbersAsync = GetNumbersAsync(1, 10, cancellationTokenSource.Token).ConfigureAwait(false);
            await foreach (var number in numbersAsync)
            {
                Console.WriteLine(number);
            }
            Console.WriteLine($"End: {DateTime.Now.ToLongTimeString()}");
            Console.ReadKey();
        }

        public static async IAsyncEnumerable<int> GetNumbersAsync(int start, int end, [EnumeratorCancellation]CancellationToken token = default)
        {
            var random = new Random();
           
            for (int i = start; i < end; i++)
            {
                if (token.IsCancellationRequested)
                {
                    Console.WriteLine("Cancellation has been requested...");
                    // Perform cleanup if necessary.
                    //...
                    // Terminate the operation.
                    break;
                }
                await Task.Delay(random.Next(500, 1500));
                yield return i;
            }
        }
    }
}
```

1. If you are writing code for the UI, use ConfigureAwait(true).
2. If you are writing code in a library that will be shared, use ConfigureAwait(false)
# 9. **Asynchronous disposable**

The `IAsyncDisposable` interface was introduced as part of C# 8.0. We need to implement the `DisposeAsync()`.

`DisposeAsync()` method allows for asynchronous clean-up operations whereas the Dispose() method performs synchronous clean-up operations. It also returns a `ValueTask` that represents the asynchronous dispose of operation.

The point that you need to keep in mind is that when we are implementing the `IAsyncDisposable` (for asynchronous clean-up) interface and then we also need to make sure that the class will also implement the `IDisposable` (for synchronous clean-up) interface. The reason is that a good implementation pattern of the `IAsyncDisposable` interface is to be prepared for both synchronous and asynchronous dispose.

The public parameterless `DisposeAsync()` method is called automatically in an await using statement, and the purpose of this `DisposeAsync()` method is to free the unmanaged resources. Freeing the memory associated with a managed object is always the responsibility of the garbage collector:

```csharp
public async ValueTask DisposeAsync()
{
    // Perform async clean-up.
    await DisposeAsyncCore();

    // Dispose of unmanaged resources.
    Dispose(false);

    // Dispose methods should call SuppressFinalize
    // Suppress finalization.
    GC.SuppressFinalize(this);
}
```

The difference between the Async Dispose Pattern and Dispose Pattern is that the call from `DisposeAsync()` to the Dispose(bool) overload method is given false as an argument. When implementing the Dispose() method, however, true is passed instead. In other words, the `DisposeAsyncCore()` method will dispose of managed resources asynchronously, so you don’t want to dispose of them synchronously as well. Therefore, call Dispose(false) instead of Dispose(true).

## **The DisposeAsyncCore() Method**

The DisposeAsyncCore() method is intended to perform the asynchronous clean-up of managed resources. It encapsulates the common asynchronous clean-up operations when a subclass inherits a base class that is an implementation of IAsyncDisposable. The DisposeAsyncCore() method is virtual so that derived classes can define additional clean-up by overriding this method. If an implementation of IAsyncDisposable is sealed, the DisposeAsyncCore() method is not needed, and the asynchronous clean-up can be performed directly in the DisposeAsync() method.

Any non-sealed class should have an additional DisposeAsyncCore() method that should return a ValueTask. So, the class should have a public IAsyncDisposable.DisposeAsync() implementation that has no parameters as well as a protected virtual ValueTask DisposeAsyncCore() method with the following:

**protected virtual ValueTask DisposeAsyncCore()**  
**{**  
**}**

If we want to implement the Asynchronous Disposable pattern for any non-sealed class, then we must provide the protected virtual DisposeAsyncCore() method.

```csharp
public class Sample : IAsyncDisposable
{
    public async ValueTask DisposeAsync()
    {
        Console.WriteLine("Delaying!");
        await Task.Delay(1000);
        Console.WriteLine("Disposed!");
    }

    protected virtual async ValueTask DisposeAsyncCore()
    {
        Console.WriteLine("DisposeAsyncCore Delaying!");
        await Task.Delay(1000);
        Console.WriteLine("DisposeAsyncCore Disposed!");
    }
}
```

with using keyword:

```csharp
using System;
using System.Threading.Tasks;
using System.IO;
namespace Csharp8Features
{
    class AsynchronousDisposable
    {
        static async Task Main(string[] args)
        {
            await using (var disposableObject = new SampleInherited())
            {
                Console.WriteLine("Welcome to C#.NET");
            }// DisposeAsync method called implicitly

            Console.WriteLine("Main Method End");
        }
    }

    public class Sample : IAsyncDisposable
    {
        static readonly string filePath = @"D:\MyTextFile1.txt";
        private TextWriter? textWriter = File.CreateText(filePath);

        public async ValueTask DisposeAsync()
        {
            await DisposeAsyncCore().ConfigureAwait(false);
            Console.WriteLine("DisposeAsync Clean-up the Memory!");
        }

        protected virtual async ValueTask DisposeAsyncCore()
        {
            if (textWriter != null)
            {
                await textWriter.DisposeAsync().ConfigureAwait(false);
            }

            textWriter = null;
            Console.WriteLine("Virtual DisposeAsyncCore Clean-up the Memory");
        }
    }

    public class SampleInherited : Sample
    {
        protected override async ValueTask DisposeAsyncCore()
        {
            await base.DisposeAsyncCore();

            Console.WriteLine("Subclass DisposeAsyncCore Clean-up the Memory");
        }
    }
}
```



## **Implementing both Dispose and Async Dispose Patterns in C#:**

You may need to implement both the IDisposable and IAsyncDisposable interfaces, especially when your class scope contains instances of these implementations. Doing so ensures that you can properly cascade clean-up calls.

 a lot of older Inversion of Control frameworks are not capable of handling asynchronous disposals yet. So you might need to implement both for that reason.
 
```csharp
using System;
using System.Threading.Tasks;
using System.IO;
namespace Csharp8Features
{
    class AsynchronousDisposable
    {
        static async Task Main(string[] args)
        {
            await using (var disposableObject = new Sample())
            {
                Console.WriteLine("Welcome to C#.NET");
            }// DisposeAsync method called implicitly

            Console.WriteLine("Main Method End");
        }
    }

    public class Sample : IDisposable, IAsyncDisposable
    {
        private Stream? disposableResource = new MemoryStream();
        private Stream? asyncDisposableResource = new MemoryStream();

        public void Dispose()
        {
            GC.SuppressFinalize(this);
            Console.WriteLine("Dispose Clean-up the Memory!");
        }

        public async ValueTask DisposeAsync()
        {
            await DisposeAsyncCore().ConfigureAwait(false);
            Dispose();
            GC.SuppressFinalize(this);
            Console.WriteLine("DisposeAsync Clean-up the Memory!");
        }

        protected virtual async ValueTask DisposeAsyncCore()
        {
            if (asyncDisposableResource != null)
            {
                await asyncDisposableResource.DisposeAsync().ConfigureAwait(false);
            }

            if (disposableResource is IAsyncDisposable disposable)
            {
                await disposable.DisposeAsync().ConfigureAwait(false);
            }
            else
            {
                disposableResource?.Dispose();
            }

            asyncDisposableResource = null;
            disposableResource = null;

            Console.WriteLine("Virtual DisposeAsyncCore Clean-up the Memory");
        }
    }
}
```

If you go to the definition of Stream class, then you will see that it implements both IDisposable and IAsyncDisposable interfaces
# 10. **Indices and ranges**

Use of index and ranges regularly:

```csharp
using System;
using System.Xml.Linq;

namespace Csharp8Features
{
    class IndicesAndRanges
    {
        static void Main(string[] args)
        {
            //Creating a Collection to store list of Countries
            List<string> countries = new List<string>()
            {
                "INDIA",
                "USA",
                "UK",
                "NZ",
                "CANADA",
                "CHINA",
                "NEPAL",
                "RUSIA",
                "SRILANKA",
                "INDONESIA"
            };

            //Accessing Element Based on the Index Position
            Console.WriteLine($"Element at Index Position 1 is {countries[1]}");
            Console.WriteLine($"Element at Index Position 3 is {countries[3]}");

            //Accessing Range of Elements using GetRange Method 
            Console.WriteLine("Accessing Range of Elements using Range");
            List<string> countryRange = countries.GetRange(1, 3);

            //Printing the Elements which are accessed by the GetRange Method
            foreach (var item in countryRange)
            {
                Console.Write(item + " ");
            }
            Console.ReadKey();
        }
    }
}
```

## New Stuff

###### **Two New Types:**

1. **System.Range:** It represents a sub-range of the given sequence or collection.
2. **System.Index:** It represents an index in the given sequence or collection.

**Two New Operators:**
**^ Operator:**
	**Old Method:** **`var LastValue = myArray[myArray.Length-1]**  `
	**New Method:** **`var LastValue = myArray[^1]`**
**.. Operator**:
**Old Method:** **`var arr = myArray.GetRange(1, 5);`** **//This will return five elements from the index position 1**  
**New Method:** **`var arr = myArray[1..5];`**

When the lower bound is omitted, it is interpreted to be zero, and when the upper bound is omitted, it is interpreted to be the length of the receiving collection.

We can also declare ranges as variables in C#. The following is the syntax:  
**Range phrase = 1..5;**  
The range. then can be used inside the [] characters as follows:  
**var subCountry= countries[phrase];**

Not only do arrays support indices and ranges but you can also use indices and ranges with string, `Span<T>`, or `ReadOnlySpan<T>`. That's because all of these types implement an array as it's underlying data structure

Ranges in C# allow the creation of substrings by using the indexer.
`var helloWorld = "Hello World; var hello = helloWorldStr[..5]`
# 11. **Null-coalescing Assignment Operator**

this is the operator: ??=

We can use this **??=** operator to assign the value of its right-hand operand to its left-hand operand only if the left-hand operand evaluates to null. That means the null-coalescing assignment operator ??= assigns a variable only if it is null.

I can remove many redundant if-else statements and make our code more readable and understandable.

internally it looks like this:
```csharp
if (variable == null)
{
	variable = expression;
}
```

1. The left-hand operand of the ??= operator must be a variable, a property, or an indexer element.
2. It is right-associative.
3. You cannot overload ??= operator.
4. You are allowed to use the ??= operator with reference types and value types. Only types that can hold a null value.

# Only Relevant Changes if You Are Going to Play Around With Memory Manually
## 12. **Unmanaged constructed types**

A type is called **unmanaged type** when it is being used in an unsafe context. This is true for many built-in basic types. A type is an unmanaged type, if it is any of the following types:

1. sbyte, byte, short, ushort, int, uint, long, ulong, char, float, double, decimal, or bool. Resume: (all numeric types + bool)
2. Any enum type
3. Any pointer types
4. Any user-defined struct type that contains fields of unmanaged types only.
5. To resume the 4 points: All the pure value types that you can think of, either Primitive or Complex.


A type is called constructed type if it is generic and the type parameter is already defined. For example, *`*List<string>`** is a constructed type while **`List<T>`** is not. This is because the **string** is already defined as a data type whereas **T** is not already defined as a data type. In C# 7.3 and earlier, a Constructed Type can’t be an Unmanaged Type. Starting with C# 8.0, a constructed value type is unmanaged if it contains fields of unmanaged types only.

Beginning with C# 7.3, you can use the unmanaged constraint to specify that a type parameter is a non-pointer, non-nullable unmanaged type. Beginning with C# 8.0, a constructed struct type that contains fields of unmanaged types only is also unmanaged:

```csharp
using System;
namespace Csharp8Features
{
    public struct Coords<T>
    {
        public T X;
        public T Y;
    }

    public class UnmanagedTypes
    {
        public static void Main()
        {
            DisplaySize<Coords<int>>();
            DisplaySize<Coords<double>>();
        }

        private unsafe static void DisplaySize<T>() where T : unmanaged
        {
            Console.WriteLine($"{typeof(T)} is unmanaged and its size is {sizeof(T)} bytes");
        }
    }
}
```

A generic struct may be the source of both unmanaged and managed constructed types. The preceding example defines a generic struct `Coords<T>` and presents examples of unmanaged constructed types. An example of a managed type is `Coords<object>`. It’s managed because it has the fields of the object type, which is managed. If you want all constructed types to be unmanaged types, use the unmanaged constraint in the definition of a generic struct as shown below.

```csharp
public struct Coords<T> where T : unmanaged
{
       public T X;
       public T Y;
}
```

##### **Unmanaged Constructed Types:**

Let’s consider the following example of an unmanaged constructed type that it was not possible to declare before C# 8.0.

```csharp
public struct Foo<T>
{
    public T var1;
    public T var2;
    public T var3;
}
```

For any unmanaged type, you can create a pointer to a variable of this type or allocate a block of memory on the stack for instances of this type as shown below.

```csharp
Span<Foo<int>> bars = stackalloc[]
{
    new Foo<int> { var1 = 10, var2 = 20, var3 = 30 },
    new Foo<int> { var1 = 11, var2 = 21, var3 = 31 },
    new Foo<int> { var1 = 21, var2 = 22, var3 = 32 },
};
```

1. This feature is a performance enhancement.
2. Constructed value types are now unmanaged if it only contains fields of unmanaged types.
3. This feature means that you can do things like allocate instances on the stack
## 13. **Stackalloc in nested expressions**

short for Stack Allocator. Creates a block of memory on the stack and returns a pointer to the start of that memory address. Stack-allocated memory is automatically destroyed when the scope it was created in is exited. You cannot explicitly free the memory allocated with stackalloc.

**Note:** The stackalloc operator allocates memory in an unsafe context (so it should be used with caution). It is similar to the alloc function of our traditional C language. The stackalloc operator implements a form of malloc (Memory Allocation) that frees the memory when the calling function returns.

##### **stackalloc Before C# 7.2 :**

This example uses an unsafe method. So, in order to compile it, you must enable unsafe code. In this example, the stackalloc operator allocates 40 bytes of memory on the stack. Here, you can see, we have written the code using the unsafe block.

```csharp
using System;
namespace Csharp8Features
{
    using System;
    class Program
    {
        static void Main()
        {
            //Before C# 7.2
            unsafe
            {
                //Allocate Some Memory on the stack using stackalloc
                int* ptr = stackalloc int[10];
                for (int i = 0; i < 10; i++)
                {
                    ptr[i] = i + 1;
                }
                for (int i = 0; i < 10; i++)
                {
                    Console.Write($"{ptr[i]} ");
                }
            } 
            Console.ReadKey();
        }
    }
}
```

To compile this program, we need to enable unsafe mode using the project properties window. So, first, open the Project Properties window and then go to the build tab and checked the unsafe check box as shown in the below image and save it.

From C# 7.2, you can assign the result of a stackalloc expression to either **`System.Span<T>`** or **`System.ReadOnlySpan<T>`** without using an unsafe context. No reason for the unsafe block anymore:

```csharp
using System;
namespace Csharp8Features
{
    using System;

    class Program
    {
        static void Main()
        {
            //C# 7.2
            //Allocate Some Memory on the stack using stackalloc
            //Int = 4 Bytes, so it will allocate 40 (10*4) Bytes of Memory in Stack
            //Using Span<int>, so unsafe block is not required
            Span<int> ptr = stackalloc int[10];
            for (int i = 0; i < 10; i++)
            {
                ptr[i] = i + 1;
            }
            for (int i = 0; i < 10; i++)
            {
                Console.Write($"{ptr[i]} ");
            }
            Console.ReadKey();
        }
    }
}
```

 From C# 7.3, stackalloc can be used for initializing arrays.
```csharp
using System;
namespace Csharp8Features
{
    using System;
    class Program
    {
        static void Main()
        {
            //C# 7.3
            //Initialzing Array without stackalloc
            var arr1 = new int[5] { 1, 4, 9, 16, 25 };
            for (int i = 0; i < 5; i++)
            {
                Console.Write($"{arr1[i]} ");
            }
            Console.WriteLine();
            var arr2 = new int[] { 1, 2, 4, 8 };
            for (int i = 0; i < 4; i++)
            {
                Console.Write($"{arr2[i]} ");
            }
            Console.WriteLine();
            //Initialzing Array with stackalloc
            unsafe
            {
                int* pArr1 = stackalloc int[5] { 1, 4, 9, 16, 25 };
                for (int i = 0; i < 5; i++)
                {
                    Console.Write($"{pArr1[i]} ");
                }
                Console.WriteLine();
                int* pArr2 = stackalloc int[] { 1, 2, 4, 8 };
                for (int i = 0; i < 4; i++)
                {
                    Console.Write($"{pArr2[i]} ");
                }
            }
            Console.WriteLine();
            //Initialzing Array with stackalloc and Span<T>
            //Here, unsafe block is not required
            Span<int> ptr1 = stackalloc int[5] { 1, 4, 9, 16, 25 };
            for (int i = 0; i < 5; i++)
            {
                Console.Write($"{ptr1[i]} ");
            }
            Console.WriteLine();
            Span<int> ptr2 = stackalloc int[] { 1, 2, 4, 8 };
            for (int i = 0; i < 4; i++)
            {
                Console.Write($"{ptr2[i]} ");
            }
            Console.ReadKey();
        }
    }
}
```

IMPORTANT: With C# 7.2, we started using `Span<T>`, `ReadOnlySpan<T>`, and `Memory<T>` because they are ref-struct instances that are guaranteed to be allocated on the stack, and therefore won’t affect the garbage collector.

##### **Understand stackalloc in C# 8:**

Starting with C# 8.0, if the result of a stackalloc expression is of the `**System.Span<T>`** or **`System.ReadOnlySpan<T>`** type, you can use the stackalloc expression in other expressions.

```csharp
using System;
using System.Dynamic;
using System.Reflection;

namespace Csharp8Features
{
    public class StackMemoryAllocation
    {
        public static void Main()
        {
            //Storing the result of stackalloc in Span<int>
            Span<int> numbers = stackalloc int[] { 10, 20, 30, 40, 50, 60, 70, 80, 80, 100 };

            //Now we can use stackalloc expression i.e. numbers in other expressions
            //IndexOfAny: Searches for the first index of any of the specified values.
            var index = numbers.IndexOfAny(stackalloc[] {11, 40, 60, 100 });

            Console.WriteLine(index); // output: 3  
        }
    }
}
```

```csharp
using System;
using System.Drawing;
using System.Reflection;

namespace Csharp8Features
{
    public class StackMemoryAllocation

    {
        public static void Main()
        {
            //Storing the result of stackalloc in Span<int> so that we can resue it in another expression
            Span<int> set = stackalloc int[6] { 1, 2, 3, 4, 5, 6 };

            //Reusing stackalloc expression  
            //Forms a slice out of the current span starting at a specified index for a specified length.
            Span<int> subSet = set.Slice(3, 2);

            foreach (var n in subSet)
            {
                Console.WriteLine(n); // Output: 4 5
            }
        }
    }
}
```

##### **When to use stackalloc in C#?**

The stackalloc should only be used for performance optimizations (either for computation or interop). This is due to the following facts:
1. The garbage collector is not required as the memory is allocated on the stack rather than the heap. The memory is released as soon as the variable goes out of scope.
2. It is faster to allocate memory on the stack rather than the heap.
# 14. **Enhancement of interpolated verbatim strings**

```csharp
using System;

class Program
{
    static void Main()
    {
        // Example of interpolated verbatim string
        string name = "John";
        int age = 30;

        string message = $@"Hello, my name is {name}.
I am {age} years old.";

        Console.WriteLine(message);
    }
}

```

In C# 8.0, you can use the `$` prefix along with `@` to create an interpolated verbatim string. This is useful when you want to include expressions and maintain a literal representation of newline characters.

In the example above, the string `message` is an interpolated verbatim string. The `$` before the `@` allows you to include expressions (like `{name}` and `{age}`) inside the string, and the `@` allows you to use a verbatim string with literal newline characters.

This feature can be particularly handy when dealing with long strings that include variables or expressions, and you want to maintain a readable and unescaped format.
# 15. Nullable Reference Types

C# 8.0 allows us to specify whether a variable should be null, and when it should not be null. Based on these annotations, the compiler will warn you when you are potentially using a null reference or passing a null reference to a function that will not accept it.

 The compiler uses flow analysis to ensure that any variable of a nullable reference type is checked against null before it’s accessed or assigned to a non-nullable reference type.

Before C# 8.

Without enabling nullable reference types this does not throw any compile errors:
```csharp
using System;
namespace NullableReferenceTypesDemo
{
    public class Program
    {
        public static void Main()
        {
            string message = null;

            //No warning
            Console.WriteLine($"The length of the message is {message.Length}");

            var originalMessage = message;
            message = "Hello, World!";

            // No warning 
            Console.WriteLine($"The length of the message is {message.Length}");

            // No warning
            Console.WriteLine(originalMessage.Length);
        }
    }
}
```

Starting with .NET 6, they’re enabled by default for new projects. If you are working with an older version of the .NET Core Application, then you need to enable Nullable annotation. In order to enable Nullable annotations in .NET Core Application, you need to edit the project **.csproj** file and add **<Nullable>enable</Nullable>** in the property group.

```xml
<Project Sdk="Microsoft.NET.Sdk>
	<PropertyGroup>
	  <Nullable>enable<Nullable>
	<PropertyGroup>
```

Now there will be a compile error.

Enabling Nullable reference types allow you to reduce the chances of running into a `NullReferenceException` (when you are trying to dereference a reference type that may be null).

A variable is either not-null or may be null. 

The compiler determines that a variable is **not-null** in two ways:

1. The variable has been assigned a value that is known to be not null.
2. The variable has been checked against null and hasn’t been modified since that check.

Any variable that the compiler hasn’t determined as **not null** is considered **may be null** and will generate a compile time error.

1. When a variable is not-null, that variable may be dereferenced safely.
2. When a variable is maybe-null, that variable must be checked to ensure that it isn’t null before dereferencing it.

you can put #nullable enable and #nullable disable directives in places where you want the nullability check.  You can use #nullable restore directive to restore to the default setting.

```csharp
using System;
namespace Csharp8Features
{
    class Person
    {
        // Warning Name is Non Null!
        public string Name { get; set; }

        // No Warning Name is null!
        public string? NullableName { get; set; }

        //Enable the below code then the warning above will be disappeared
        //public Person(string name)
        //{
        //    Name = name;
        //}
    }
}
```

The first property i.e. Name is a reference type, and it is null and for this reason the compiler warns you. The Second property i.e. NullableName is a nullable reference type and that’s why the compiler is not warning because the NullableName can be null, you have defined it as nullable.

##### **Benefits of Nullable Reference Types in C#**

The introduction of this feature from C# 8.0 allows for several benefits that are not present in earlier versions:

1. Allows the programmer to clearly show their intent when declaring variables.
2. Provides protection against Null Reference Exceptions.
3. The compiler warns you if you dereference a nullable reference when it may be null.

##### **Rules for Non-nullable Reference Type in C#**

When a variable is not supposed to be null, the compiler enforces some rules to make sure that it is safe to dereference that variable without checking that it is not null.

1. The variable must be initialized to a non-null value.
2. The variable can never be assigned the null value.

##### **Rules for Nullable Reference Type in C#**

When a variable can be null, in that case, the compiler enforces different rules to make sure that you have correctly checked for a null reference.

1. The variable may only be dereferenced when the compiler can guarantee that the value is not null.
2. It may be initialized with the default null value and may be assigned the value null in another code.