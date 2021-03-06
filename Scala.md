SCALA
=====

## How to use the scala shell with Ubuntu:
- DL latest version from [here](http://www.scala-lang.org/download).
- Extract to directory of choice, then run.

        bash [dir]/scala/bin/scala

## Good to knows
- imperative programming style   e.g. java, c++ ... often mutate a state shared between different functions
- functional programming style   Haskell ... does not change a mutual state. every function just computes and returns a value
- statically typed
- has type inference

## Scala interpreter commands

- `:help`           ... print help
- `:quit`           ... end interpreter
- `:save <path>`    ... save shell history to <path>
- `:history`        ... show history

# Basic usage

## Defining functions:

    def functionName(parameter: Type, parameter: Type, ...): Return Type = { function body }

e.g.

    def getmax(x: Int, y: Int): Int = {
      if (x > y)
        x
      else
        y
    }

## Using loops

    var i = 0
    while (i < args.length) {
        println(args(i))
        i += 1
    }

## Iterate with foreach and for

    args.foreach(arg => println(arg))
        or
    args.foreach((arg: String) => println(arg))

- foreach is called on args and passes in a functional literal

## Functional literal

    (Parameter list) => function body

e.g.

    (x: Int, y: Int) => x + y


## Methods in Scala
- Methods are class member functions
- they can be called from instances of the corresponding class
- as a rule methods with only one parameter can be called without dot or parenthesis

e.g.

    for (i <- 0 to 2)
        [body]

In this example the class Int has a method "to" with one parameter of type Int.
One could also use this syntax:

    for (i <- (0).to(2))

Other examples:

    1 + 2                   is actually     (1).+(2)
    someArray(i)            is actually     someArray.apply(i)
    someArray(i, "text")    is actually     someArray.update(i, "text")

Scala basically treats everything as objects with methods.

## Using lists in Scala
- Scala Lists are always immutable
- Lists can be easily created and initialized:

        val listOne = List(1, 2)

- A scala list has a concatenation method named `:::`

        val listTwo = listOne ::: List(3, 4)

- A scala list has a prepend method named `::` (cons). This method prepends a value to the beginning of a List.
Prepending requires constant time whereas appending to a list grows linearly with the size of the list.

        val listThree = 42 :: listTwo

## Using Tuples in Scala
- Tuples are containers that can contain elements of different types.

        val trip = (1, "two", 3)
        print(trip._2)

## Using Sets and Maps in Scala
- Sets and Maps can be immutable or mutable. By default they are immutable, if a mutable Object is required,
it needs to be specifically imported.
- Sets and mutable Sets provide different methods and behaviors. If something is added to an immutable set,
the set is not ordered; a mutable set will be reordered.

        val tmpSet = Set("One", "Two", "Three")
        tmpSet += "Four"

- Maps provide a convenient key -> value mapping

        val tmpMap = Map[Int, String]()
        tmpMap += (1 -> "One")
        tmpMap += (2 -> "Two")
        tmpMap += (3 -> "Three")
        print(tmpMap(3))

## Classes and Objects

- Classes are defined like thus:

        class SomeName {
            //enter class definition
        }

- An object of a class can be created thusly:

        new SomeName

- A class contains the following members:
    - fields ... are either val or var and refer to objects
    - methods ... contain executable code

- Private members cannot be accessed via the objects of a class:

        class SomeName {
            var someField = 0
            private var somePrivateField = 0
        }

        x = new SomeName
        x.someField // can be accessed
        x.somePrivateField // does not compile

- Defining methods:

        class AddStuff {
            var acc = 0
            private val constAdd = 10

            def add (a: Int): Unit = { acc += a + constAdd }
        }

        var stuff = new AddStuff
        stuff add 12
        stuff.acc // returns 22

- If a method has no return type, `[: Unit =]` is optional in the function definition. The following is also valid:

        class AddStuff {
            var acc = 0
            private val constAdd = 10

            def add (a: Int) { acc += a + constAdd }
        }

## Singleton objects = Companion objects

Scala classes do not provide static methods. If static methods are required, a class definition and a
companion object definition have to be created in the same file.
Companion objects can access private members of the companion class. Now the methods defined in the companion
object can be used like a static method would have been used in Java.

e.g.

    package stuff

    class CompanionClass {
        private val constNumber = 10
        private def add (a: Int): Int = { a + constNumber }
    }

    object CompanionClass {
        def printSum(b: Int) {
            val tmp = new CompanionClass
            print("This has been calculated: ")
            println(tmp add b)
        }
    }

    // compile scalac CompanionClass.scala

    import stuff.CompanionClass
    CompanionClass(405) // prints: "This has been calculated: 415"

p71


Mostly from
Programming in Scala; Odersky, Spoon, Venners; artima; 2010
