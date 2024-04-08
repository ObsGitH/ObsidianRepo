
# `AutoMapper` Overview

## What is this Library?

A Library dedicated to map data members of one object to data members of another object. That is... make properties of the same data type and Name have the same values in both objects.

It maps the properties of two different objects by transforming the input object of one type to the output object of another.

## Why Use This Library?

Manual mapping is done like this

```csharp
using System;
namespace AutoMapperDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Create and Populate Employee Object
            Employee emp = new Employee
            {
                Name = "James",
                Salary = 20000,
                Address = "London",
                Department = "IT"
            };

            //Mapping Employee Object to EmployeeDTO Object
            EmployeeDTO empDTO = new EmployeeDTO
            {
                Name = emp.Name,
                Salary = emp.Salary,
                Address = emp.Address,
                Department = emp.Department
            };

            Console.WriteLine("Name:" + empDTO.Name + ", Salary:" + empDTO.Salary + ", Address:" + empDTO.Address + ", Department:" + empDTO.Department);
            Console.ReadLine();
        }
    }
    
    public class Employee

	{
	
	public string Name { get; set; }
	
	public int Salary { get; set; }
	
	public string Address { get; set; }
	
	public string Department { get; set; }
	
	}

	public class EmployeeDTO
	
	{
	
	public string Name { get; set; }
	
	public int Salary { get; set; }
	
	public string Address { get; set; }
	
	public string Department { get; set; }
	
	}
}
```

this becomes more time consuming and error prone as the complexity of the objects that you are mapping increases.

That is why you should use the **open-source** library `AutoMapper`.

## Using Auto-Mapper Step-by-Step

### Step1.

Install the package : Tools => NuGet Package Manager => Package manager Console. Click on the context menu that just popped up and write: Install-Package `AutoMapper`.

go to the project references (dependencies) section to see if the package is there.

### Step2.

Do not worry I will explain later.

create a new .`cs` file (SHIFT + F2) and write this:

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Provide all the Mapping Configuration
            var config = new MapperConfiguration(cfg =>
            {
                    //Configuring Employee and EmployeeDTO
                    cfg.CreateMap<Employee, EmployeeDTO>();
                    //Any Other Mapping Configuration ....
                });

            //Create an Instance of Mapper and return that Instance
            var mapper = new Mapper(config);
            return mapper;
        }
    }
}
```

What happen here is you are creating a `MapperConfiguration` instance
with a lambda method that has a `IMapperConfigurationExpression` as a parameter. This parameter allow you to` CreateMap()` between two objects: `<Source, Destination>` in this case:  `Employee, EmployeeDTO` and many other operations.

Then you are passing this `MapperConfiguration` instance to the `Mapper` constructor and returning the `Mapper` instance.

You need to remember that you can create only one `**MapperConfiguration` Instance Per `AppDomain`,** and it should be instantiated during the application Start-Up.


### Step3

Now , in the application's main file, create a `Mapper` instance with the method implemented at Step2 and map an Employee instance with an `EmployeeDTO` instance


```csharp
Employee emp1 = new Employee()
{
    Name = "Tiago",
    Department = "Engineering",
    Age = 22,
	    Address = "you fucking thought that i was going to put my real address here lmao!"
	
	};

// The mapping is happening here.
var empDTO = mapper.Map<Employee, EmployeeDTO>(emp1);
```

The `Map<source, destination>()` is able to infer the source from the it's argument's data type. So `var empDTO = mapper.Map<EmployeeDTO>(emp1);` is also valid.

## What If The Properties Have Different Names?

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Provide all the Mapping Configuration
            var config = new MapperConfiguration(cfg =>
            {
                //Configuring Employee and EmployeeDTO
                cfg.CreateMap<Employee, EmployeeDTO>()

                //Provide Mapping Configuration of FullName and Name Property
                .ForMember(dest => dest.FullName, act => act.MapFrom(src => src.Name))
                
                //Provide Mapping Dept of FullName and Department Property
                .ForMember(dest => dest.Dept, act => act.MapFrom(src => src.Department));

                //Any Other Mapping Configuration ....
            });

            //Create an Instance of Mapper and return that Instance
            var mapper = new Mapper(config);
            return mapper;
        }
    }
}
```

