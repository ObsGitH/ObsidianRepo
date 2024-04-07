
# Optional Parameters

## What Is It?

Parameters in C# are compulsory by default. You have to pass a value to them in order for the method and program to work.
Optional Parameters are parameters that allow the method to work just fine even if you don't pass a value to it.
Optional parameters can be declared in a delegate, indexer, method or constructor.
**Optional parameters always come last in the parameter list.**

## The 4 Ways You Can Implement Optional Parameters

### 1. Parameter Array
 
```csharp
using System;
namespace OptionalParameter
{
    class Program
    {
        static void Main(string[] args)
        {
            ADDNumbers(10, 20);
            ADDNumbers(10, 20, 30, 40);
            ADDNumbers(10, 20, new object[] { 30, 40, 50 });
            Console.ReadLine();
        }

        public static void ADDNumbers(int FN, int SN, params object[] restOfTheNumbers)
        {
            int result = FN + SN;
            foreach (int i in restOfTheNumbers)
            {
                result += i;
            }
            Console.WriteLine("Total = " + result.ToString());
        }
    }
}
```

You can pass 0 or n amount of objects to the last parameter of  `AddNumbers`. That is because `restOfTheNumbers` is a parameter array and therefore is optional.

Also play attention to this code as an example on how to implement parameter arrays in your program. Here we are adding 2 (FN, SN) up to 2 + n (FN, SN and n amount of objects in the `restOfTheNumbers` parameter array) amount of numbers. 

The parameter array always comes last in the parameter list.
### 2. Method Overloading

```csharp
using System;
namespace OptionalParameter
{
    class Program
    {
        static void Main(string[] args)
        {
            ADDNumbers(10, 20);        
            ADDNumbers(10, 20, new int[] { 30, 40, 50 });
            
            Console.ReadLine();
        }

        public static void ADDNumbers(int FN, int SN, int[] restOfTheNumbers)
        {
            int result = FN + SN;
            foreach (int i in restOfTheNumbers)
            {
                result += i;
            }
            Console.WriteLine("Total = " + result.ToString());
        }

        public static void ADDNumbers(int FN, int SN)
        {
            int result = FN + SN;
            Console.WriteLine("Total = " + result.ToString());
        }
    }
}
```

The Overload of  `AddNumbers()` makes the last parameter optional. If no value is passed to the `restOfTheNumbers` parameter the bottom implementation of `AddNumbers` is the one executed.

### 3. Parameter with a Default Value
```csharp
using System;
namespace OptionalParameter
{
    class Program
    {
        static void Main(string[] args)
        {
            //Adding two Integer Numbers
            ADDNumbers(10, 20);

            //Adding Five Integer Numbers
            ADDNumbers(10, 20, new int[] { 30, 40, 50 });

            Console.ReadLine();
        }

        public static void ADDNumbers(int FN, int SN, int[] restOfTheNumbers = null)
        {
            int result = FN + SN;
            //Loop through the restOfTheNumbers only if it is not null
            //else we will get runtime error
            if (restOfTheNumbers != null)
            {
                foreach (int i in restOfTheNumbers)
                {
                    result += i;
                }
            }
            
            Console.WriteLine("Total = " + result.ToString());
        }
    }
}
```

default values make the parameters optional by assigning a value implicitly if you (developer) does not pass any.

### 4. `The Optional` Attribute

The **`OptionalAttribute`** in C# that is present in **`System.Runtime.InteropServices`** namespace

```csharp
using System;
using System.Runtime.InteropServices;

namespace OptionalParameter
{
    class Program
    {
        static void Main(string[] args)
        {
            ADDNumbers(10, 20);        
            ADDNumbers(10, 20, new int[] { 30, 40, 50 });
           
            Console.ReadLine();
        }

        public static void ADDNumbers(int FN, int SN, [Optional] int[] restOfTheNumbers)
        {
            int result = FN + SN;
            // loop thru restOfTheNumbers only if it is not null otherwise 
            // you will get a null reference exception
            if (restOfTheNumbers != null)
            {
                foreach (int i in restOfTheNumbers)
                {
                    result += i;
                }
            }
            Console.WriteLine("Total = " + result.ToString());
        }
    }
}
```

decorating a parameter with the Optional attribute makes that parameter optional.
## The Powerful Named Parameters

When passing arguments to methods you can explicitly define which parameter you want to pass the value to with named parameters.

Syntax: `parameterName: value`

Named parameters allow you to disrespect the predefined order of parameters use the first argument in a method to pass a value to the third parameter of the method's signature for example.

