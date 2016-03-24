# Go

# Packages:
- Go programs are made of packages.
- Programs start running in package main.
- further packages can be used by `import`
- imports can be written as:
	```
	import "fmt"
	import "math"
	```
    or as

	```
    import (
	    "fmt"
	    "math"
	)
	```

# Functions:
- Go is a statically typed language.
- Functions take zero or more arguments, arguments and return types have to be typed.
- The types of function arguments are defined AFTER the name of the argument.
	```
	func add(x int, y int) int {
		return y + x	
	}
	```

- Functions can return multiple values
	```
	func swap(fst string, snd string) (string, string) {
		return snd, fst
	}
    //
	func main() {
		a, b := swap("I am", "First")
		fmt.Println(a, b, "says Mr Yoda")
	}
	```

# Variables
- Variables are declared via the keyword `var` and required a type after the variable name.
- Variables can be declared at both package and function level. Variables can also be initialized when they are declared.
	```
	var a bool
	var d int = 10
	func main() {
		var i int
		fmt.Println("I spy with my little i", i, "which is", a, "ok and also very ", d, "dent")
	}
	```
- NOTE: Depending on the type the variables are initialized with different values (e.g. bool - false, int - 0, string - "") - and not with null.
- Inside a function the variable declaration can be shortened by using the short assignment `:=` with implicit type.
	```
	fancyPants := "The fairy queen short assignes fancy pants which are made of"
	fmt.Println(fancyPants, reflect.TypeOf(fancyPants))
	fancyPants := "The fairy queen short assignes fancy pants which are made of reassigned"
	fmt.Println(fancyPants, reflect.TypeOf(fancyPants))
	```

# Constants
- Constants are declared like variables using the keyword `const`.
- Constants can only be initialized at their declaration, but can take advantage of type inference.
	```
	const epicPants string = "Her epic pants on the other hand are very constant"
    fmt.Println(epicPants)
    const epicShirt = "As is her epic shirt, which interestingly is also made of inferred"
    fmt.Println(epicShirt, reflect.TypeOf(epicShirt))
    //
	const intern = 10
	const lowFloat = 1.2
    fmt.Println("Meanwhile the", reflect.TypeOf(intern), "does not even know its inferred", reflect.TypeOf(lowFloat), "value")
	```

# Basic types

- The available basic types are
	```
	bool
	string
	int  int8  int16  int32  int64
	uint uint8 uint16 uint32 uint64 uintptr
	byte // alias for uint8
	rune // alias for int32
		 // represents a Unicode code point
	float32 float64
	complex64 complex128
	```

- NOTE! `int, uint, uintptr` types are 32bit/64bit on 32/64bit systems respectively


# Type conversion
- The expression `T(v)` converts value `v` into type `T`.
- NOTE: Go requires explicit conversion between different types.
	```
    hui := 49
    pfui := math.Sqrt(float64(hui))
    var wui int = int(pfui)
    fmt.Println(wui)
	```

# Control structures

### For
- Go has only one loop structure, the `for` loop; the syntax is `for [init statement]; [ending condition]; [post iteration statement] {}`
	```
	for i := 0; i < 4; i++ {
		fmt.Println("The fairy queen does situp number", i)
	}
	fmt.Println("And now she's having a break.")
	```

- The first and the last statement of a for loop is optional:
	```
	moreSitups := 4
	for moreSitups < 10 {
		fmt.Println("The fairy queen does situp number", moreSitups)
		moreSitups = moreSitups + 1
	}
	fmt.Println("And now she's tired and eats icecream.")
	```

### If
- The plain `if` looks like this:
    ```
	evenMoreSitups := 10
	if evenMoreSitups > 9 {
		fmt.Println("No, thank you!")
	}
	```

- There is a "short" statement, that can be executed immediately before an `if` condition.
	```
	func pow(x, n, lim float64) float64 {
		if v := math.Pow(x, n); v < lim {
			return v
		}
		return lim
	}
    //
	func main() {
		fmt.Println(
			pow(3, 2, 10),
			pow(3, 3, 20),
		)
	}
	```

- `if - else` is nothing fancy and looks like this:
	```
	if [condition] {
	} else {
	}
	```
- NOTE `elseif` functionality is handled by `switch`.


### Switch
- `switch` has `case`, `default` as fallthrough and can have an initial statement:
	```
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.", os)
	}
	```