`cfg.CreateMap<Employee, EmployeeDTO>()` returns a `IMappingExpression<Employee, EmployeeDTO>` instance. The `IMappingExpression` instances can call  `ForMember()`. The first parameter of  `ForMember()` defines the destination's property to be mapped to the source's property. The second parameter is a generic Action delegate that takes a `IMemberConfigurationExpression` as a parameter, which allows you to call `MapFrom()` and define the source's property that will be used to map the destination's property.

## Advantages of `AutoMapper`

- **Reduces Repetitive Code:** AutoMapper eliminates the need to manually write repetitive code (like property-to-property assignment) to map properties from one object to another. This can significantly reduce the amount of code you have to maintain.
- **Improves Code Readability and Maintainability:** With less code to manage for object mapping, your codebase becomes cleaner and easier to maintain.
- **Consistency in Mapping:** AutoMapper ensures consistent mapping rules across the application, reducing the likelihood of errors that can occur in manual mappings.
- **Custom Mapping Capabilities:** While it automates simple mappings, AutoMapper also allows for custom configuration for complex mappings, giving you flexibility when needed.
- **Ease of Use:** AutoMapper is generally easy to set up and use, especially for straightforward mappings, making it a convenient tool for rapid development.
- **Reduces Human Error:** Manual object mapping is prone to errors, especially in large and complex projects. AutoMapper automates this process, thereby reducing the chance of mistakes.




## Disadvantages of `AutoMapper`

- **Performance Overhead:** AutoMapper can introduce performance overhead, especially if not configured properly. This can be a concern in high-performance or resource-limited environments.
- **Complexity in Debugging:** Debugging can become more challenging with AutoMapper, particularly when dealing with complex mappings or when the mapping configuration is not straightforward.
- **Learning Curve:** While AutoMapper is easy to use for basic scenarios, mastering its advanced features and best practices can require a significant time investment.
- **Overreliance on Convention:** AutoMapper relies heavily on naming conventions. You must write more custom configurations if your source and destination objects don’t follow a consistent naming convention.
- **Unnecessary Complexity for Simple Tasks:** For very simple object mapping scenarios, using AutoMapper might be overkill, and manual mapping could be more straightforward and efficient.


## When To Use and When Not To Use `AutoMapper`

You should consider using `AutoMapper` in C# under the following circumstances:

- **Large Projects with Complex Object Models:** In large-scale applications with complex domain models and Data Transfer Objects (DTOs), `AutoMapper` can save significant time and effort by automating the mapping process.
- **Projects with Repetitive Mapping Code:** If your project involves a lot of repetitive and tedious mapping code (like copying properties from one object to another), `AutoMapper` can help reduce this boilerplate code, making your codebase cleaner and easier to maintain.
- **When Consistency in Mapping Rules is Important:** `AutoMapper` helps maintain consistent mapping rules across your application. This is particularly useful in team environments or large projects where different developers might otherwise implement mappings inconsistently.
- **CRUD Operations in Layered Architecture:** In applications with a clear separation of concerns, such as MVC applications, `AutoMapper` is valuable for transforming data between layers (e.g., from data access objects to business logic objects or business logic objects to view models).
- **When You Need Customizable Mapping Logic:** `AutoMapper` is not just for straightforward mappings; it also allows for complex and custom mapping logic. This is useful when you have specific rules for how certain properties should be mapped or transformed.
- **In Applications Where Development Speed is a Priority:** Rapid application development can benefit from `AutoMapper`, as it speeds up writing and updating code involving object mappings.

However, there are scenarios where `AutoMapper` may not be the best choice`:`


