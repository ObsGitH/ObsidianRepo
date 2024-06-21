Interfaces can embed other interfaces with the effect of creating a combined interface that is satisfied by exactly the types that implement the embedded interface and any methods that the newly defined interface adds.

Besides calling methods via interfaces, Go allows converting interface values to other types with a run-time type check. The language constructs to do so are the _type assertion_ which checks against a single potential type:

```go 

var shp Shape = Square{5}
square, ok := shp.(Square) // Asserts Square type on shp, should work
if ok {
	fmt.Printf("%#v\n", square)
} else {
	fmt.Println("Can't print shape as Square")
}

```
and the _type switch_,checks against multiple types
```go
func (sq Square) Diagonal() float64 { return sq.side * math.Sqrt2 }
func (c Circle) Diameter() float64 { return 2 * c.radius }

func LongestContainedLine(shp Shape) float64 {
	switch v := shp.(type) {
	case Square:
		return v.Diagonal() // Or, with type assertion, shp.(Square).Diagonal()
	case Circle:
		return v.Diameter() // Or, with type assertion, shp.(Circle).Diameter()
	default:
		return 0 // In practice, this should be handled with errors
	}
}
```

The _empty interface_ `interface{}` is an important base case because it can refer to an item of _any_ concrete type. It is similar to the Object class in [Java](https://en.wikipedia.org/wiki/Java_(programming_language) "Java (programming language)") or [C#](https://en.wikipedia.org/wiki/C_Sharp_(programming_language) "C Sharp (programming language)") and is satisfied by any type, including built-in types like int.
	- empty interface is equivalent to the object type in c# in terms of purpose and applicability. 
 Code using the empty interface cannot simply call methods (or built-in operators) on the referred-to object, but it can store the `interface{}` value, try to convert it to a more useful type via a type assertion or type switch, or inspect it with Go's `reflect` package. Because `interface{}` can refer to any value, it is a limited way to escape the restrictions of static typing, like `void*` in C but with additional run-time type checks.

!!! READ WITH ATTENTION THIS IS ACTUALLY HUGE !!!
The `interface{}` type can be used to model structured data of any arbitrary schema in Go, such as [JSON](https://en.wikipedia.org/wiki/JSON "JSON") or [YAML](https://en.wikipedia.org/wiki/YAML "YAML") data, by representing it as a `map[string]interface{}` (map of string to empty interface). This recursively describes data in the form of a dictionary with string keys and values of any type.

Interface values are implemented using pointer to data and a second pointer to run-time type information.  Like some other types implemented using pointers in Go, interface values are `nil` if uninitialized