- Switch without a condition is used instead of if-else chains:
	```
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
	```


### Defer

- The `defer` statement defers the execution of a function until the surrounding function ends.
- The defer's arguments are evaluated immediately, but the function call is not executed until the end of the surrounding function.
	```
	xx := "a Yoda"
	yy := "the fairy queen is not"
	defer fmt.Println(xx)
	xx = "something else"
	fmt.Println(yy)
	```

- Defers are pushed onto a stack, which means: last in first out...


# Pointers

- Go has no pointer arithmetic.
- A pointer holds the memory address of a variable.
- `*T` is of type pointer to a value of type `T` 
	```var p *int```
- The `&` operator generates a pointer to its corresponding variable:
	```
	var p *int
	i := 42
	p = &i
	```
	or
	```
	b := &i
	```
- The `*` operator enables access to the value of the variable a pointer is associated with.
	```
	fmt.Println(*p)	// read value of i through pointer p
	*p = 21			// set value of i through pointer p
	```

- The following example shows basic pointer usage and unusage:
    ```
	func testPointer() {
		var i int = 21
		var x *int
		//
		//fmt.Println("*x has value:\t", *x) // Since the pointer points nowhere at the moment, this leads to panic.
		//x = 12 // Since this is a pointer, it can only hold addresses, no actual value and this leads to panic.
		//
		fmt.Println("x has value:\t", x)		// <nil>
		//
		x = &i
		fmt.Println("x has value:\t", x)		// [some address]
		fmt.Println("*x points to:\t", *x)		// 21
		fmt.Println("i before:\t", i)			// 21
		//
		*x = 22
		fmt.Println("i after:\t", i)			// 22
		fmt.Println("x has value:\t", x)		// [same address]
		//
		*x = *x/2
		fmt.Println("i has been divided:\t", i)	// 11
	}
	```

# Structs
- A `struct` is a collection of fields.
- The fields of a struct can be accessed using a dot. Struct fields can also be accessed via a struct pointer.
- The fields of a struct will be initialized with default values according to the field type, if they are not set at struct initialization.
	```
	type Vertex struct {
		X int
		Y int
	}
    //
	func useVertex() {
		var va Vertex = Vertex{1, 2}
		vb := Vertex{}
		fmt.Println("Struct access: va:", va, "vb:", vb)
		fmt.Println("Struct field access: va.X:", va.X, "vb.X:", vb.X)
        //
		var pa *Vertex = &va
		pb := &vb
		pb.X = 120
		fmt.Println("Struct pointer access: va.X:", pa.X, "vb.X:", pb.X)
		*pb = *pa
		fmt.Println("Pointer replace of struct vb:", vb)
	}
	```

# Arrays
- The type `[n]T` is an array of `n` values of type `T`.
- Arrays cannot be resized.
	```
    var story [6]string
    story[0] = "\n"
    story[1] = "..."
    story[2] = "happily"
    story[3] = "ever"
    story[4] = "after"
    story[5] = "(but not really)"
    fmt.Println(story[0], "Tell me the story ...", story[0], story)
	```

### Slices of an array
- Slices point to arrays.
- `[]T` is a slice with elements of type `T`.
	```
	primes := []int{2, 3, 5, 7, 11, 13}
	fmt.Println("primes ==", primes)
	```

- Slices can be multi dimensional and can contain any type including other slices:
	```
	import "fmt"
	import "strings"
    //
	func printBoard {
		board := [][]string{
			[]string{"_", "_", "_"},
			[]string{"_", "_", "_"},
			[]string{"_", "_", "_"},
		}
        //
		for i := 0; i < len(board); i++ {
			fmt.Printf("%s\n", strings.Join(board[i], " "))
		}
	}
	```

- Slices can be re-sliced using the syntax `s[lo:hi]` where `lo` and `hi` are integers corresponding to the slice index.
- `lo` is always included, `hi` is always excluded. `s[lo:lo]` is empty, `s[lo:lo+1]` contains exactly one element.
- A missing `lo` index implies and index of 0
	```
	s := []int{1, 2, 3, 4, 5, 6}
	fmt.Println(s[:3])
	```
- A missing `hi` index implies `len(s)`
	```
	s := []int{1, 2, 3, 4, 5, 6}
	fmt.Println(s[3:])
	```







Distilled from resources:
[Go homepage](https://golang.org/doc/)
[Go by example](https://gobyexample.com/)
