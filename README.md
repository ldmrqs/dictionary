# Go (Golang) Dictionary

A complete reference of Go commands in a direct and straightforward way.

## Table of Contents

- [Basic Commands](#basic-commands)
- [Variable Declaration](#variable-declaration)
- [Data Types](#data-types)
- [Control Structures](#control-structures)
- [Functions](#functions)
- [Arrays and Slices](#arrays-and-slices)
- [Maps](#maps)
- [Structs](#structs)
- [Pointers](#pointers)
- [Interfaces](#interfaces)
- [Goroutines and Concurrency](#goroutines-and-concurrency)
- [Channels](#channels)
- [Error Handling](#error-handling)
- [Packages and Imports](#packages-and-imports)
- [Methods](#methods)
- [Operators](#operators)
- [Reserved Words](#reserved-words)
- [Built-in Functions](#built-in-functions)

---

## Basic Commands

Essential commands to get started:

| Command | Meaning |
|---------|---------|
| `package` | define file package |
| `import` | import packages/libraries |
| `func` | define function |
| `main` | main function (entry point) |
| `fmt.Println` | print line to screen |
| `fmt.Printf` | print with formatting |
| `fmt.Scan` | read user input |
| `return` | return value from function |

**Example:**
```go
package main                    // define package

import "fmt"                    // import fmt package

func main() {                   // main function
    fmt.Println("Hello, world!") // print to screen
    
    var name string
    fmt.Scan(&name)             // read user input
    fmt.Printf("Hello, %s!\n", name) // print with formatting
}
```

---

## Variable Declaration

Ways to create and use variables:

| Command | Meaning |
|---------|---------|
| `var` | declare variable explicitly |
| `:=` | declare and assign value (type inference) |
| `const` | declare constant |
| `iota` | automatic counter for constants |

**Example:**
```go
// Explicit declaration
var age int = 25              // declare explicit variable
var name string               // declare without initial value

// Declaration with inference
x := 10                      // declare and assign (inference)
y := "text"                  // Go infers it's a string

// Constants
const PI = 3.14159           // declare constant
const (
    Monday = iota            // 0 (automatic counter)
    Tuesday                  // 1
    Wednesday                // 2
)
```

---

## Data Types

Main Go types:

| Type | Meaning |
|------|---------|
| `int` | integer number |
| `int8`, `int16`, `int32`, `int64` | integers with specific size |
| `uint` | unsigned integer number |
| `float32`, `float64` | decimal number |
| `string` | text |
| `bool` | true or false |
| `byte` | equivalent to uint8 |
| `rune` | equivalent to int32 (Unicode character) |
| `interface{}` | any type |

**Example:**
```go
var integer int = 42           // integer number
var decimal float64 = 3.14     // decimal number
var text string = "Go"         // text
var boolean bool = true        // boolean
var letter rune = 'A'          // Unicode character
var anything interface{} = "can be anything"
```

---

## Control Structures

To control program flow:

| Command | Meaning |
|---------|---------|
| `if` | if (condition) |
| `else` | else |
| `else if` | else if |
| `for` | loop (only loop type in Go) |
| `range` | iterate over arrays, slices, maps |
| `switch` | choose between multiple options |
| `case` | specific case in switch |
| `default` | default case in switch |
| `break` | exit loop or switch |
| `continue` | skip to next iteration |

**Example:**
```go
// If/else
age := 18
if age >= 18 {               // if age greater than or equal to 18
    fmt.Println("Adult")
} else {                     // else
    fmt.Println("Minor")
}

// For loops
for i := 0; i < 5; i++ {     // traditional loop
    fmt.Println(i)
}

numbers := []int{1, 2, 3, 4, 5}
for i, num := range numbers { // iterate with range
    fmt.Printf("Index: %d, Value: %d\n", i, num)
}

// Switch
day := "monday"
switch day {                 // choose between options
case "monday":              // specific case
    fmt.Println("Start of week")
case "friday":
    fmt.Println("End of week")
default:                    // default case
    fmt.Println("Mid week")
}
```

---

## Functions

To organize and reuse code:

| Command | Meaning |
|---------|---------|
| `func` | define function |
| `return` | return values |
| `defer` | defer execution until end of function |
| `panic` | stop program with error |
| `recover` | recover from panic |

**Example:**
```go
// Simple function
func add(a, b int) int {       // define function
    return a + b               // return value
}

// Function with multiple returns
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

// Function with defer
func deferExample() {
    defer fmt.Println("This executes last") // defer execution
    fmt.Println("This executes first")
    fmt.Println("This executes second")
}

// Using functions
result := add(5, 3)           // 8
quotient, err := divide(10, 2) // 5.0, nil
```

---

## Arrays and Slices

To work with lists of data:

| Command | Meaning |
|---------|---------|
| `[n]type` | array with fixed size |
| `[]type` | slice (dynamic array) |
| `make` | create slice with specific size |
| `append` | add elements to slice |
| `len` | get length |
| `cap` | get capacity |
| `copy` | copy elements between slices |

**Example:**
```go
// Arrays (fixed size)
var numbers [5]int             // array with 5 integers
numbers[0] = 10               // assign value

// Slices (dynamic size)
var list []int                // empty slice
list = append(list, 1, 2, 3)  // add elements

// Create slice with make
slice := make([]int, 5)       // slice with 5 zeros
slice2 := make([]int, 3, 10)  // length 3, capacity 10

// Slice operations
fmt.Println(len(list))        // length: 3
fmt.Println(cap(list))        // capacity

// Slice slicing
part := list[1:3]             // elements from index 1 to 2
```

---

## Maps

To work with key-value pairs:

| Command | Meaning |
|---------|---------|
| `map[type]type` | declare map |
| `make(map[type]type)` | create empty map |
| `delete` | remove element from map |
| `ok` | check if key exists |

**Example:**
```go
// Declare and initialize map
person := map[string]int{      // declare map
    "age":    25,
    "height": 175,
}

// Create empty map
grades := make(map[string]float64) // create empty map
grades["math"] = 8.5
grades["english"] = 9.0

// Check if key exists
age, exists := person["age"]   // check existence
if exists {
    fmt.Printf("Age: %d\n", age)
}

// Remove element
delete(person, "height")       // remove "height" key

// Iterate over map
for key, value := range grades {
    fmt.Printf("%s: %.1f\n", key, value)
}
```

---

## Structs

To create custom types:

| Command | Meaning |
|---------|---------|
| `type` | define new type |
| `struct` | structure with fields |
| `.` | access struct field |
| `&` | get struct address |

**Example:**
```go
// Define struct
type Person struct {           // define structure
    Name  string
    Age   int
    Email string
}

// Create struct instance
p1 := Person{                  // create with values
    Name:  "John",
    Age:   30,
    Email: "john@email.com",
}

p2 := Person{"Mary", 25, "mary@email.com"} // abbreviated form

// Access fields
fmt.Println(p1.Name)          // access field
p1.Age = 31                   // modify field

// Anonymous struct
coordinate := struct {
    X, Y float64
}{10.5, 20.3}
```

---

## Pointers

To work with memory addresses:

| Command | Meaning |
|---------|---------|
| `*type` | pointer to type |
| `&` | get variable address |
| `*` | dereference pointer (get value) |
| `new` | create pointer to zero value |

**Example:**
```go
x := 42
p := &x                       // get address of x

fmt.Println(*p)               // dereference: 42
*p = 100                      // modify value through pointer
fmt.Println(x)                // now x equals 100

// Pointer to struct
person := &Person{Name: "Ana", Age: 28}
fmt.Println(person.Name)      // Go automatically dereferences

// Create pointer with new
ptr := new(int)               // pointer to int with zero value
*ptr = 50
```

---

## Interfaces

To define contracts and polymorphism:

| Command | Meaning |
|---------|---------|
| `interface` | define interface |
| `type assertion` | check specific type |
| `type switch` | switch based on type |

**Example:**
```go
// Define interface
type Speaker interface {       // define interface
    Speak() string
}

// Types that implement the interface
type Person struct {
    Name string
}

func (p Person) Speak() string { // implement interface
    return "Hello, I'm " + p.Name
}

type Dog struct {
    Name string
}

func (d Dog) Speak() string {
    return "Woof! I'm " + d.Name
}

// Use interface
func greet(s Speaker) {
    fmt.Println(s.Speak())
}

// Usage
person := Person{Name: "John"}
dog := Dog{Name: "Rex"}

greet(person)                 // polymorphism
greet(dog)                    // polymorphism

// Type assertion
var i interface{} = "text"
s, ok := i.(string)           // check if it's string
if ok {
    fmt.Println("It's a string:", s)
}
```

---

## Goroutines and Concurrency

To execute code concurrently:

| Command | Meaning |
|---------|---------|
| `go` | execute function in goroutine |
| `sync.WaitGroup` | wait for goroutines to finish |
| `sync.Mutex` | mutex for synchronization |
| `runtime.NumCPU()` | number of available CPUs |

**Example:**
```go
import (
    "fmt"
    "sync"
    "time"
)

// Simple function
func task(id int) {
    fmt.Printf("Task %d started\n", id)
    time.Sleep(time.Second)
    fmt.Printf("Task %d finished\n", id)
}

func main() {
    var wg sync.WaitGroup      // wait group
    
    for i := 1; i <= 3; i++ {
        wg.Add(1)              // add goroutine to group
        go func(id int) {      // execute in goroutine
            defer wg.Done()    // mark as finished
            task(id)
        }(i)
    }
    
    wg.Wait()                  // wait for all to finish
    fmt.Println("All tasks finished")
}

// Mutex for synchronization
var (
    counter int
    mutex   sync.Mutex        // mutex for protection
)

func increment() {
    mutex.Lock()              // lock mutex
    counter++
    mutex.Unlock()            // unlock mutex
}
```

---

## Channels

For communication between goroutines:

| Command | Meaning |
|---------|---------|
| `chan type` | channel for specific type |
| `make(chan type)` | create channel |
| `<-` | send/receive from channel |
| `close` | close channel |
| `select` | choose between multiple channels |

**Example:**
```go
// Basic channel
ch := make(chan int)          // create integer channel

// Send in goroutine
go func() {
    ch <- 42                  // send value to channel
}()

value := <-ch                 // receive value from channel
fmt.Println(value)            // 42

// Buffered channel
buffered := make(chan string, 2) // channel with buffer of 2
buffered <- "first"
buffered <- "second"
// Doesn't block because it has buffer

// Select for multiple channels
ch1 := make(chan string)
ch2 := make(chan string)

go func() {
    time.Sleep(time.Second)
    ch1 <- "channel 1"
}()

go func() {
    time.Sleep(2 * time.Second)
    ch2 <- "channel 2"
}()

select {                      // choose between channels
case msg1 := <-ch1:
    fmt.Println("Received:", msg1)
case msg2 := <-ch2:
    fmt.Println("Received:", msg2)
case <-time.After(3 * time.Second):
    fmt.Println("Timeout!")
}

// Close channel
close(ch)                     // close channel
```

---

## Error Handling

To deal with errors idiomatically:

| Command | Meaning |
|---------|---------|
| `error` | standard error type |
| `fmt.Errorf` | create error with formatting |
| `errors.New` | create simple error |
| `nil` | null value (no error) |

**Example:**
```go
import (
    "errors"
    "fmt"
)

// Function that can return error
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero") // create error
    }
    return a / b, nil         // nil = no error
}

// Function with formatted error
func validateAge(age int) error {
    if age < 0 {
        return fmt.Errorf("invalid age: %d", age) // formatted error
    }
    return nil
}

// Use functions with error
result, err := divide(10, 0)
if err != nil {               // check if there's error
    fmt.Println("Error:", err)
    return
}
fmt.Println("Result:", result)

// Handle multiple errors
if err := validateAge(-5); err != nil {
    fmt.Println("Validation error:", err)
}
```

---

## Packages and Imports

To organize and reuse code:

| Command | Meaning |
|---------|---------|
| `package` | define package name |
| `import` | import packages |
| `_` | import only for side effects |
| `.` | import with direct names |

**Example:**
```go
// File: main.go
package main                  // define main package

import (
    "fmt"                     // standard package
    "math/rand"              // subpackage
    "time"
    
    _ "database/sql/driver"   // import for side effects
    . "strings"              // import with direct names
)

func main() {
    fmt.Println("Hello")      // fmt.Println
    ToUpper("text")          // strings.ToUpper (no prefix)
    
    rand.Seed(time.Now().UnixNano())
    fmt.Println(rand.Intn(100))
}

// File: utils/math.go
package utils                 // custom package

// Exported function (first letter uppercase)
func Add(a, b int) int {
    return a + b
}

// unexported function (first letter lowercase)
func multiply(a, b int) int {
    return a * b
}
```

---

## Methods

To add behaviors to types:

| Command | Meaning |
|---------|---------|
| `func (receiver) name()` | method with receiver |
| `func (receiver *type)` | method with pointer receiver |

**Example:**
```go
type Rectangle struct {
    Width, Height float64
}

// Method with value receiver
func (r Rectangle) Area() float64 {    // method with receiver
    return r.Width * r.Height
}

// Method with pointer receiver
func (r *Rectangle) Double() {         // pointer receiver
    r.Width *= 2
    r.Height *= 2
}

// Method with basic type receiver
type MyString string

func (s MyString) Uppercase() string {
    return strings.ToUpper(string(s))
}

// Using methods
rect := Rectangle{Width: 10, Height: 5}
fmt.Println(rect.Area())      // 50

rect.Double()                 // modifies original
fmt.Println(rect.Area())      // 200

text := MyString("go")
fmt.Println(text.Uppercase()) // "GO"
```

---

## Operators

Go symbols and operators:

| Operator | Meaning |
|----------|---------|
| `+` | addition or concatenation |
| `-` | subtraction |
| `*` | multiplication or dereference |
| `/` | division |
| `%` | remainder of division |
| `++` | increment |
| `--` | decrement |
| `==` | equal to |
| `!=` | not equal to |
| `<` | less than |
| `>` | greater than |
| `<=` | less than or equal |
| `>=` | greater than or equal |
| `&&` | logical AND |
| `\|\|` | logical OR |
| `!` | logical NOT |
| `&` | bitwise AND or address |
| `\|` | bitwise OR |
| `^` | bitwise XOR |
| `<<` | left shift |
| `>>` | right shift |
| `:=` | declaration and assignment |
| `=` | assignment |
| `+=` | add and assign |
| `-=` | subtract and assign |
| `<-` | channel operator |

**Example:**
```go
// Arithmetic
a := 10
b := 3
fmt.Println(a + b)    // 13 (addition)
fmt.Println(a % b)    // 1 (remainder)
a++                   // increment
b--                   // decrement

// Comparison
fmt.Println(a == b)   // false (equal to)
fmt.Println(a != b)   // true (not equal to)

// Logical
x := true
y := false
fmt.Println(x && y)   // false (logical AND)
fmt.Println(x || y)   // true (logical OR)
fmt.Println(!x)       // false (logical NOT)

// Bitwise
fmt.Println(5 & 3)    // 1 (bitwise AND)
fmt.Println(5 | 3)    // 7 (bitwise OR)
fmt.Println(5 ^ 3)    // 6 (bitwise XOR)

// Assignment
a += 5                // a = a + 5
a -= 2                // a = a - 2
```

---

## Reserved Words

Words that cannot be used as names:

| Word | Meaning |
|------|---------|
| `break` | exit loop or switch |
| `case` | case in switch |
| `chan` | channel |
| `const` | constant |
| `continue` | next iteration |
| `default` | default case |
| `defer` | defer execution |
| `else` | else |
| `fallthrough` | continue to next case |
| `for` | loop |
| `func` | function |
| `go` | goroutine |
| `goto` | unconditional jump |
| `if` | if |
| `import` | import |
| `interface` | interface |
| `map` | map |
| `package` | package |
| `range` | iterate |
| `return` | return |
| `select` | select channel |
| `struct` | structure |
| `switch` | select option |
| `type` | define type |
| `var` | variable |

---

## Built-in Functions

Ready-made Go functions:

| Function | Meaning |
|----------|---------|
| `len()` | length of slice, array, map, string |
| `cap()` | capacity of slice or channel |
| `make()` | create slice, map or channel |
| `new()` | allocate memory for type |
| `append()` | add to slice |
| `copy()` | copy between slices |
| `delete()` | remove from map |
| `close()` | close channel |
| `panic()` | stop program |
| `recover()` | recover from panic |
| `print()` | print (debugging) |
| `println()` | print line (debugging) |

**Example:**
```go
// len and cap
slice := make([]int, 3, 5)    // length 3, capacity 5
fmt.Println(len(slice))       // 3
fmt.Println(cap(slice))       // 5

// append
slice = append(slice, 4, 5)   // add elements
fmt.Println(len(slice))       // 5

// copy
source := []int{1, 2, 3}
destination := make([]int, len(source))
copy(destination, source)     // copy slice

// map operations
m := make(map[string]int)
m["key"] = 10
delete(m, "key")              // remove element

// panic and recover
func recoverExample() {
    defer func() {
        if r := recover(); r != nil { // recover from panic
            fmt.Println("Recovered:", r)
        }
    }()
    
    panic("something went wrong!")    // stop program
}
```

---

## Important Tips

### Zero Values (Default Values)

Every type in Go has a default value:

| Type | Zero Value |
|------|------------|
| `int` | `0` |
| `float64` | `0.0` |
| `string` | `""` |
| `bool` | `false` |
| `pointer` | `nil` |
| `slice` | `nil` |
| `map` | `nil` |
| `interface` | `nil` |
| `channel` | `nil` |

### Naming Conventions

- **Public**: first letter uppercase (`MyFunction`)
- **Private**: first letter lowercase (`myFunction`)
- **Constants**: UPPERCASE or CamelCase (`PI`, `MaxUsers`)
- **Packages**: lowercase, no underscore (`mathutil`)

### Go Idioms

```go
// Check error
if err != nil {
    return err
}

// Check existence in map
if value, ok := myMap["key"]; ok {
    // key exists
}

// Blank identifier to ignore values
_, err := functionThatReturnsTwoValues()

// Empty interface for any type
var anything interface{}

// Type assertion with check
if text, ok := anything.(string); ok {
    // it's a string
}
```

---

## How to Use This Dictionary

### Quick Search

Use Ctrl+F (or Cmd+F on Mac) to quickly find any command.

### Recommended Learning Order

1. **Basics**: package, import, func, var
2. **Control**: if, for, switch
3. **Types**: int, string, bool, struct
4. **Collections**: slice, map
5. **Pointers**: basic concepts
6. **Functions**: definition and usage
7. **Methods**: receivers
8. **Interfaces**: contracts
9. **Concurrency**: goroutines and channels
10. **Advanced**: reflection, generics

### Key Go Differences

- **Only one loop type**: `for`
- **No classes**: uses structs and methods
- **Garbage collector**: automatic memory management
- **Compiled**: generates single binary
- **Static typing**: types defined at compile time
- **Built-in concurrency**: goroutines and channels

---

## Useful Resources

- **Official documentation**: https://golang.org/doc/
- **Go Tour**: https://tour.golang.org/
- **Go Playground**: https://play.golang.org/
- **Effective Go**: https://golang.org/doc/effective_go.html

---

## License

This dictionary is open source and can be used freely in your projects and studies.

---

**Last updated:** July 2025  
**Version:** 1.0

*"Go is simple, fast and straight to the point. This dictionary helps you master the language in English!"*