- **For Simple Projects:** If your project is simple, with few mappings, or if the mappings are straightforward, the overhead of introducing a third-party library might not be justified.
- **Performance-Critical Applications:** Be cautious in performance-sensitive applications, as `AutoMapper` can introduce some overhead, especially if not configured optimally.
- **When Explicit Control is Preferred:** In cases where you need or prefer explicit control over every aspect of the mapping, or if you want to avoid the abstraction introduced by `AutoMapper`, manual mapping might be a better choice.


# Complex Mapping

Mapping properties of complex data types.

The `Employee` and `EmployeeDTO` classes now have a complex type property called Address:

```csharp


    
namespace AutoMapperDemo
{
	public class EmployeeDTO
    {
        public string Name { get; set; }
        public int Salary { get; set; }
        public string Department { get; set; }
        public AddressDTO Address { get; set; }
    }


    public class Employee
    {
        public string Name { get; set; }
        public int Salary { get; set; }
        public string Department { get; set; }
        public Address Address { get; set; }
    }

	public class AddressDTO

	{
		public string City { get; set; }
		public string State { get; set; }
		public string Country { get; set; }
	}
 
	 public class Address
    {
        public string City { get; set; }
        public string State { get; set; }
        public string Country { get; set; }
    }
	
}


```

So now we need to map `Address` object to `AddressDTO` object in the configurations:

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Provide all the Mapping Configuration
            var config = new MapperConfiguration(cfg => {
                //Configuring Address and AddressDTO
                cfg.CreateMap<Address, AddressDTO>();
                //Configuring Employee and EmployeeDTO
                cfg.CreateMap<Employee, EmployeeDTO>();
            });

            //Create an Instance of Mapper and return that Instance
            var mapper = new Mapper(config);
            return mapper;
        }
    }
}
```

Now When we Map an Employee instance to an `EmployeeDTO` instance we will not run into any problems.

## 2 Possible Problems

### Problem 1

The complex data type properties have different names (Address and EMPAddress for example).

map the property with the name of Address to the property with the name of EMPAddress in the `<Employee, EmployeeDTO>` map:

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Provide all the Mapping Configuration
            var config = new MapperConfiguration(cfg => {
                //Configuring Address and AddressDTO
                cfg.CreateMap<Address, AddressDTO>();

                //Configuring Employee and EmployeeDTO
                cfg.CreateMap<Employee, EmployeeDTO>()
                //Provide Mapping Information for AddressDTO and address
                .ForMember(dest => dest.AddressEMP, act => act.MapFrom(src => src.Address)); ;
            });

            //Create an Instance of Mapper and return that Instance
            var mapper = new Mapper(config);
            return mapper;
        }
    }
}
```

### Problem 2

`Address` and `AddressDTO` complex data types have properties with different names themselves:

```csharp
namespace AutoMapperDemo
{
	public class Address
    {
        public string City { get; set; }
        public string State { get; set; }
        public string Country { get; set; }
    }

    public class AddressDTO
    {
        public string EmpCity { get; set; }
        public string EmpState { get; set; }
        public string Country { get; set; }
    }
}
```

To solve it just map these properties to each other in the config of the `<Address, AddressDTO>` Map:

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Provide all the Mapping Configuration
            var config = new MapperConfiguration(cfg => {
                //Configuring Address and AddressDTO
                cfg.CreateMap<Address, AddressDTO>()
                .ForMember(dest => dest.EmpCity, act => act.MapFrom(src => src.City))
                .ForMember(dest => dest.EmpState, act => act.MapFrom(src => src.State));

                //Configuring Employee and EmployeeDTO
                cfg.CreateMap<Employee, EmployeeDTO>()
                //Provide Mapping Information for AddressDTO and address
                .ForMember(dest => dest.AddressEMP, act => act.MapFrom(src => src.Address));
            });

            //Create an Instance of Mapper and return that Instance
            var mapper = new Mapper(config);
            return mapper;
        }
    }
}
```



# Mapping Complex Types to Primitive Types

To map the Address complex property of the `Employee` class to three primitive properties of the `EmployeeDTO`
class we map the `Address` class properties to the `EmployeeDTO` primitive properties through the `<Employee, EmployeeDTO>` map.

```csharp
namespace AutoMapperDemo
{
    public class EmployeeDTO
    {
        public string Name { get; set; }
        public int Salary { get; set; }
        public string Department { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string Country { get; set; }
    }
    
