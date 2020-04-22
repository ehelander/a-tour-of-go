# [A Tour of Go](https://tour.golang.org/welcome/1)

- [gofmt](https://golang.org/cmd/gofmt/) format tool
- [install Go](https://golang.org/doc/install)
  - https://www.digitalocean.com/community/tutorials/how-to-install-go-and-set-up-a-local-programming-environment-on-macos
    - `brew install golang`
    - `go get golang.org/x/tour`

## Basics

- [Packages](https://tour.golang.org/basics/1)
  - Every program is made up of packages.
  - Programs start running in the package `main`.
  - Convention: Package name is the last element of the import path.
- [Imports](https://tour.golang.org/basics/2)

  - Factorized import statement:

    ```go
    import (
        "fmt",
        "math"
    )
    ```

    - Can be written as separate imports (`import "fmt"), though the factorized style is preferred.

- [Exported names](https://tour.golang.org/basics/3)
  - A name is exported if it begins with a capital letter.
    - Unexported names are not accessible outside the package.
- [Functions](https://tour.golang.org/basics/4), [continued](https://tour.golang.org/basics/5)
  - Data types follow the variable name.
  - The type can be omitted from all but the last variable in a sequence of parameters that share a type.
- [Multiple results](https://tour.golang.org/basics/6)
  - A function can return multiple results.
- [Named return values](https://tour.golang.org/basics/7)
  - Naked return: Variables are defined at the top of the function and are returned with a naked `return` keyword.
    - Should only be used for short functions (to not hurt readability).
- [Variables](https://tour.golang.org/basics/8)
  - `var` declares a list of variables.
  - Type follows the variable name.
  - A `var` statement can be package- or function-scoped.
- [Variables with initializers](https://tour.golang.org/basics/9)
  - A `var` declaration can include an initializer.
  - If present, the type can be omitted; the variable will take the type of the initializer.
- [Short variable declarations](https://tour.golang.org/basics/10)
  - Inside a function, the `:=` operator can be used to declare a variable and assign it an initial value.
- [Basic types](https://tour.golang.org/basics/11)
  - Go's basic types
    - `bool`
    - `string`
    - int
      - `int`
      - `int8`
        - Alias: `byte`
      - `int16`
      - `int32`
        - Alias: `rune`
      - `int64`
    - uint
      - `uint`
      - `uint8`
      - `uint16`
      - `uint32`
      - `uint64`
      - `uintptr`
    - float
      - `float32`
      - `float64`
    - complex
      - `complex64`
      - `complex128`
  - Notes:
    - `int`, `uint`, and `uintptr` are 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems
    - When using an integer, use `int` unless a sized or unsigned integer type is needed.
- [Zero values](https://tour.golang.org/basics/12)
  - Variables declared without an explicit initial value receive their _zero value_:
    - Numeric: `0`
    - Boolean: `false`
    - Strings: `""`
- [Type conversions](https://tour.golang.org/basics/13)
  - `T(v)` converts value `v` to type `T`.
- [Type inference](https://tour.golang.org/basics/14)
  - When the right-hand side of an assignment contains an untyped numeric constant (and no explicit type is supplied), the type is inferred based on the precision of the constant (e.g., `int`, `float64`, `complex128`).
- [Constants](https://tour.golang.org/basics/15)
  - Declared with the `const` keyword
  - Values:
    - character
    - string
    - boolean
    - numeric
  - Constants cannot be declared with `:=`.
- [Numeric constants](https://tour.golang.org/basics/16)
  - Numeric constants are high-precision values.
  - An untyped constant takes the type needed by its context.

## Flow Control

- [For](https://tour.golang.org/flowcontrol/1), [continued](https://tour.golang.org/flowcontrol/2)
  - `for` is the only looping construct in Go.
  - Basic syntax: `for i := 0; i < 10; i++ {}`
    - Init statement (`i := 0`) - optional
      - Executed before the first iteration.
      - Variables declared here are only visible inside the `for` scope.
    - Conditional expression (`i < 10`) - required
      - Evaluated before every iteration.
      - Loop stops as soon as this condition evaluates to `false`.
      - When the init and post statements are excluded, `for` functions like a `while`.
    - Post statement (`i++`) - optional
      - Executed at the end of every iteration.
  - Note: No parentheses; but braces required.
- [Forever](https://tour.golang.org/flowcontrol/4)
  - An empty `for` is an infinite loop.
- [If](https://tour.golang.org/flowcontrol/5)
  - Similar to `for` in that no parentheses, but required braces.
- [If with a short statement](https://tour.golang.org/flowcontrol/6), [If and else if](https://tour.golang.org/flowcontrol/7)
  - `if` can being with a short statement to execute before the condition.
    - Variables declared in the statement are only scoped to the `if` or its `else`s.
- [Exercise: Loops and Functions](https://tour.golang.org/flowcontrol/8)

  ```go
  package main

  import (
    "fmt"
    "math"
  )

  func Sqrt10Iterations(x float64) float64 {
    z := 1.0
    for i := 0; i < 10; i++ {
      z -= (z*z - x) / (2 * z)
    }
    return z
  }

  func valuesDiffer(x, y float64) bool {
    const threshold = 0.0000000001
    var absoluteDifference = math.Abs(x - y)
    return absoluteDifference > threshold
  }

  func SqrtUntilDone(x float64) float64 {
    z := 1.0
    var zPrevious float64
    var iterations int
    for valuesDiffer(z, zPrevious) {
      zPrevious = z
      z -= (z*z - x) / (2 * z)
      iterations++
    }
    fmt.Println(iterations)
    return z
  }

  func main() {
    fmt.Println(Sqrt10Iterations(2))
    fmt.Println(SqrtUntilDone(2))
    fmt.Println(math.Sqrt(2))
  }
  ```

- [Switch](https://tour.golang.org/flowcontrol/9), [Switch evaluation order](https://tour.golang.org/flowcontrol/10)
  - Go's `switch` only runs the selected, not those that follow.
    - No `break` statement is needed.
  - The `switch` cases do not need to be constants, nor the value integers.
  - Cases are evaluated from top to bottom.
- [Switch with no condition](https://tour.golang.org/flowcontrol/11)
  - A `switch` with no condition is equivalent to `switch true`.
- [Defer](https://tour.golang.org/flowcontrol/12)
  - A `defer` statement delays the execution of a function until its surrounding function returns.
- [Stacking defers](https://tour.golang.org/flowcontrol/13)
  - Deferred functions are pushed onto a stack, evaluated in LIFO order.
