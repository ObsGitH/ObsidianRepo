
Go uses an [interface](https://en.wikipedia.org/wiki/Protocol_(object-oriented_programming) "Protocol (object-oriented programming)") system in place of [virtual inheritance](https://en.wikipedia.org/wiki/Virtual_inheritance "Virtual inheritance"), and type embedding instead of non-virtual inheritance.


Composition reminds me of the has a relationship learned in OOP.  is a  (Parent child relationship) vs has a (Object has an instance of another object).

As long you encapsulate the mutable state of every type within itself, you will probably have a very good, error free time programming in go.


Go promotes composition over inheritance, and it uses interfaces to achieve polymorphism. This approach simplifies the language and avoids some of the complexities and pitfalls associated with inheritance.

### Composition in Go

In Go, you create complex types by combining simple types. This is known as composition. Instead of creating a class hierarchy, you compose types with the desired behavior.

#### Example of Composition

Here's an example to illustrate composition in Go:

```go 
package main

import "fmt"

// Basic type
type Person struct {
    Name string
}

// Another basic type
type Employee struct {
    Person
    EmployeeID int
}

func main() {
    emp := Employee{
        Person: Person{Name: "John Doe"},
        EmployeeID: 1234,
    }

    fmt.Println("Employee Name:", emp.Name)
    fmt.Println("Employee ID:", emp.EmployeeID)
}

```

In this example, `Employee` composes a `Person` type. The `Employee` type embeds the `Person` type, allowing it to inherit the fields and methods of `Person`.

### Interfaces in Go

Interfaces in Go provide a way to define behavior. A type implements an interface by implementing its methods. This is similar to polymorphism in traditional object-oriented programming.

#### Example of Interfaces

Here's an example using interfaces in Go:

```go 
package main

import "fmt"

// Defining an interface
type Speaker interface {
    Speak() string
}

// Defining a type that implements the interface
type Dog struct {
    Name string
}

// Implementing the interface method
func (d Dog) Speak() string {
    return "Woof!"
}

func main() {
    var s Speaker
    s = Dog{Name: "Buddy"}

    fmt.Println(s.Speak())
}

```
In this example, the `Dog` type implements the `Speaker` interface by defining the `Speak` method. Any type that implements the `Speak` method is considered to implement the `Speaker` interface.

### Key Differences from Traditional Inheritance

1. **No Class Hierarchies:**
    
    - Go does not have classes or class hierarchies. It uses struct types to define data and methods.
2. **Composition Over Inheritance:**
    
    - Instead of inheriting behavior and data from a base class, Go encourages composing types with the desired functionality.
3. **Interfaces for Polymorphism:**
    
    - Interfaces in Go provide a way to define and enforce behavior. Types implement interfaces implicitly by implementing their methods.
4. **Simplicity:**
    
    - Go's approach avoids some of the complexities and pitfalls of inheritance, such as the diamond problem and deep class hierarchies.

### Practical Example

Here’s a more practical example showing how you might use composition and interfaces to create reusable components:

```go
package main

import "fmt"

// Defining a Printer interface
type Printer interface {
    Print() string
}

// Defining a struct that implements the Printer interface
type ConsolePrinter struct {
    Message string
}

// Implementing the Print method
func (cp ConsolePrinter) Print() string {
    return cp.Message
}

// Another struct that uses composition
type Document struct {
    Title string
    Printer
}

func main() {
    doc := Document{
        Title: "My Document",
        Printer: ConsolePrinter{Message: "Hello, World!"},
    }

    fmt.Println("Document Title:", doc.Title)
    fmt.Println("Document Content:", doc.Print())
}

```

In this example, `Document` composes the `Printer` interface, allowing it to use any type that implements the `Printer` interface for printing content.

### Conclusion

 By using composition, you can build complex types by combining simpler ones, and by using interfaces, you can define and enforce behavior in a flexible way.

# Common Patterns and Techniques for Composition

### Type Embedding

#### Example of Type Embedding

Here’s a simple example of type embedding in Go:

```go
package main

import "fmt"

// Basic type
type Person struct {
    Name string
    Age  int
}

// Another basic type
type Employee struct {
    Person     // Embedded type
    EmployeeID int
}

func main() {
    emp := Employee{
        Person:    Person{Name: "John Doe", Age: 30},
        EmployeeID: 1234,
    }

    // Accessing fields of the embedded type directly
    fmt.Println("Name:", emp.Name)
    fmt.Println("Age:", emp.Age)
    fmt.Println("Employee ID:", emp.EmployeeID)
}

```

In this example, the `Employee` struct embeds the `Person` struct. This means that an `Employee` instance has all the fields of `Person` and can access them directly.

### Method Promotion

When a type embeds another type, it not only inherits the fields of the embedded type but also its methods. This is known as method promotion.

#### Example of Method Promotion

Consider the following example:

```go
package main

import "fmt"

// Basic type with a method
type Person struct {
    Name string
}

func (p Person) Greet() string {
    return "Hello, my name is " + p.Name
}

// Another type that embeds Person
type Employee struct {
    Person
    EmployeeID int
}

func main() {
    emp := Employee{
        Person:    Person{Name: "Alice"},
        EmployeeID: 5678,
    }

    // Calling a method of the embedded type
    fmt.Println(emp.Greet()) // Output: Hello, my name is Alice
    fmt.Println("Employee ID:", emp.EmployeeID)
}

```

In this example, the `Employee` struct embeds the `Person` struct, and the `Greet` method of `Person` is promoted to `Employee`. This allows an `Employee` instance to call the `Greet` method directly.

### Practical Example

Here’s a more complex example demonstrating type embedding and interfaces together:

```go
package main

import "fmt"

// Printer interface
type Printer interface {
    Print() string
}

// Person type
type Person struct {
    Name string
}

func (p Person) Print() string {
    return "Name: " + p.Name
}

// Employee type embedding Person and implementing Printer
type Employee struct {
    Person
    EmployeeID int
}

func (e Employee) Print() string {
    return fmt.Sprintf("Name: %s, Employee ID: %d", e.Name, e.EmployeeID)
}

func main() {
    emp := Employee{
        Person:    Person{Name: "Bob"},
        EmployeeID: 7890,
    }

    fmt.Println(emp.Print()) // Output: Name: Bob, Employee ID: 7890
}

```

In this example, the `Employee` type embeds the `Person` type and also implements the `Printer` interface. The `Print` method in `Employee` overrides the `Print` method in `Person`, demonstrating polymorphism through interfaces and embedding.

### Conclusion

Type embedding in Go is a form of composition that allows one type to include and reuse the fields and methods of another type. It simplifies code reuse and promotes flexibility, aligning with Go’s preference for composition over inheritance. This approach helps avoid the complexities and pitfalls associated with traditional inheritance hierarchies while providing a powerful mechanism for building complex types from simpler ones.


In addition to type embedding, which is a primary form of composition in Go, here are other common patterns and techniques used for composition:

1. **Struct Fields:**
    
    - Simply defining one struct within another struct and accessing its fields directly.
    
```go
type Address struct {
    Street  string
    City    string
    Country string
}

type Person struct {
    Name    string
    Age     int
    Address Address  // Embedding Address struct as a field
}

```
    
2. **Interfaces:**
    
    - Defining interfaces and implementing them in different structs to achieve polymorphism and provide common behaviors.
    
```go
type Shape interface {
    Area() float64
}

type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

type Rectangle struct {
    Width  float64
    Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

```
    
3. **Function Types:**
    
    - Declaring functions as types and using them as fields in structs to define behavior.
    
```go
type Handler func(data []byte) error

type Service struct {
    HandleRequest Handler
}

```
    
4. **Channels:**
    
    - Using channels to communicate between goroutines and coordinate concurrent operations.
    
```go
type Worker struct {
    Jobs chan Job
}

```
    
5. **Composition via Methods:**
    
 defining methods on structs that interact with other types (often through struct fields), encapsulating functionality related to those types. This approach allows for clear separation of concerns and modularization of behavior.

 Struct Definitions

1. **Database Struct:**
   - Represents a database connection.

   ```go
   type Database struct {
       connection *sql.DB
   }
   ```

   - Here, `Database` is a struct that contains a field `connection` of type `*sql.DB`, which represents a connection to a SQL database.

2. **Query Method:**
   - Defines a method on `Database` to perform database queries.

   ```go
   func (db *Database) Query(query string) ([]Record, error) {
       // Query implementation
   }
   ```

   - The `Query` method is associated with the `Database` struct using the receiver `(db *Database)`. It allows executing SQL queries on the database and returning a slice of `Record` and an error.

3. **Repository Struct:**
   - Represents a repository that uses `Database` to fetch records.

   ```go
   type Repository struct {
       db Database
   }
   ```

   - The `Repository` struct embeds (or contains) a `Database` instance as a field. This is a form of composition where `Repository` has a `Database` to delegate database operations to.

4. **FindByID Method:**
   - Defines a method on `Repository` to fetch a record by ID using the embedded `Database`.

   ```go
   func (repo *Repository) FindByID(id int) (Record, error) {
       // Use db.Query to fetch record
   }
   ```

   - The `FindByID` method is associated with the `Repository` struct. It utilizes the `Query` method of the embedded `Database` (`repo.db`) to execute a query and retrieve a specific record based on the given `id`.

Benefits of Composition via Methods

- **Encapsulation:** Methods encapsulate functionality related to the struct they belong to (`Database` or `Repository`), promoting code organization and clarity.
  
- **Separation of Concerns:** `Database` focuses on database connectivity and operations, while `Repository` focuses on higher-level data access methods.

- **Code Reuse:** By embedding `Database` in `Repository`, the `Repository` struct inherits the methods of `Database`, allowing reuse of database-related functionality without repeating code.

Practical Use Case

In a real-world scenario, the `Database` struct might handle low-level operations like establishing connections, managing transactions, and executing queries. Meanwhile, the `Repository` struct might provide higher-level methods like `FindByID`, `FindByUsername`, etc., which utilize the `Database` methods to interact with the database and return specific data.

```go
package main

import (
    "database/sql"
    "fmt"
)

type Record struct {
    ID   int
    Name string
    // other fields
}

type Database struct {
    connection *sql.DB
}

func (db *Database) Query(query string) ([]Record, error) {
    // Simulated query execution; replace with actual SQL operations
    fmt.Println("Executing query:", query)
    
    // Simulated result
    records := []Record{
        {ID: 1, Name: "John"},
        {ID: 2, Name: "Alice"},
        // more records
    }
    
    return records, nil
}

type Repository struct {
    db Database
}

func (repo *Repository) FindByID(id int) (Record, error) {
    query := fmt.Sprintf("SELECT * FROM records WHERE id = %d", id)
    results, err := repo.db.Query(query)
    if err != nil {
        return Record{}, err
    }
    if len(results) == 0 {
        return Record{}, fmt.Errorf("record with ID %d not found", id)
    }
    return results[0], nil
}

func main() {
    db := Database{connection: nil} // initialize with actual database connection
    repo := Repository{db: db}
    
    record, err := repo.FindByID(2)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    
    fmt.Println("Found record:", record)
}
```

In this example:

- `Database` struct handles the `Query` method to execute SQL queries.
- `Repository` struct embeds `Database` and provides `FindByID` method to fetch a record by ID.

This composition via methods pattern in Go leverages struct embedding and method association to create modular and reusable components, enhancing maintainability and scalability of your codebase.
    
6. **Higher-Order Functions:**
    
    - Using functions that take other functions as parameters or return functions to build modular and flexible behavior.
```go
type Transformer func(data []byte) ([]byte, error)

func ComposeTransformers(transformers ...Transformer) Transformer {
    return func(data []byte) ([]byte, error) {
        for _, t := range transformers {
            newData, err := t(data)
            if err != nil {
                return nil, err
            }
            data = newData
        }
        return data, nil
    }
}

```
    

These patterns and techniques in Go allow developers to compose complex behaviors and structures from simpler components, promoting code reuse, modularity, and maintainability. Each approach has its strengths and is chosen based on the specific requirements of the application or system being developed.