```csharp
using System;
namespace NamedParameter
{
    class Program
    {
        static void Main(string[] args)
        {
            //Using Named Parameter while calling Method
            Test(1, 2); //a = 1 and b = 2 and c = 20 by default value
            Test(1, c: 2); //a = 1 and b = 10 by default and c = 2

            //Order is not Important with Named Parameter
            Test(b:1, c: 2, a:10);
            Console.ReadLine();
        }
        
        public static void Test(int a, int b = 10, int c = 20)
        {
            Console.WriteLine($"a = {a}, b = {b}, c= {c}");
        }
    }
}
```
#  Indexers

## What Are Indexers?

Indexers allow instances of a class to be indexed just like arrays. The indexed value can be set or retrieved without explicitly specifying a type or instance member.

Array is a predefined class. Array elements can be accessed through and index because of indexers. Therefore user-defined classes can also have their properties accessed through an index

## Implementation

indexer syntax: `acessModifiers type(of the properties that are going to be indexed) this(to tell c# that we are creating an indexer for this class) [int Index or string Name (what index we are going to use (strings or int's))] 

```csharp
`acessModifiers type(of the properties that are going to be indexed) this(to tell c# that we are creating an indexer for this class) [int Index or string Name (what index we are going to use (strings or ints))]
{
	[get{statements}] //get accessor
	[set{statements}] //set accessor
}
```

resuming:
```csharp
	class Whatever
	{
		acessModifier type this [int number or string Name]
		{
			get
			{
			
			}
			set
			{
			
			}
		}
	}
```

Now let's actually implement this shit:

```csharp
using System;
namespace IndexersDemo
{
	class Program
	
	{
	
		static void Main(string[] args)
		
		{
		//Create an Instance of the Employee Class
		Employee emp = new Employee(101, "Pranaya", "SSE", 10000, "Mumbai", "IT", "Male");
		//Access the Employee Object using Indexer i.e. using string Index name
		Console.WriteLine("EID = " + emp["ID"]);
		Console.WriteLine("Name = " + emp["Name"]);
		Console.WriteLine("Job = " + emp["job"]);
		Console.WriteLine("Salary = " + emp["salary"]);
		Console.WriteLine("Location = " + emp["Location"]);
		Console.WriteLine("Department = " + emp["department"]);
		Console.WriteLine("Gender = " + emp["Gender"]);
		//Set the Employee Object using Indexer i.e. using string Index name
		emp["Name"] = "Kumar";
		emp["salary"] = 65000;
		emp["Location"] = "BBSR";
		Console.WriteLine("========Afrer Modification========");
		//Access the Employee Object using Indexer i.e. using Integer Index Position
		Console.WriteLine("EID = " + emp[0]);
		Console.WriteLine("Name = " + emp[1]);
		Console.WriteLine("Job = " + emp[2]);
		Console.WriteLine("Salary = " + emp[3]);
		Console.WriteLine("Location = " + emp[4]);
		Console.WriteLine("Department = " + emp[5]);
		Console.WriteLine("Gender = " + emp[6]);
		Console.ReadLine();
		}
	}

    public class Employee
    {
        //Declare the Properties
        public int ID { get; set; }
        public string Name { get; set; }
        public string Job { get; set; }
        public double Salary { get; set; }
        public string Location { get; set; }
        public string Department { get; set; }
        public string Gender { get; set; }

        //Initialize the Propertities through constructor
        public Employee(int ID, string Name, string Job, int Salary, string Location,
                        string Department, string Gender)
        {
            this.ID = ID;
            this.Name = Name;
            this.Job = Job;
            this.Salary = Salary;
            this.Location = Location;
            this.Department = Department;
            this.Gender = Gender;
        }

        //Creating the Indexer using Integer Index Position
        public object this[int index]
        {
            //Get accessor is used for returning a value
            get
            {
                //based in the index position, return the appropriate property value
                if (index == 0)
                    return ID;
                else if (index == 1)
                    return Name;
                else if (index == 2)
                    return Job;
                else if (index == 3)
                    return Salary;
                else if (index == 4)
                    return Location;
                else if (index == 5)
                    return Department;
                else if (index == 6)
                    return Gender;
                else
                    return null;
            }
            //Set accessor is used to assigning a value
            set
            {
                //based in the index position, set the appropriate property value
                if (index == 0)
                    ID = Convert.ToInt32(value);
                else if (index == 1)
                    Name = value.ToString();
                else if (index == 2)
                    Job = value.ToString();
                else if (index == 3)
                    Salary = Convert.ToDouble(value);
                else if (index == 4)
                    Location = value.ToString();
                else if (index == 5)
                    Department = value.ToString();
                else if (index == 6)
                    Gender = value.ToString();
            }
        }

        //Creating the Indexer using string Index name
        public object this[string Name]
        {
            //Get accessor is used for returning a value
            get
            {
                //based in the index name, return the appropriate property value
                if (Name.ToUpper() == "ID")
                    return ID;
                else if (Name.ToUpper() == "NAME")
                    return Name;
                else if (Name.ToUpper() == "JOB")
                    return Job;
                else if (Name.ToUpper() == "SALARY")
                    return Salary;
                else if (Name.ToUpper() == "LOCATION")
                    return Location;
                else if (Name.ToUpper() == "DEPARTMENT")
                    return Department;
                else if (Name.ToUpper() == "GENDER")
                    return Gender;
                else
                    return null;
            }
            //Set accessor is used to assigning a value
            set
            {
                //based in the index name, set the appropriate property value
                if (Name.ToUpper() == "ID")
                    ID = Convert.ToInt32(value);
                else if (Name.ToUpper() == "NAME")
                    Name = value.ToString();
                else if (Name.ToUpper() == "JOB")
                    Job = value.ToString();
                else if (Name.ToUpper() == "SALARY")
                    Salary = Convert.ToDouble(value);
                else if (Name.ToUpper() == "LOCATION")
                    Location = value.ToString();
                else if (Name.ToUpper() == "DEPARTMENT")
                    Department = value.ToString();
                else if (Name.ToUpper() == "GENDER")
                    Gender = value.ToString();
            }
        }
    }
}
```