	public class Employee
	{
		public string Name { get; set; }
		public int Salary { get; set; }
		public string Department { get; set; }
		public Address Address { get; set; }
	}

    public class Address
    {
        public string City { get; set; }
        public string State { get; set; }
        public string Country { get; set; }
    }
}
```


```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Provide all the Mapping Configuration
            var config = new MapperConfiguration(cfg => {
                //Configuring Employee and EmployeeDTO
                cfg.CreateMap<Employee, EmployeeDTO>()
                //Provide Mapping Information for City Property
               .ForMember(dest => dest.City, act => act.MapFrom(src => src.Address.City))
               //Provide Mapping Information for State Property
               .ForMember(dest => dest.State, act => act.MapFrom(src => src.Address.State))
               //Provide Mapping Information for Country Property
               .ForMember(dest => dest.Country, act => act.MapFrom(src => src.Address.Country));
            });
            
            //Create an Instance of Mapper Class and return that Instance
            var mapper = new Mapper(config);
            return mapper;
        }
    }
}
```

## Mapping Primitive Types to Complex Types

But what if the `Employee` class was the one with the 3 primitive properties and the `EmployeeDTO` had the complex property?

```csharp
namespace AutoMapperDemo
{
    public class EmployeeDTO
    {
        public string Name { get; set; }
        public int Salary { get; set; }
        public string Department { get; set; }
        public Address Address { get; set; }
    }
    public class Employee
    {
        public string Name { get; set; }
        public int Salary { get; set; }
        public string Department { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string Country { get; set; }
    }
}
```

For that we can create an new instance of the `Address` class where it's properties are initialized with values of the 3 primitive properties from the `Employee` class and map the `EmployeeDTO` complex property: Address to this new `Address` class instance:

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            var config = new MapperConfiguration(cfg => {
                cfg.CreateMap<Employee, EmployeeDTO>()
                .ForMember(dest => dest.Address, act => act.MapFrom(src => new Address()
                {
                    City = src.City,
                    State = src.State,
                    Country = src.Country
                }));
            });

            //Create an Instance of Mapper Class and return that Instance
            var mapper = new Mapper(config);
            return mapper;
        }
    }
}
```

# Auto Reverse Mapping
## What Is It?

Regular mappings are one directional, they map type A to type B. Auto Reverse mapping creates bi-directional mappings that maps type A to B and B to A.

## Implementation

We are going to make a bidirectional map `<Order, OrderDTO>`This are the classes we are going to work with:

```csharp
namespace AutoMapperDemo
{

    public class OrderDTO
    {
        public int OrderId { get; set; }
        public int NumberOfItems { get; set; }
        public int TotalAmount { get; set; }
        public int CustomerId { get; set; }
        public string Name { get; set; }
        public string Postcode { get; set; }
        public string MobileNo { get; set; }
    }
    public class Order
    {
        public int OrderNo { get; set; }
        public int NumberOfItems { get; set; }
        public int TotalAmount { get; set; }
        public Customer Customer { get; set; }
    }
    public class Customer
    {
        public int CustomerID { get; set; }
        public string FullName { get; set; }
        public string Postcode { get; set; }
        public string ContactNo { get; set; }
    }
}
```

The only new thing that you have not seen already is the `ReverseMap()` that is responsible for creating  the bi-directional map.

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Configuring AutoMapper
            var config = new MapperConfiguration(cfg => {
                //Mapping Order with OrderDTO
                cfg.CreateMap<Order, OrderDTO>()
                    //OrderId is different so map them using For Member
                    .ForMember(dest => dest.OrderId, act => act.MapFrom(src => src.OrderNo))

                    //Customer is a Complex type, so Map Customer to Simple type using For Member
                    .ForMember(dest => dest.Name, act => act.MapFrom(src => src.Customer.FullName))
                    .ForMember(dest => dest.Postcode, act => act.MapFrom(src => src.Customer.Postcode))
                    .ForMember(dest => dest.MobileNo, act => act.MapFrom(src => src.Customer.ContactNo))
                    .ForMember(dest => dest.CustomerId, act => act.MapFrom(src => src.Customer.CustomerID))
                    .ReverseMap(); //Making the Mapping Bi-Directional
            });

