# Pattern Matching

mechanism to see if value matches a shape or type. If it matches the pattern it will extract the data from the value.

Pattern can be used with any data type. If else statements can only match patterns with primitive data types (don't really understanding it yet.

What you will learn: when and is clauses for pattern matching, tuples and how to use them effectively, Digit Separators( _ ), when to use thrown exceptions, expression bodied members, Async main methods, Local Functions, ref and out variable and how to discard a out variable ( _ ).
## Switch Statements

switch statements are considered to be pattern matching statements. The default clause is evaluated last and clauses that come first are executed first.

### When Clause

 We can specify this condition using the when clause

```csharp
using System;
namespace PatternMatchingDemo
{
    class Program
    {
        static void Main()
        {
            Rectangle square = new Rectangle(10, 10);
            DisplayArea(square);
            Rectangle rectangle = new Rectangle(10, 5);
            DisplayArea(rectangle);
            Circle circle = new Circle(10);
            DisplayArea(circle);
            Triangle triangle = new Triangle(10, 5);
            DisplayArea(triangle);
            Console.ReadKey();
        }
        public static void DisplayArea(Shape shape)
        {
            switch (shape)
            {
                case Rectangle r when r.Length == r.Height:
                    Console.WriteLine("Area of Sqaure is : " + r.Length * r.Height);
                    break;
                case Rectangle r:
                    Console.WriteLine("Area of Rectangle is : " + r.Length * r.Height);
                    break;
                case Circle c:
                    Console.WriteLine("Area of Circle is : " + c.Radius * c.Radius * Shape.PI);
                    break;
                case Triangle t:
                    Console.WriteLine("Area of Triangle is : " + 0.5 * t.Base * t.Height);
                    break;
                default:
                    throw new ArgumentException(message: "Invalid Shape", paramName: nameof(shape));
            }
        }
    }
}
```
## Pattern Matching with "is" expression

The **“is”** operator is available from the first version of C# and it is used to check whether an object is compatible with a specific type or not. For example, if a specific interface is implemented, or if the type of the object derives from a base class or not.

```csharp
using System;
namespace PatternMatchingDemo
{
    //Base Class
    public class Shape
    {
        public const float PI = 3.14f;
    }

    //Derived Class 1
    public class Circle : Shape
    {
        public double Radius { get; }
        public Circle(double radius)
        {
            Radius = radius;
        }
    }

    //Derived Class 2
    public class Rectangle : Shape
    {
        public double Length { get; }
        public double Height { get; }
        public Rectangle(double length, double height)
        {
            Length = length;
            Height = height;
        }
    }

    //Derived Class 3
    public class Triangle : Shape
    {
        public double Base { get; }
        public double Height { get; }
        public Triangle(double @base, double height)
        {
            Base = @base;
            Height = height;
        }
    }
}
```


```csharp
using System;
namespace PatternMatchingDemo
{
    class Program
    {
        static void Main()
        {
            Circle circle = new Circle(10);
            DisplayArea(circle);
            Rectangle rectangle = new Rectangle(10, 5);
            DisplayArea(rectangle);
            Triangle triangle = new Triangle(10, 5);
            DisplayArea(triangle);
            Console.ReadKey();
        }
        public static void DisplayArea(Shape shape)
        {
            //If variable shape holds the object of type Circle,
            //then it will store that object in variable c
            if (shape is Circle c)
            {
                Console.WriteLine("Area of Circle is : " + c.Radius * c.Radius * Shape.PI);
            }

            //If variable shape holds the object of type Rectangle,
            //then it will store that object in variable r
            else if (shape is Rectangle r)
            {
                Console.WriteLine("Area of Rectangle is : " + r.Length * r.Height);
            }

            //If variable shape holds the object of type Triangle,
            //then it will store that object in variable t
            else if (shape is Triangle t)
            {
                Console.WriteLine("Area of Triangle is : " + 0.5 * t.Base * t.Height);
            }
            else
            {
                throw new ArgumentException(message: "Invalid Shape", paramName: nameof(shape));
            }
        }
    }
}
```


# Digit Separators

You can use "_" underscore to separate digits now in C#.

```csharp
using System;
class Program
{
    static void Main()
    {
        // Both are equivalent.
        var bigNumber = 123456789012345678;
        var bigNumberSplit = 123_456_789_012_345_678;

        Console.WriteLine("bigNumber : {0}, bigNumberSplit : {1}", bigNumber, bigNumberSplit);

        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }
}
```

in the output the underscores are nowhere to be found.
you can put as many underscores in between numeric values as you want and it will work as expected.
# Tuples

“**Will a tuple simplify the code here**“. If the answer is “**yes**“, then use a tuple.

 **Tuples in C# 7 are values, so they are copied by value, rather than by reference. Most of the time, this should not be an issue. However, if you are passing around tuples of large structs (this is because every single value in this big tuple is going to have to copied by value to the formal parameters.), this might have an impact on the performance of the application. Ref locals/returns can be used to work around these performance issues**
## How Can You Return More Than One Value From a Method?

1. **Using Custom `DataType`:** classes and objects can be used to return multiple values from a method like the `EventArgs` class/classes. But a lot of times creating a whole class for that feels like to much effort.
2. **Using Ref and Out variable.** The **“out” and “ref”** parameters will not work with the `async` methods.
3. **Using dynamic keyword:** You can also return multiple values from a method by using the dynamic keyword as the return type. But from a performance point of view, we probably don’t want to use dynamic.
4. Tuples

Tuples allow you to return more than one value from a method.
## Old School Tuples

Since .NET Framework 4.0 we had the class `Tuple`.

```csharp
using System;
using System.Collections.Generic;
namespace TulesDemo
{
    class Program
    {
        static void Main()
        {
            var values = new List<double>() { 10, 20, 30, 40, 50 };

            //Store the Result of Calulate Method in a variable of Tuple type 
            Tuple<int, double> t = Calulate(values);

            //Access the First value using Item1 and second value using Item2 properties
            Console.WriteLine($"There are {t.Item1} values and their sum is {t.Item2}");
            Console.ReadKey();
        }

        //Declaring the return type as Tuple<int, double>
        private static Tuple<int, double> Calulate(IEnumerable<double> values)
        {
            int count = 0;
            double sum = 0.0;
            foreach (var value in values)
            {
                count++;
                sum += value;
            }

            //Creating an object of Tuple class by calling the static Create method
            Tuple<int, double> t = Tuple.Create(count, sum);

            //Returning the tuple instance
            return t;
        }
    }
}
```

Problems with the Old School Tuple Class:

1. The **First Problem** is that the Tuples in C# are classes, i.e. reference types. As reference types, the memory is allocated on the Heap Area and Garbage is collected only when they are no longer used. For applications where performance is a major concern, it can be an issue.
2. The **Second Problem** is that the elements in the tuple don’t have any names and you can only access them by using the build-in names such as Item1, Item2, Item3, etc. which are not meaningful at all. The **Tuple<T1, T2>** type does not provide any information about what the tuple actually represents which makes it a poor choice in public APIs.
3. The **Third Problem** is that you can use a maximum of 8 properties in a Tuple in C#. If you want to return more than 8 values from a method, then again the last argument of the Tuple must be another Tuple i.e. Tuple within another Tuple. This makes the syntax more difficult to understand.




# New School Tuples

With C# 7, now it is possible to declare the tuple as **inline**, which is like an anonymous type, except that they are not limited to the current method.

```csharp
using System;
using System.Collections.Generic;
namespace TulesDemo
{
    class Program
    {
        static void Main()
        {
            var values = new List<double>() { 10, 20, 30, 40, 50 };
            var result = Calulate(values);
            Console.WriteLine($"There are {result.Item1} values and their sum is {result.Item2}");
            Console.ReadKey();
        }
        
        private static (int, double) Calulate(IEnumerable<double> values)
        {
            int count = 0;
            double sum = 0.0;
            foreach (var value in values)
            {
                count++;
                sum += value;
            }
            return (count, sum);
        }
    }
}
```

if you are getting **Predefined type ‘System.ValueTuple´2´ is not defined or imported** error, then you need to add the **`System.ValueTuple`** package from NuGet Package Manager.
## Deconstruction (Splitting Tuples)

Once you retrieve the tuple, then you need to handle its individual elements. Handling these elements one by one is really a dirty approach. We can overcome this by splitting the Tuples in C#.

Deconstruction of tuples can be done in 3 ways:

### Giving Parameters To Tuples

```csharp
using System;
using System.Collections.Generic;
namespace TulesDemo
{
    class Program
    {
        static void Main()
        {
            var values = new List<double>() { 10, 20, 30, 40, 50 };
            var result = Calulate(values);
            Console.WriteLine($"There are {result.count} values and their sum is {result.sum}");
            Console.ReadKey();
        }
        
        private static (int count, double sum) Calulate(IEnumerable<double> values)
        {
            int count = 0;
            double sum = 0.0;
            foreach (var value in values)
            {
                count++;
                sum += value;
            }
            return (count, sum);
        }
    }
}
```




### Using var Keyword to Infer The Type of Each Variable

```csharp
using System;
class Program
{
    static void Main()
    {
        //De-Constructing Tuples 
        var (Name, Salary, Gender, Dept) = GetEmployeeDetails(1001);

        //Do something with the data.
        //Here we are just printing the data in the console
        Console.WriteLine("Employee Details :");
        Console.WriteLine($"Name: {Name},  Gender: {Gender}, Department: {Dept}, Salary:{Salary}");

        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }

    //GetEmployeeDetails Method returns a tuple with 4 values
    private static (string, double, string, string) GetEmployeeDetails(long EmployeeID)
    {
        //Based on the EmployyeID get the data from a database
        //Here we are hardcoded the value
        string EmployeeName = "Pranaya";
        double Salary = 2000;
        string Gender = "Male";
        string Department = "IT";

        //Returning 4 Values through a Tuple
        return (EmployeeName, Salary, Gender, Department);
    }
}
```

You can mix var with strongly typed variable declarations as well. Which you really should not do because it's just fucking retarded.

```csharp
using System;
class Program
{
    static void Main()
    {
        //De-Constructing Tuples 
        (var Name, var Salary, string Gender, var Dept) = GetEmployeeDetails(1001);

        //Do something with the data.
        //Here we are just printing the data in the console
        Console.WriteLine("Employee Details :");
        Console.WriteLine($"Name: {Name},  Gender: {Gender}, Department: {Dept}, Salary:{Salary}");

        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }

    //GetEmployeeDetails Method returns a tuple with 4 values
    private static (string, double, string, string) GetEmployeeDetails(long EmployeeID)
    {
        //Based on the EmployyeID get the data from a database
        //Here we are hardcoded the value
        string EmployeeName = "Pranaya";
        double Salary = 2000;
        string Gender = "Male";
        string Department = "IT";

        //Returning 4 Values through a Tuple
        return (EmployeeName, Salary, Gender, Department);
    }
}
```

### Deconstructing Tuples to Already Declared Variables

```csharp
using System;
class Program
{
    static void Main()
    {
        //First Declare the Variables
        string Name;
        double Salary;
        string Gender = "Female";
        string Dept = "HR";

        //De-Constructing Tuples into Already Declared variables
        (Name, Salary, Gender, Dept) = GetEmployeeDetails(1001);

        //Do something with the data.
        //Here we are just printing the data in the console
        Console.WriteLine("Employee Details :");
        Console.WriteLine($"Name: {Name},  Gender: {Gender}, Department: {Dept}, Salary:{Salary}");

        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }

    //GetEmployeeDetails Method returns a tuple with 4 values
    private static (string, double, string, string) GetEmployeeDetails(long EmployeeID)
    {
        //Based on the EmployyeID get the data from a database
        //Here we are hardcoded the value
        string EmployeeName = "Pranaya";
        double Salary = 2000;
        string Gender = "Male";
        string Department = "IT";

        //Returning 4 Values through a Tuple
        return (EmployeeName, Salary, Gender, Department);
    }
}
```






## Tuple Exceptions

Note that you cannot specify a specific type outside the parentheses even if every field in the tuple has the same type. This generates compiler error **CS8136, “Deconstruction ‘var (…)’ form disallows a specific type for ‘var’.”.**

Note that you must assign each element of the tuple to a variable. If you omit any elements, the compiler generates error **CS8132, “Cannot deconstruct a tuple of ‘x’ elements into ‘y’ variables.”**

You cannot mix declarations and assignments to existing variables on the left-hand side of a deconstruction. The compiler generates error **CS8184, “a deconstruction cannot mix declarations and expressions on the left-hand side.”** when the members include newly declared and existing variables.
# Throw Expressions

```csharp
using System;
namespace ThrownExpressionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Main Method Execution Started...");

            //Calling the Divide Method by passing 10 and 0
            var Result = Divide(10, 0);
            Console.WriteLine($"Result: {Result}");
            Console.WriteLine("Main Method Execution Completed...");
            Console.ReadKey();
        }

        public static double Divide(int FirstNumber, int SecondNumber)
        {
            //Throwing an Exception in the Middle of the Expression
            return SecondNumber != 0 ? 
                FirstNumber % SecondNumber : 
                throw new DivideByZeroException();
        }
    }
}
```

You can add Exception Throwing to expression-bodied members, null-coalescing expressions, and conditional expressions. Throw expressions are the way to tell the compiler to throw the exception under specific conditions like in expression-bodied members or inline comparisons.

## Handy Situations For You To Thrown an Exception

1. Auto-Property Initializer Statement
2. Inline Comparison using the “?” operator 
3. Expression-Bodied Member


```csharp
using System;
namespace ThrownExpressionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            TryWithNameNull();
            TryGetFirstName();
            TryGetLastName();

            Console.WriteLine("Press any key to exist.");
            Console.ReadKey();
        }

        static void TryWithNameNull()
        {
            try
            {
                new Employee(null);
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.GetType() + ": " + ex.Message);
            }
        }

        static void TryGetFirstName()
        {
            try
            {
                new Employee("Pranaya").GetFirstName();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.GetType() + ": " + ex.Message);
            }
        }

        static void TryGetLastName()
        {
            try
            {
                new Employee("Pranaya").GetLastName();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.GetType() + ": " + ex.Message);
            }
        }
    }

    class Employee
    {
        public string FullName { get; }
        public Employee(string name) => FullName = name ?? throw new ArgumentNullException(name);
        public string GetFirstName()
        {
            var parts = FullName.Split(' ');
            return (parts.Length > 1) ? parts[0] : throw new InvalidOperationException("Method:GetFirstName, Full Name is not available");
        }
        public string GetLastName() => throw new NotImplementedException("Method GetLastName is not Implemented");
    }
}
```

# `Async Main` Method

 asynchronous programming performs long-running operations without blocking the main thread of execution. The async keyword in C# defines an asynchronous method, which allows you to use the await keyword to call other asynchronous methods without blocking. This is particularly useful in desktop, web, and mobile applications where a responsive user interface is important.
 
Starting with C# 7.1, you can create an `async` Main method as well, which allows you to use await at the entry point of your application. This is helpful when performing asynchronous operations as soon as the application starts without blocking the main thread.

`Async` Main Method allow you to use await in the application's entry point. Therefore you can perform `async` operations as soon as the application starts.

## Properties of C# Main Method

1. The C# entry point method must be static,
2. The name of the method must be the Main
3. The return type of this method can be either void or int.
4. It can have one parameter of a string array containing any command-line arguments.

allowed signatures:
```csharp
public static void Main();
public static int Main();
public static void Main(string[] args);
public static int Main(string[] args);
public static Task Main();
public static Task<int> Main();
public static Task Main(string[] args);
public static Task<int> Main(string[] args);
```


## When Do We Use an `Async Main` Method?

###### **I/O-Bound Operations**

When your application performs I/O-bound operations at startup, such as:

- Reading from or writing to files.
- Making network requests (e.g., calling web APIs).
- Database operations.
execute these applications asynchronously to improve responsiveness and scalability.
###### **Asynchronous Libraries**

If your application relies on libraries that expose asynchronous APIs, you can directly call these APIs from the Main method using await. This is cleaner and more straightforward than using .GetAwaiter().GetResult() or .Result, which can lead to deadlocks in certain contexts.

###### **Console Applications**

In console applications that perform asynchronous operations, using an async Main method simplifies the codebase. It’s especially beneficial in scenarios like:

- Command line tools that interact with HTTP services.
## **Things to Consider**

- **Application Type:** Primarily useful in console applications. In other applications like web apps, the framework typically handles asynchrony for you.
- **Error Handling:** Ensure proper error handling in the Main method. Unhandled exceptions in asynchronous methods can terminate your application.
- **Compatibility:** async Main is available from C# 7.1 onwards, so ensure your project is configured to use an appropriate language version.
# Local Functions

The **Local Functions in C#** are the special kind of inner function or you can say sub-function or function within a function that can be declared and defined by the parent function. These methods or functions are private methods for their containing type and are only called by their parent method.

If you want to execute some piece of code multiple times within a method then you can put those codes as an inner function or you can say local function within that method. Then call that local function whenever required from the parent method.

## Use case

1. Small helper functions are to be used several times within the main or parent method.
2. Parameter validation functions for any iterators or asynchronous methods. For example an `IsRequestValid()` method to see if the HTTP Request received by the parent function is valid.
3. An alternate to recursive functions as local function comparatively takes less memory due to the reduced call stack.

## Characteristics

1. You can not overload a Local Function in C#.
2. The Accessibility modifiers such as public, private, and protected are not allowed.
3. The compiler will issue a warning if the local function is not used by the parent function as there is no meaning of defining a local function in C# if it is not being used by the parent method.
4. All variables in the enclosing scope, including local variables, can be accessed.
# Expression Bodied Members

Allows us to provide the implementation of a member in a better readable format. You can use it whenever the logic for any supported members such as a method or property consists of a single expression. It's definition has the following general syntax.

Feels a lot like a lambda expression but it is not. Lambda expressions have a body and expression bodied members can only have one statement/expression as it's body.

Turns a value whose type matches the method’s return type, or, for methods that return void, that performs some operation.

**member => expression;** Where expression is a valid C# expression.

`public string name => "Tiago";`
`public string LastName { get => LastName;  set {LastName = value} }`

## Traits

1. Expression Bodied Methods can specify all the accessibility operators i.e. public, protected, internal, private, and protected internal.
2. These can be declared virtual or abstract or can even override a base class method.
3. Such methods can be static.
4. Methods can even support asynchronous behavior if they return void, Task, or `Task<T>`.
## You Can Use It With

1. Methods
2. Properties
3. Constructor
4. Deconstructors
5. Destructor
6. Getters
7. Setters
8. Indexers





## Limitations

The Expression Bodied Members in C# 7 don’t support a block of code. It supports only one statement with an expression. But you can always make up for it by just implementing a lambda expression instead of a expression bodied member.

Branching Statements such as (if..else, switch case) are not allowed with Expression Bodied Members. However if..else behavior can be addressed by using the ternary operator.

Looping statements such as (for, for each, while, and do..while are not allowed) with Expression Bodied Members in C# 7. However, in some cases, it can be addressed with LINQ queries. Example:
`**public IEnumerable<int> HundredNumbersListWithExprBody() => from n in Enumerable.Range(0, 100) select n;**``


# Generalized `Async` return Types

## `Async` methods can have 3 return types

1. **`Task<TResult>:`** This return type is used when the a`sync` method returns a value.
2. **Task:**  This return type is used when the` async` method does not return any value but you want the caller method to wait for the `async` method execution completion.
3. **void:** This return type is used when the `async` method does not return any value as well as you don’t want the caller method to wait for the `async` method execution completion. **They simply call the `async` method and continue their work**. So, if you have methods other than event handlers that don’t return any value, it’s always advisable to use Task return type instead of void.

## Generalized Asynchronous Return Types

Task is a class. We also know the reference types behave differently in C#. In some situations, it is better to return anything rather than a Task i.e. rather than a class.

The Generalized `Async` Returns Types in C# 7 means, now you can return a lightweight value type instead of a reference type (Task) to avoid additional memory allocations. From C# 7, there is an inbuilt value type i.e. **`ValueTask <T>`** which can be used instead of **`Task<T>`**.

**To use the **`System.Threading.Tasks.ValueTask<TResult>`** type, you must add the **`System.Threading.Tasks.Extensions`** NuGet Package to your project.**

```csharp
using System;
using System.Linq;
using System.Threading.Tasks;

//You need to add System.Threading.Tasks.Extensions Package from NuGet
namespace GeneralizedAsyncReturnTypesDemo
{
    public class Example
    {
        public static void Main()
        {
            Console.WriteLine(ShowTodaysInfo().Result);
            Console.WriteLine("Press any key to exist.");
            Console.ReadKey();
        }

        //Return Type of the Async Method is ValueTask<string> which is a value type
        private static async ValueTask<string> ShowTodaysInfo()
        {
            var infoTask = GetLeisureHours();
            // You can do other work that does not rely on integerTask before awaiting.
            string ret = $"Today is {DateTime.Today:D}\n" +
                         "Today's hours of leisure: " +
                         $"{await infoTask}";
            return ret;
        }

        //Return Type of the Async Method is ValueTask<int> which is a value type
        static async ValueTask<int> GetLeisureHours()
        {
            // Task.FromResult is a placeholder for actual work that returns a string.  
            var today = await Task.FromResult<string>(DateTime.Now.DayOfWeek.ToString());

            // The method then can process the result in some way.  
            int leisureHours;
            if (today.First() == 'S')
                leisureHours = 16;
            else
                leisureHours = 5;
            return leisureHours;
        }
    }
}
```

### If all of this is just about `ValueTask<T>` how is it generalized?

you can also create your own data type which can be the return type of your async method. However, if you do not want to create your own type, then you can use the **`ValueTask<T>`** which is already available.

# Ref Returns and Ref Locals

If a method just returns one value, then it’s always better to use a return type instead of the out or ref modifier.

### Ref Local
The **Ref Local** is a new variable type that is used to store the references. It is mostly used in conjunction with Ref returns to store the reference in a local variable. That means Local variables now can also be declared with the ref modifier

The variable no2 references variable no1, and thus changing no2 changes no1 as well:

```csharp
using System;
namespace RefReturnsAndRefLocalsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int Number1 = 1;

            //Create a Local variable using ref keyword and store the referenec of another variable
            ref int Number2 = ref Number1;
            Number2 = 2;
            Console.WriteLine($"Local Variable {nameof(Number1)} After The Change: {Number1}");
            Console.ReadLine();
        }
    }
}
```

### Ref Return

With C# 7.0, this constraint has been waived off and now you can return references from a method as well. This change is really adding flexibility to handle the scenarios when we want references to return in order to make an in-lined replacement.

Here the program will change the value of an array index with a ref variable.

```csharp
using System;
namespace RefReturnsAndRefLocalsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating an Integer Array
            int[] IntegerArray = { 2, 4, 62, 54, 33, 55, 59, 71, 92 };

            Console.WriteLine("Printing All Elements of IntegerArray");
            for (int i = 0; i < IntegerArray.Length; i++)
            {
                Console.Write($"{IntegerArray[i]}\t");
            }
            Console.WriteLine();
            //Creating an Instance of the Program class
            Program program = new Program();

            //Invoking the GetFirstOddNumber method with ref keyword
            //Storing the Result in a variable of ref type
            ref int OddNumber = ref program.GetFirstOddNumber(IntegerArray);

            //Printing the Odd Number returned by GetFirstOddNumber method
            Console.WriteLine($"First Odd Number Returned By GetFirstOddNumber Method: {OddNumber}");

            //Changing the oddNum value as it is a reference variable,
            //it will also change the same in the actual Integer Array
            OddNumber = 35;
            Console.WriteLine("Changing First Odd Number Value to 35");
            //Printing all the Elements of the IntegerArray
            Console.WriteLine("Printing All Elements of IntegerArray After Changing First Odd Number");
            for (int i = 0; i < IntegerArray.Length; i++)
            {
                Console.Write($"{IntegerArray[i]}\t");
            }
            
            Console.ReadKey();
        }

        //Making the Method return type as ref int to return the reference of an integer number
        public ref int GetFirstOddNumber(int[] numbers)
        {
            for (int i = 0; i < numbers.Length; i++)
            {
                if (numbers[i] % 2 == 1)
                {
                    //Returning the Reference of the Number
                    return ref numbers[i];  
                }
            }
            throw new Exception("Odd Number Not Found");
        }
    }
}
```


# Out Variables and Discards

out variables can be of var data type

to ignore output parameter just type: `out _`. when you call the method