A lot of things are going on...

- As you can see it is possible to overload an indexer. One indexer is indexing with ints and the other with strings.
- the indexer type is object because the properties have different types, the only way to index all of them in the same indexer is with an indexer of object type. This will generate implicit unboxing which sucks.
- The logic inside the get and set accessor would be more readable with a switch block
- The `ToUpper()` method takes away the restriction of indexers that index with strings. Normally you would need to match the index string perfectly to access the property through the index. With `ToUpper()` or `ToLower()` you just need to worry about writing a string with the right sequence of characters and not worry about upper-case or lower-case. (if the web-page writer used a switch block he would not need to write the `ToUpper()` method one thousand times)

Go to this link to see a useful application of an indexer:
https://dotnettutorials.net/lesson/indexers-real-time-example-csharp/


# Enumerations

## What Are `Enum`'s?

Enumerations are Strongly Typed Name Constants. They are also user-defined value data types

Enumerations are genius. They allow you to represent anything that is fixed like a state. for a person that would be: eating, sleeping,. For a semaphore: red, green, yellow. Any group of constants that relate to each other and to one thing(object, entity, concept, whatever) should be implemented as an `enum` in your program.
## `Enum` Traits

1. The `Enums` are Enumerations.
2. `Enums` are **Strongly Typed Named** **Constants**. Explicit Cast is needed to convert them from the `enum` type to an integral type and vice versa. Also, **an `enum` of one type cannot be implicitly assigned to an `enum` of another type even though the underlying value of their members is the same.**
3. The default underlying type of an `enum` is int.
4. The default value for the first element of the `enum` is ZERO and gets incremented by 1.
5. It is also possible to customize the underlying type and values of `enums`.
6. The `Enums` are complex value data types.
7. `enum` keyword (all small letters) is used to create the enumerations, whereas the `Enum` class, contains static `GetValues()` and `GetNames()` methods which can be used to list `Enum` underlying type values and Names.
8. `Enum`'s are sealed. In the sense that inheritance does not work in any way with `Enum`'s
9. `Enum` members need to have a unique Name and underlying value. Just like classes cannot have 2 members with the same name.

### How to Change `Enum` Default Value(int) to Another


```csharp
public enum Gender : short
{
	Unknown, //Type short and value 0
	Male = 20, //Type short and value 20 (would be 0 if we did not explicitly define it's value)
	Female = 30, //Type  short and value 30
}
```

`enum` is a predefined value data type so the underlying value of the `enum`  has to be a primitive value data type

### `Enum`s of Different Types Cannot Be Implicitly Assigned to Each Other

```csharp
namespace EnumsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            // This following line will not compile. 
            // Cannot implicitly convert type 'Season' to 'Gender'. 
            // An explicit conversion is required.
            // Gender gender = Season.Winter;

            // The following line compiles as we have an explicit cast
            Gender gender = (Gender)Season.Winter;
        }
    }

    //Gender Enum
    public enum Gender : int
    {
        Unknown = 1,
        Male = 2,
        Female = 3
    }

    //Season Enum
    public enum Season : int
    {
        Winter = 1,
        Spring = 2,
        Summer = 3
    }
}
```


### Useful Things From the `Enum` Class

1. `**GetValues():**` retrieves an array of the values of the constants in a specified enumeration.
2. **`GetNames(`):** retrieves an array of the names of the constants in a specified enumeration.

While calling the above two methods, we need to pass the type of the `enum`: 
`GetValues(typeof(EnumType))`
`GetNames(typeof(Season))`