            //Creating the Mapper Instance
            var mapper = new Mapper(config);

            //returning the Mapper Instance
            return mapper;
        }
    }
}
```

Now you can map an new `Order` object to an existing `OrderDTO` object and vice-versa.

```csharp
using System;
namespace AutoMapperDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Step1: Initialize the Mapper
            var mapper = MapperConfig.InitializeAutomapper();

            //Step2: Create the Order Request
            Order OrderRequest = CreateOrderRequest();

            //Step3: Map the OrderRequest object with OrderDTO Object
            var orderDTOData = mapper.Map<Order, OrderDTO>(OrderRequest);
            //or
            //var orderDTOData = mapper.Map<OrderDTO>(OrderRequest);

            //Step4: Print the OrderDTO Data
            Console.WriteLine("After Mapping - OrderDTO Data");
            Console.WriteLine("OrderId : " + orderDTOData.OrderId);
            Console.WriteLine("NumberOfItems : " + orderDTOData.NumberOfItems);
            Console.WriteLine("TotalAmount : " + orderDTOData.TotalAmount);
            Console.WriteLine("CustomerId : " + orderDTOData.CustomerId);
            Console.WriteLine("Name : " + orderDTOData.Name);
            Console.WriteLine("Postcode : " + orderDTOData.Postcode);
            Console.WriteLine("MobileNo : " + orderDTOData.MobileNo);
            Console.WriteLine();

            //Step5: modify the OrderDTO data
            orderDTOData.OrderId = 10;
            orderDTOData.NumberOfItems = 20;
            orderDTOData.TotalAmount = 2000;
            orderDTOData.CustomerId = 5;
            orderDTOData.Name = "Smith";
            orderDTOData.Postcode = "12345";

            //Step6: AutoMapper Reverse Mapping
            mapper.Map(orderDTOData, OrderRequest);

            //Step7: Print Order Data
            Console.WriteLine("After Reverse Mapping - Order Data");
            Console.WriteLine("OrderNo : " + OrderRequest.OrderNo);
            Console.WriteLine("NumberOfItems : " + OrderRequest.NumberOfItems);
            Console.WriteLine("TotalAmount : " + OrderRequest.TotalAmount);
            Console.WriteLine("CustomerId : " + OrderRequest.Customer.CustomerID);
            Console.WriteLine("FullName : " + OrderRequest.Customer.FullName);
            Console.WriteLine("Postcode : " + OrderRequest.Customer.Postcode);
            Console.WriteLine("ContactNo : " + OrderRequest.Customer.ContactNo);
            Console.ReadLine();
        }

        private static Order CreateOrderRequest()
        {
            return new Order
            {
                OrderNo = 101,
                NumberOfItems = 3,
                TotalAmount = 1000,
                Customer = new Customer()
                {
                    CustomerID = 777,
                    FullName = "James Smith",
                    Postcode = "755019",
                    ContactNo = "1234567890"
                },
            };
        }
    }
}
```



## But What If  `Order` class has the primitive data types and `OrderDTO` has the complex data types?

```csharp
namespace AutoMapperDemo
{
    public class OrderDTO
    {
        public int OrderId { get; set; }
        public int NumberOfItems { get; set; }
        public int TotalAmount { get; set; }
        public Customer Customer { get; set; }
    }
    public class Order
    {
        public int OrderNo { get; set; }
        public int NumberOfItems { get; set; }
        public int TotalAmount { get; set; }
        public int CustomerId { get; set; }
        public string Name { get; set; }
        public string Postcode { get; set; }
        public string MobileNo { get; set; }
    }
}
```

We already know the answer to that questions from the **Mapping Complex Types to Primitive Types** part of this Obsidian note:

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Configuring AutoMapper
            var config = new MapperConfiguration(cfg => {
                //Mapping Order with OrderDTO
                cfg.CreateMap<Order, OrderDTO>()
                    .ForMember(dest => dest.OrderId, act => act.MapFrom(src => src.OrderNo))
                    .ForMember(dest => dest.Customer, act => act.MapFrom(src => new Customer()
                    {
                        CustomerID = src.CustomerId,
                        FullName = src.Name,
                        Postcode = src.Postcode,
                        ContactNo = src.MobileNo
                    }))
                    .ReverseMap() //This will make the Mapping as Bi-Directional
                    //Mappping Complex Type to Primitive Type Properties
                    .ForMember(dest => dest.CustomerId, act => act.MapFrom(src => src.Customer.CustomerID))
                    .ForMember(dest => dest.Name, act => act.MapFrom(src => src.Customer.FullName))
                    .ForMember(dest => dest.MobileNo, act => act.MapFrom(src => src.Customer.ContactNo))
                    .ForMember(dest => dest.Postcode, act => act.MapFrom(src => src.Customer.Postcode));
            });

            //Creating the Mapper Instance
            var mapper = new Mapper(config);

            //returning the Mapper Instance
            return mapper;
        }
    }
}
```

