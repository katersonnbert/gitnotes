# Python 3

|R| ... things to read again and again!


# Python style guide and documentation conventions:

- [style guide crash course](https://gist.github.com/sloria/7001839)

- [Style guide Pep](https://www.python.org/dev/peps/pep-0008/)

- [Docstring Pep](https://www.python.org/dev/peps/pep-0257/)


# Basics

## Magic variable __`"_"`__

In interactive mode, the magic variable __`"_"`__ always returns the last value printed to the console.


## Comments

The hash character is used to add comments to code:

    # comment 1
    var = 1 # comment 2
            # comment 3

Triple single quotes are used for multi line comments.

    '''
    multi
    line
    comment
    '''


## Numbers in Python:

    1/2     # division always returns float
    1//2    # floor division, returns int
    1%2     # division remainder, returns int (?)
    2**3    # power of

Python supports

- int
- float
- Decimal
- Fraction
- complex numbers


## Strings in Python:

- Strings can be enclosed by single '...' or double "..." quotes.
- Special characters are escaped by the backslash character.
- __`\n`__ produces a new line.
- using __`r`__ before a string prevents special character escape.

        print('C:\name\path')   # will print "C:" [new line] "ame\path"
        print(r'C:\name\path')  # will print "C:\name\path"

- multi line strings are defined by using triple-quotes `'''...'''` or `"""..."""`.

        """
        Write multiple
        Lines of
        String
        """

- Strings can be concatenated by the `+` operator.

        lang = 'Py' + 'thon'  # lang contains 'Python'

- Strings can be repeated.

        food = 2 * 'yum'  # food contains 'yumyum'

- Strings can be treated as arrays.

        word = 'Python'
        word[0]         # returns 'P'
        word[-1]        # returns the last character 'n'
        word[-2]        # returns the second last character 'o'
        word[0:2]       # slicing returns the characters from inclusive 0 to exclusive 2 'Py'

|R|

NOTE: Python strings are immuatable! __`word[0] = "X"`__ will fail.

NOTE: When slicing the left index is always in-, the right always excluded.

    len(word)   # returns the length of a string.


## Lists in Python:

- A Python `List` is a compound data type.
- Contains comma separated values.
- Can contain values of different types.

        squares = [1, 4, 9, 16, 25]

- Can be indexed and sliced.

        squares[0]      # returns 1
        squares[-3:]    # returns [9, 16, 25], slicing returns a new list

- Lists are mutable, their values can be changed.

        squares[0] = 5  # changes content of the list 
                        # from [1, 4, 9, 16, 25] to [5, 4, 9, 16, 25]

- Lists can be nested

        one = [1, 2, 3]
        two = ['a', 'b', 'c']
        many = [one, two]
        many.append('d')        # appends new items to the end of a list.
        len(many)               # returns the length of a list.


## Control structures:

### `if` statement:

    if [condition]:
        [execute code]
    elif [condition]:
        [execute code]
    else:
        [execute code]


### `while` statement:

    while [condition]:
        [execute code]


### `for` statement:

- The Python __`for`__ statement provides the iteration over the items of any sequence (list or string).
- The __`range()`__ function generates arithmetic progressions; it provides additional starting points 
and different steps of progression.

        for i in range(5):
            print(i, end=', ')      # prints 0, 1, 2, 3, 4, 

- The __`break`__ keyword ends a __`for`__ statement prematurely.
- The __`for`__ statement can be used with an __`else`__ clause.

        for n in range(2, 10):
            for x in range(2, n):
                if n % x == 0:
                    print(n, 'equals', x, '*', n//x)
                    break
            else:
                print(n, 'is a prime number')

- The __`continue`__ keyword in the body of a `for` statement ends the current iteration, 
ignores any following code and continues with the next element of the __`for`__ statement.

        for num in range(2, 10):
            if num % 2 == 0:
                print("Found an even number", num)
                continue
            print("Found a number", num)


## Functions:

- The keyword __` def `__ introduces a function definition.
- Function names should be lower case, words should be separated by underscores.
- Always use __` self `__ as the first argument to instance methods.
- Always use __` cls `__ as the first argument to class methods.
- The first statement of the function body can be a string literal. If present, this literal is the 
documentation string of the function.

        def print_me(s):
            """Print the text that is provided to the function"""
            print("I will print: ", s)
        
        print_me("me")     # prints "I will print: me"


### Execution of a function |R|
- A function definition introduces the function name to the current local symbol table.
- When a function is executed, a new symbol table is created used for the local variables of this function.
- Variable references always look up the values in the following order:
    1. local symbol table
    2. local symbol tables of enclosing functions
    3. global symbol table
    4. table of built-in names
- Global variables cannot be directly assigned with a value within a function!
- Arguments of a function are introduced into the newly created symbol table. Their value is always an 
object reference, never a value of the object!
- Functions without a __` return `__ keyword return value __` None `__.
- Not executed function calls return a reference to this function in the local symbol table and can be 
used to assign it to another name:

        print_me                    # prints '<function print_me at [memoryaddress]>'
        p = print_me
        p("I shall be printed")     # will print the provided string


### Function arguments
There are distinct types of function arguments:

- required arguments
- positional arguments
- keyword arguments
- optional arguments = arguments with default value
- [variadic](https://en.wikipedia.org/wiki/Variadic_function) arguments


#### Default argument values

- Within the definition of a function, default values of arguments can be defined.

        def ask_ok(prompt, retries=4, complaint='Yes or no, please!'):
            print(complaint)

__|R|__
- PITFALL NOTE: The default value of a function argument is evaluated only once. When the default is 
a mutable object, this can lead to unexpected behavior, e.g.:

        def f(a, L=[]):
            L.append(a)
            return L
        
        print(f(1))     # prints [1]
        print(f(2))     # prints [1, 2]
        print(f(3))     # prints [1, 2, 3]

Avoid this by using __`None`__ instead of __`[]`__ as default values and initialize accordingly in the function body.


#### Required function arguments
- A required argument has no default value

        def f(req_arg, def_arg='default_value'):
            print('I shall print the default: '+ def_arg)

        f('')

Can be passed as

- keyword arguments
- positional arguments

keyword arguments are passed as __`[identifier]=[value]`__

    def cat(legs, tail, whiskers):
        print('legs: ' + str(legs) + ', tail: '+ str(tail) + ', whiskers: '+ str(whiskers))

    cat(tail=1, whiskers='yes, has them', legs=4)


#### Arbitrary Argument Lists

- A function can be called with an arbitrary number of arguments (called variadic arguments), denoted by __`*args`__.
- Variadic arguments will be wrapped in a Tuple.

        def write_multiple_items(file, separator, *args):
            file.write(separator.join(args))

- Any arguments defined after the variadic argument can only be used as keyword arguments.

        def concat(*args, sep = "-"):
            return sep.join(args)

- Lists can be handed as arbitrary arguments list using the __`*`__ operator, dicts using __`**`__.

        l = ['1', '2', '3', '4']
        concat(*l)

        def concat_four(one, two, three, four='string'):
            print(one +" "+ two +" "+ three +" "+ four)
        
        d = {"one": "Concatenate", "two": "this", "three": "wonderful"}
        concat_four(**d)


## Lambda expressions

- Lambda expressions are small, anonymous functions.
- They can be used via the __`lambda`__ keyword.
- Lambdas can reference variables from the containing scope.

        def make_incrementor(n):
            return lambda x: x + n
        
        f = make_incrementor(5)
        f(0)
        f(1)
        f(10)

- Lambdas can be used to pass small functions as arguments into a function

        pairs = [(1, 'z'), (2, 'e'), (3, 'b'), (4, 'x')]
        pairs
        pairs.sort(key=lambda pair: pair[1])
        pairs


## Annotating function argument and return types

- Function argument and return types can be annotated in the function definition.
- __`->`__ denotes the return type.
- Annotating types is completely optional.
- Annotations are stored in the `\__annotations\__` attribute of the function as a dict.

        def f(ham: str, eggs: str = 'eggs') -> str:
            print("Annotations:", f.__annotations__)

        f('bacon')


## Data Structures

### List comprehensions
- Easy method to obtain lists where operations have been performed on each member.
- List comprehensions are square brackets containing at least (but not limited to) one __`for`__ clause. 

        squares = [x**2 for x in range(10)]

        [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]

        # is the abbreviation for:

        combs = []
        for x in [1,2,3]:
            for y in [3,1,4]:
                if x != y:
                    combs.append((x, y))

        combs

- List comprehensions can also be used with nested lists.


### Tuples
- A tuple consists of a number of values, separated by commas.
- Contents of a tuple can be accessed by index.
- Tuples can be nested by using parenthesis.
- __|R|__ NOTE: contents of a tuple are immutable! (but they can contain lists which elements are mutable again...)

        t = 12345, 54321, 'hello!'  # would print (12345, 54321, 'hello!')
        u = t, (1, 2, 3, 4, 5)      # would print ((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))
        u

- Tuples are mainly used for heterogenious elements.
- __NOTE__: A tuple with a single element can only be created with the element 
    followed by a single, trailing comma! __|R
   |__

        singleton = 'hello',
        type(singleton)

- Elements can be extracted to variables from a tuple in the reverse packing syntax.

        x, y, z = t
        y
        z


### Sets __|R|__
- A set is an unordered collection.
- Sets must not contain duplicate elements - duplicate entries are automatically discarded.
- Sets support union __`|`__, intersection __`&`__, difference __`-`__, symmetric difference __`^`__.
- Sets are created by using curly braces; empty sets have to be created by using __`set()`__

        basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
        'orange' in basket      # returns True
        'crabgrass' in basket   # returns False

- similar to list comprehension, set comprehension is supported

        a = {x for x in 'abracadabra says me' if x not in 'abcme'}
        a


### Dictionaries
- Set of unordered key : value pairs, separated by commas.
- Keys have to be unique and be of an immutable type like string or number.
- Dictionaries are indexed by keys.
- Dictionaries support comprehensions.
- Dictionaries can be looped using the __`dict.items()`__ method.


# Methods
- Methods are functions that belong to an object and are named in the fashion __`obj.methodname`__.


## Modules
- Files with the ending '.py' can contain one or more function definitions. The content of 
these files can be imported by using the name of the file after an `import` statement.
- __|R|__ Importing a module introduces only the module name to the current symbol table. 
Functions from the module can be accessed from this reference.
- __|R|__ If a module contains executable statements in addition to function definitions, 
these statements are executed exactly ONCE when the module is imported the first time.
- When a module is changed on the fly, the interpreter has to re-import this module.
- A star import imports all functions not beginning with an underscore. (star import should not be used though anyway)

        from sys import *

- Names from a module can be imported directly

        from sys import path, platform


### Modules as script __|R|__
- Modules can be run as scripts.

        __`python mymodule.py <arguments>`__

- The `_name_` of an executed module will be changed to `_main_`. Adding `if _name_ == _main_`, 
makes it usable as a script as well as an importable module. [xxx] is this actually true?


### Module search path
- When a module is imported, the interpreter first looks up its name in the built-in module namespace.
- If nothing is found, the search includes directories given in __`sys.path`__. This includes
    - the directory of the input script or the current directory if it has been changed.
    - __`PYTHONPATH`__ ... The default search path is installation dependent, but generally begins 
    with `prefix/lib/pythonversion`.


### Python compiled files (.pyc) __|R|__
- Compiled python modules are cached in the `__pycache__` directory with name 'module.version.pyc' 
where version is the python version used to compile.
- Switches can be used to reduce the file size of compile python files. __'-0'__ removes assert statements, 
__'-00'__ asserts and docstrings.


[currently at](https://docs.python.org/3/tutorial/modules.html#standard-modules)


# Writing Python 2 and 3 compatible code:
A list of Python 2 and 3 compatible code can be found at 
[Python-future](http://python-future.org/compatible_idioms.html).

- [Porting Python 2 to 3](https://docs.python.org/3/howto/pyporting.html)


# Creating PyPI packages

Always use .rst files for as README file format.
- [rst](http://docutils.sourceforge.net/rst.html)

- [Creating python packages](https://packaging.python.org/tutorials/distributing-packages/)
- [Creating a source distribution with distutils](https://docs.python.org/3.4/distutils/sourcedist.html)
- [Packaging python libraries](http://www.diveintopython3.net/packaging.html)

Before uploading to PyPI, test the distribution on TestPyPI
- [Using TestPyPI](https://packaging.python.org/guides/using-testpypi)


# Resources used:
[Pyhton3 tutorial](https://docs.python.org/3/tutorial)
