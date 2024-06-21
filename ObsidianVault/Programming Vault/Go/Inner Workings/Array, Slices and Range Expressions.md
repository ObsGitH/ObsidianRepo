
For each type _T_ and each non-negative integer constant _n_, there is an [array type](https://en.wikipedia.org/wiki/Array_data_type "Array data type") denoted [_n_]_T_; arrays of differing lengths are thus of different types. [Dynamic arrays](https://en.wikipedia.org/wiki/Dynamic_array "Dynamic array") are available as "slices", denoted []_T_ for some type _T_.  Slices have a length(){amount of memory used by the type} and a _capacity_(){amount of memory given by the memory allocator to the type}  as part of their definition specifying when new memory needs to be allocated to expand the array. Several slices may share their underlying memory.
- - **Efficiency:** Knowing the capacity helps in optimizing memory allocation and performance when working with slices, especially in scenarios where you are appending a large number of elements.
- **Avoiding Reallocation:** By preallocating a sufficient capacity when possible, you can avoid frequent reallocations and improve performance.

Certainly! In Go, the `range` keyword is used in `for` loops to iterate over elements in various data structures such as arrays, slices, maps, strings, and channels. The `range` expression returns both the index (or key) and the value for each iteration. Let's delve into how `range` expressions work with different data types:

### 1. Range with Arrays and Slices

When iterating over arrays or slices using `range`, you get the index and value of each element:

```go
package main

import "fmt"

func main() {
    nums := []int{2, 3, 5}

    // Iterate over slice elements
    for index, value := range nums {
        fmt.Printf("Index: %d, Value: %d\n", index, value)
    }
}
```

Output:
```
Index: 0, Value: 2
Index: 1, Value: 3
Index: 2, Value: 5
```

In this example:
- `index` is the index of the current element.
- `value` is the value at that index in the slice.

### 2. Range with Maps

With maps, `range` iterates over key-value pairs:

```go
package main

import "fmt"

func main() {
    colors := map[string]string{
        "red":   "#ff0000",
        "green": "#00ff00",
        "blue":  "#0000ff",
    }

    // Iterate over map key-value pairs
    for key, value := range colors {
        fmt.Printf("Key: %s, Value: %s\n", key, value)
    }
}
```

Output:
```
Key: red, Value: #ff0000
Key: green, Value: #00ff00
Key: blue, Value: #0000ff
```

Here:
- `key` is the key of the current map entry.
- `value` is the value associated with that key.

### 3. Range with Strings

When using `range` with a string, it iterates over Unicode code points:

```go
package main

import "fmt"

func main() {
    greeting := "Hello, 世界"

    // Iterate over string runes (Unicode code points)
    for index, runeValue := range greeting {
        fmt.Printf("Index: %d, Rune: %c\n", index, runeValue)
    }
}
```

Output:
```
Index: 0, Rune: H
Index: 1, Rune: e
Index: 2, Rune: l
Index: 3, Rune: l
Index: 4, Rune: o
Index: 5, Rune: ,
Index: 6, Rune:  
Index: 7, Rune: 世
Index: 10, Rune: 界
```

In this case:
- `index` is the byte index of the current rune.
- `runeValue` is the actual Unicode code point (rune) at that position.

### 4. Range with Channels

When iterating over a channel with `range`, it receives values from the channel until it's closed:

```go
package main

import "fmt"

func main() {
    nums := make(chan int)

    // Producer goroutine sending data to channel
    go func() {
        defer close(nums)
        nums <- 1
        nums <- 2
        nums <- 3
    }()

    // Consumer iterating over channel
    for num := range nums {
        fmt.Println("Received:", num)
    }
}
```

Output:
```
Received: 1
Received: 2
Received: 3
```

Here:
- The `for num := range nums` construct receives values from the `nums` channel until it's closed.
- This is a common pattern for concurrent programming in Go, where goroutines communicate via channels.

### Notes on Range

- **Value Only:** If you omit the index by using `_` instead, you'll receive only the values (or keys in maps).

  ```go
  for _, value := range nums {
      fmt.Println(value)
  }
  ```

- **Modifying Elements:** If you modify elements of a slice or array within a `range` loop, the modifications affect the original slice or array.

- **Range with String and Index:** Using `range` with strings provides a convenient way to iterate over Unicode characters (runes) even if they occupy multiple bytes.

### Conclusion

Go's `range` expression is a versatile and powerful feature that simplifies iterating over collections like arrays, slices, maps, strings, and channels. It provides a clean and efficient way to work with data structures, enhancing readability and maintainability in Go programs.