# Conditional Mapping

Conditional Mapping allows you to define conditions that must be met to allow the destination property to be mapped with the value of the source property.

Conditional Mapping is mostly achieved with the `Condition()` method, but you can also use ?: (syntactic sugar if else statement) or other things to create conditions.

These are the classes being used to demonstrate:

```csharp
namespace AutoMapperDemo
{
    public class ProductDTO
    {
        public int ProductID { get; set; }
        public string ItemName { get; set; }
        public int ItemQuantity { get; set; }
        public int Amount { get; set; }
    }
    public class Product
    {
        public int ProductID { get; set; }
        public string Name { get; set; }
        public string OptionalName { get; set; }
        public int Quantity { get; set; }
        public int Amount { get; set; }
    }
}
```

We are going to create a `<Product, ProductDTO>` map with the following conditions:

1. If the Name Start with A then Map the Name Value else Map the `OptionalName `value
2. Map the quantity value if its greater than 0
3. Map the amount value if its greater than 100


```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Configuring AutoMapper
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<Product, ProductDTO>()
                    //If the Name Start with A then Map the Name Value else Map the OptionalName value
                    .ForMember(dest => dest.ItemName, act => act.MapFrom(src =>
                        (src.Name.StartsWith("A") ? src.Name : src.OptionalName)))

                    //Map the quantity value if its greater than 0
                    .ForMember(dest => dest.ItemQuantity, act => act.Condition(src => (src.Quantity > 0)))

                    //Map the amount value if its greater than 100
                    .ForMember(dest => dest.Amount, act => act.Condition(src => (src.Amount > 100)));
            });

            //Creating the Mapper Instance
            var mapper = new Mapper(config);

            //returning the Mapper Instance
            return mapper;
        }
    }
}
```

Now the Amount property from the `Product` instance will only be mapped to the Amount property of the `ProductDTO` instance if the property holds a value greater than 100 and so on, and so on


# The Ignore Method

By default, `AutoMapper `tries to map all the properties from the source to the destination type when the names of both the source and destination type are the same. If you want some properties not to map with the destination type property, use the Ignore() method to ignore specific properties during the mapping process.

These are the classes we are working with:

```csharp
namespace AutoMapperDemo
{
    public class EmployeeDTO
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string Email { get; set; }
    }
    public class Employee
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string Email { get; set; }
    }
}
```

