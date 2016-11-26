# HackerSchool - Go e Git

## Configuration

```bash
export GOPATH=$HOME/dev/go
echo "export GOPATH=$HOME/dev/go" >> $HOME/.bashrc
```

## Download sources

```bash
go get github.com/jguer/go-hes
```

## Basic example

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Arch!")
}
```

### Run code
```bash
go run hello.go
```

### Compile code
```bash
go build hello.go
```

## Packages
```go
import (
    "fmt"       // Println, Printf, ...
    "strings"
)
```
## Functions
```go
func plus(a int, b int) int { // 2 args, 1 return
    return a + b
}

func morePlus(a, b, c int) (int, error) { // 2 args, 2 returns
    return a + b + c, nil
}

func do(f func(int, int) int) int { // Receives function argument
    return f(0, 1)
}

func f(a int, b int) (c int) { // Declares c as return variable
    c = a + b
    return // Automatically returns c
}

func valchanger(a *int, b int) { //Pass by reference
    // Must be called with (&vara, varb)
    *a = *a + b
}
```

## Variable declarations

Convention:
- lower camelCase for private definitions
- upper CamelCase for public definitions

```go
var myNum int
var myNum2 int = 2
var myNum3 = 3        // Implicit type
complexNum := 4 + 2i  // Implicit var and type
var myString string = "Hello World"
const num = 3  // Constant
var r rune = 'G' // Rune, int32, holds multi-byte characters
```

### Strings
```go
myString[0] // First byte of string (beware of multi-byte characters)
strings.ToUpper("golang") // Uppercase
strings.Contains("golang", "go") // true
concat := "Go supports" + " string concatenation"
```

### Arrays
```go
var a [5]int    // 5 positions initialized to 0
var b [...]int{1, 2, 3, 4} // 4 positions initialized with values
len(a)    // 5
```

### Slices (array lists)
```go
s := make([]string, 3)
s2 := []string{"Hello", "World", "of"}
s3 := append(s2, "Go")
fmt.Println(s3[:2])   // [Hello World]
fmt.Println(s3[1:])   // [World of Go]
fmt.Println(s3[1:3])   // [World of]
```

### Maps
```go
m := make(map[string]int)
m["k1"] = 7
m["k2"] = 8
fmt.Println(m)
fmt.Println(m["k1"])
delete(m, "k2")
v, err := m["k2"]          // Returns two values
fmt.Println("found k2?", v, err)
_, err = m["k2"]          // Ignoring first value
```

### Structs
```go
type Vertex struct {
    X int
    Y int
}
myVertex := Vertex{1, 2} //Declare implicit
myNewVertex := Vertex{X: 3, Y: 4} //Declare explicit
anotherVertex = &Vertex{X: 5, 6: 40}) // Reference
myVertex.X = anotherVertex.X // Automatically dereferences pointer

func (v *Vertex) area() int {    // Method, can be called with myVertex.area()
    return v.X * v.V
}
```

## IO
```go
fmt.Println("Hello World") // Print line
fmt.Printf("%v \n", myVar) // Print format: value
fmt.Printf("%T \n", myVar) // Print format: type
fmt.Printf("My string: %s \n", myString) // Print format: strings (could use %v too)
```

## Conditionals

### If
```go
var num int = 3;
if aux := 2; num > aux {
    fmt.Println("num is great!")
} else if num == aux {
    fmt.Println("num is the same")
} else {
    fmt.Println("num is low")
}
```

### Switch
```go
i := 4
switch i {
case 1: // if i == 1
    fmt.Println("one")
case 2: // if i == 2
    fmt.Println("two")
case 3, 4, 5, 6:
    fmt.Print("something, ")
    fallthrough                 // Requires explicit fallthrough
default:
    fmt.Println("don't care")
}
```

## Cycles

### For
```go
for i := 0; i < 25; i++ {
    if i == 12 {
        // Skip 12
        continue
    }
    if i == 14 {
        // Stop loop before printing 14
        break
    }
    fmt.Println(i);
}

for i < 20 {
    // Simulates 'while' loop of other languages
}

for {
    // Infinite loop
}

// Cycle through keys and values
for i, v := range []string{"Hello", "World", "of", "Go"} {
    fmt.Printf("index %v has value %v \n", i, v)
}
```

## Comments
```go
// Single line comments
/* Multi line comments */
```