Goal: map `EmployeeDTO` instances to `Employee` instances without mapping the Address property of the `Employee` class to the `EmployeedDTO` Address property. 

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Configuring AutoMapper
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<Employee, EmployeeDTO>()

                    //Ignoring the Address property of the destination type
                    .ForMember(dest => dest.Address, act => act.Ignore());
            });

            //Creating the Mapper Instance
            var mapper = new Mapper(config);

            //returning the Mapper Instance
            return mapper;
        }
    }
}
```

## Creating Extension Methods That Ignore Properties in Order to Reduce Workload

Easy enough. So let's make it harder.

We are going to create an extension method in a static class that will ignore multiple properties for us if called from a `IMappingExpression` interface instance.

### Step1

Create a class new file and implement a class that is derived from `System.Attribute`.

example:
filename: `NoMapAttribute.cs`

```csharp
namespace AutoMapperDemo
{
    public class NoMapAttribute : System.Attribute
    {
    }
}
```

### Step 2

Decorate the properties of the source object's class that you want to ignore with the `NoMap` attribute:

```csharp
namespace AutoMapperDemo
{
    public class Employee
    {
        public int ID { get; set; }
        public string Name { get; set; }
        [NoMap]
        public string Address { get; set; }
        [NoMap]
        public string Email { get; set; }
    }
}
```

### **Step3: Creating an Extension Method**

Now here is the hard part. We are going to create a generic extension method in a static class that we can call in the `MapperConfiguration` instance to ignore the properties that we decorated with the `NoMap` attribute.

```csharp
using AutoMapper;
using System.ComponentModel;
namespace AutoMapperDemo
{
    //Extension Method: 
    //The Class Should Be Static
    //Method Should Be Static
    //First Parameter is the class which which we can access the Method
    public static class IgnoreNoMapExtensions
    {
        //Method is Generic and Hence we can use with any TSource and TDestination Type
        public static IMappingExpression<TSource, TDestination> IgnoreNoMap<TSource, TDestination>(
            this IMappingExpression<TSource, TDestination> expression)
        {
            //Fetching Type of the TSource
            var sourceType = typeof(TSource);

            //Fetching All Properties of the Source Type using GetProperties() method
            foreach (var property in sourceType.GetProperties())
            {
                //Get the Property Name
                PropertyDescriptor descriptor = TypeDescriptor.GetProperties(sourceType)[property.Name];

                //Check if Property is Decorated with the NoMapAttribute
                NoMapAttribute attribute = (NoMapAttribute)descriptor.Attributes[typeof(NoMapAttribute)];
                if (attribute != null)
                {
                    //If Property is Decorated with NoMap Attribute, call the Ignore Method
                    expression.ForMember(property.Name, opt => opt.Ignore());
                }  
            }
            return expression;
        }
    }
}
```


### **Step4: Using the `IgnoreNoMap` Extension Method in Mapper Configuration**

The last thing we need to do is call the extension method in the `MapperConfiguration` instance

```csharp
using AutoMapper;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Configuring AutoMapper
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<Employee, EmployeeDTO>()
                .IgnoreNoMap();
            });

            //Creating the Mapper Instance
            var mapper = new Mapper(config);

            //returning the Mapper Instance
            return mapper;
        }
    }
}
```


## **Use Cases for Ignore Method**

- **Property Mismatches:** When some properties that do not exist in the destination type exist in the source type the will need to be ignored very often.
- **Read-Only Properties:** read-only properties of the destination type cannot have their values altered and therefore need to be ignored.
- **Conditional Mapping:** In scenarios where certain properties should be mapped or ignored based on specific conditions.
- **Performance Optimization:** Ignoring properties that are expensive to map or are not needed in the destination object can improve performance. Not mapping properties that are irrelevant for your purposes and or properties that are expensive (complex data type properties) can significantly improve performance.



# How to Map Fixed and Dynamic Values

You will use `MapFrom()` to map destination properties to fixed or dynamic values.

The classes we are working with:

```csharp
using System;
namespace AutoMapperDemo
{
using System;
    public class TemporaryAddress
    {
        public string Name { get; set; }
        public string Address { get; set; }
        public string CreatedBy { get; set; }
        public DateTime? CreatedOn { get; set; }
    }

	public class PermanentAddress
    {
        public string Name { get; set; }
        public string Address { get; set; }
        public string CreatedBy { get; set; }
        public DateTime? CreatedOn { get; set; }
    }
}
```

Goal: create a `<PermanentAddress,TemporaryAddress>` map that will always hard code the string "System" to the `CreatedBy` property of `TemporaryAddress` object and map the `CreatedOn` property with a dynamic value which is the current `DateTime`. 

**fixed values are values that are non-changing and fixed values. They decided at compile time.
dynamic values are values that are constantly changing and can only be pin pointed at run time.
**

Solution:
```csharp
using AutoMapper;
using System;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Configuring AutoMapper
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<PermanentAddress, TemporaryAddress>()
                    //Using MapFrom Method to Store Static or Hard-Coded Value in a Destination Property
                    .ForMember(dest => dest.CreatedBy, act => act.MapFrom(src => "System"))

                    //Before AutoMapper 8.0, to Store Static Value use the UseValue() method
                    //.ForMember(dest => dest.CreatedBy, act => act.UseValue("System"))

                   //Using MapFrom Method to Store Dynamic Value in a Destination Property
                   //Here, we are calling GetCurrentDateTime method which will return a dynamic value
                   .ForMember(dest => dest.CreatedOn, opt => opt.MapFrom(src => GetCurrentDateTime()))

                    ////Before AutoMapper 8.0, To Store Dynamic value use ResolveUsing() method
                    //.ForMember(dest => dest.CreatedBy, act => act.ResolveUsing(src =>
                    //{
                    //    return DateTime.Now;
                    //}))

                   .ReverseMap();
            });
            
            //Creating the Mapper Instance
            var mapper = new Mapper(config);

            //returning the Mapper Instance
            return mapper;
        }

        //Method to return Dynamic Value
        public static DateTime GetCurrentDateTime()
        {
            //Write the Logic to Get Dynamic value
            return DateTime.Now;
        }
    }
}
```


# Null Substitution

Just use `NullSubstute()` and pass the value that you want to substitute null as an argument to this method. Simple enough.

This uses the classes from **How to Map Fixed and Dynamic Values**.

```csharp
using AutoMapper;
using System;
namespace AutoMapperDemo
{
    public class MapperConfig
    {
        public static Mapper InitializeAutomapper()
        {
            //Configuring AutoMapper
            var config = new MapperConfiguration(cfg =>
            {
                cfg.CreateMap<PermanentAddress, TemporaryAddress>()
                    //Using MapFrom Method to Store Static or Hard-Coded Value in a Destination Property
                    .ForMember(dest => dest.CreatedBy, act => act.MapFrom(src => "System"))
                    
                   //Using MapFrom Method to Store Dynamic Value in a Destination Property
                   //Here, we are calling GetCurrentDateTime method which will return a dynamic value
                   .ForMember(dest => dest.CreatedOn, opt => opt.MapFrom(src => GetCurrentDateTime()))

                   //Storing NA in the destination Address Property, if Source Address is NULL
                   .ForMember(dest => dest.Address, act => act.NullSubstitute("N/A"))

                   .ReverseMap();
            });
            
            //Creating the Mapper Instance
            var mapper = new Mapper(config);

            //returning the Mapper Instance
            return mapper;
        }

        //Method to return Dynamic Value
        public static DateTime GetCurrentDateTime()
        {
            //Write the Logic to Get Dynamic value
            return DateTime.Now;
        }
    }
}
```

Now if the source object has it's Address property == null, the destination object Address property will hold the value: "N/A"