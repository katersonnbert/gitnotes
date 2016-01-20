# Java Good to Knows and Best practices:

When using Object Properties:
- define the properties of a class as
	private

  and the value setter and getter methods as well as the property getter methods as
	public final

Difference JavaFX Observable and ObservableValue:
	an Observable does not wrap its value and fires changes to registered InvalidationListeners
	an ObservableValue wraps its value and fires changes to registered ChangeListeners

InvalidationListeners will change their state to invalid, when they receive the FIRST change from its bound observable. Until the Listener is validated, it will stay invalid, regardless of how many changes will follow. It is implemented as lazy.

ChangeListeners will recompute their changes with every change event, even if the bound observableValue is lazy, the listener will aways enforce eager computation.


JavaFX bindings and properties all support lazy evaluation: changes are only computed, when the changed values are requested


http://www.oracle.com/technetwork/articles/javase/bloch-effective-08-qa-140880.html

http://blog.jooq.org/2013/08/20/10-subtle-best-practices-when-coding-java/


- do not use raw types!
	List dogs = new ArrayList();			// evil
	List<dogs> = new ArrayList<dogs>();		// good, it prevents ClassCastExceptions during runtime


- use enums instead of int constants!


- use @Override								 ... if you don't, then the following error message will tell you you tried to overload a method instead:
											// Broken! Unintentional overloading
	public boolean equals(MyClass other) { 	// MyClass should be Object.
		...
	}


- lazy initialization - don't do it. if you have to do it:
	// Lazy initialization holder class idiom for static fields
	private static class FieldHolder {
		 static final FieldType field = computeFieldValue();
	}
	static FieldType getField() { return FieldHolder.field; }


- Use context objects
	// Bad because it sucks if you want to add additional informations 
	// like e.g. ID or source to the interface later on
	interface EventListener {
		void message(String message);
	}

	// use a context object instead:
	interface MessageContext {
	    String message();
	    Integer id();
    	MessageSource source();
	}
 
	// Awesome!
	interface EventListener {
	    void message(MessageContext context);
	}


- Do not use anonymous, local or inner classes more than absolutely necessary! 
	They always keep references to their outer class wherever they are.

	Solution: whenever using an anonymous, local or inner class,
		try to make it final or a regular top level class!


- Use Single Abstact Methods (SAM) utilizing lambdas:
	// Instead of
	listeners.add(new EventListener() {
		@Override
		public void message(MessageContext c) {
		    System.out.println(c.message()));
		}
	});

	// use e.g.
	$(document)
		.find(c -> $(c).id() != null)					// Find elements with an ID
		.children(c -> $(c).tag().equals("order"))		// Find their  child elements
		.each(c -> System.out.println($(c)))			// Print all matches


	More information on lambdas
    http://blog.informatech.cr/2013/04/10/java-optional-objects/
    http://blog.informatech.cr/2013/03/25/java-streams-api-preview/
    http://blog.informatech.cr/2013/03/24/java-streams-preview-vs-net-linq/
    http://blog.informatech.cr/2013/03/11/java-infinite-streams/


- Avoid returning null from API methods!
	If methods can be chained, than returning null will suck to identify where the null
	actually comes from! So don't return it; use null only for the "uninitialized" or 
	"absent" semantics.


- NEVER return null arrays or lists from API methods!
	Null cannot hold an error message, so specific Exceptions
	are not possible, when an array or a list is null.
	Returning null will lead to hard to find null pointer exceptions here.


- Short circuit equals(); this dramatically increases performance, if they are compared for identity first:
	@Override
	public boolean equals(Object other) {
		if (this == other) return true;
	 
		// Rest of equality logic...
	}

	// Note: short circuit 
	@Override
	public boolean equals(Object other) {
		if (this == other) return true;
		if (other == null) return false;
	 
		// Rest of equality logic...
	}


- try to make methods final by default
	Good to know: @Override does not work with final methods
	making methods final prohibits exactly that: that custom methods are being overridden.


- Avoid the method(T...) signature
	void acceptAll(Object... all);	// this is nice

	void acceptAll(T... all);		// this is nice as well

	void acceptAll(T... all);
	void acceptAll(String message, T... all)	// this leads to an ambiguous method call

	To break it down: using a method(T...) signature will prohibit method overloading for this method


- static ... when adding the static keyword to a method or a variable of a class, this method/variable does not belong to an instance of this class, but can be
used without any instance at all.


- Keywords:
	declaration			...	public static int myNuber;
							public Label myText;
	initialization		... myNumber = 10;
							myText = new Label();

- a class implementing two interfaces which contain a method with the same signature will lead to an error!





# Java Background

Java Compiler ... just in time compiler. compiles sourcecode to bytecode and bytecode only during runtime into machinecode. Its using heuristics to anticipate which parts of the code will be used and compiles only these.

Java has no pointers, its using references instead. A reference represents an object and stores it in a variable. References have a type thats not changeable and can therefore not be exploited as an access point to parts of memory containing e.g. variables defined as private.

Garbage collector ... Java checks constantly, if objects are still referenced. Only if there are no references left, an object can and will be deleted. This also means, as long as an object has hidden references, it will not be deleted. The garbage collector supposedly deletes everything thats not referenced any more on its own. Since its a background thread of its own, it can use up resources while freeing memory at inopportune moments during the execution of a program, but modern gc's should not act up, while a cpu intensive activity is happening.

Java sucks at scripting and platformspecific tasks

Java in ce intarweb is based on the Oracle JVM distribution - for all browsers, there is no other JVM at the moment.

- JDK ... java development kit ... required for dev of sourcecode and compilation of the same into bytecode
- API ... application and programming interface (programmierschnittstelle)
- JRE ... Java runtime environment ... is required for bytecode (compiled by the JDK) to machine code compilation and execution; contains java class-libraries
- JVM ... java virtual machine (part of the JRE) ... contains class loader, memory handling and garbage collector, execution engine for machine code
- JavaFX ... new java graphical user interface stack (from oracle) ... pure java library, was manly developed for client side webapplications parallel to flash, silverlight, etc.

There are four different Java Platforms
- Java SE (standard edition, normal software dev)
- Java ME (micro edition, dev for phones, pdas, tablets, etc, modern tablets are powerful enough to run the normal JVM though)
- Java Card (Java on chipcards, reduced Java functionality)
- Java EE (Enterprise edition; dev of enterprise applications)



# Java is an imperative language

Always remember: Java is case sensitive

- token ... unity of characters that are interpreted as an entity e.g. 12345, teststring
- separators ... whitespaces, ;,()[]{} ... used to separate tokens from one another
- java is using the unicode set for all characters
- naming conventions for identifiers (variables, methodnames, classnames, etc), not allowed are:
    - whitespaces within identifiers
    - the first character must not be a number (e.g. 2dumb2live)
    - special characters (like ! * % / etc)
    - "null", "class" ... these are reserved keywords

literals are:
- the boolean expressions true and false
- clear numbers e.g. 12345 or 12.34567
- characters under quote e.g. "textliteral"
- character literals with special function e.g. \n
- null

reserved key words which cannot be used as literals:
abstract continue for new switch assert default goto package synchronized boolean do if private this
break double implements protected throw byte else import public throws case enum instance of return transient
catch extends int short try char final interface static void class finally long strictfp volatile
const float native super while


## comments
line comments ... valid from the identifier "//" until the end of the line
``` java
	code // this is a comment
```

block comments ... valid with the identifier pair /* and */
``` java
	code /* this
	is a
	comment */ code
```

Javadoc comments ... valid between identifier pair /** and */, used to create API documentation


public static void main(String[]) ... required initial method of a Java program, has to be there. always. dont argue.


method overloading ... Java allows multiple methods of the same name, which receive different input arguments. depending on the input the appropriate corresponding method will be chosen automatically and executed.

public class Name1{							... declaration of a class
public static void main(String[] args){		... declaration of a method


## Modifier:
public/private	... visibility modifier, defines if a class or a method is visible to other classes.
static		... defines, if this method requires an object of the class it belongs to, or if it is a property of a class and can be called without an instance of the object.
void		... defines, that a method does not return a value after execution

Note: if the main method of a class is supposed to return something, there is a specific method for this: java.lang.System.exit(int status). This method ends an application and returns the exit int "status" to the command line - status: 0 ... successful end, 0>status<=255 ... error. In unix this return value can be retrieved by $?, with cmd.exe in windows using %ERRORLEVEL%

Variables ... a variable actually is a reserved piece of memory and uses a fixed number of bytes depending on the datatype. each variable has a fixed datatype.


## Primitive datatypes:

Java is a type based programming language. Every variable and every literal has a type which will not change during runtime. The following types are called primitive datatypes.

	datatype	memory		value
	boolean		... 1bit	true / false
	char		... 16bit	16bit unicode (0x0000 ... 0xFFFF) ... can contain exactly one character
	byte		... 8bit	-2⁷ to 2⁷-1 (-128 to 127)
	short		... 16bit	-2¹⁵ to 2¹⁵-1 (-32.768 to 32.767)
	int			... 32bit	-2³¹ to 2³¹-1 (–2.147.483.648 ... 2.147.483.647)
	long		... 64bit	–2^63 bis 2^63 – 1 (–9.223.372.036.854.775.808 ... 9.223.372.036.854.775.807)
	float		... 32bit	1,40239846E-45f ... 3,40282347E+38f
	double		... 64bit	4,94065645841246544E-324 ... 1,79769131486231570E+308

String is not a primitive datatype, but a reference type (... reference to an object). Can be used similar to a primitive datatype.

Declaration of variables
To store information, we use variables. To use variables, they have to be declarated first.

``` java
	int age;		// statement to declare the variable "age" of datatype int
	String name;	// statement to declare the String reference "name"
```

Declared variables are not automatically initialized with null or empty values, every variable has to be initialized before it can be used at all.


variables can be declared and immediately initialized with data:

``` java
	int age = 48;
	String name = "Mr Pookie"
```

variables of the same type can be declared and initialized at the same time:

``` java
	String	person1 = "Number one", person2 = "Number two";
```

Strings and numbers can be easily concatenated; non-Strings will be automatically converted to String:

``` java
	int concNum = 12;
	String concStr = "Concatenate string with number " + concNum;
```

Read from text input:
``` java
	String s = new java.util.Scanner(System.in).nextLine();
	int i = new java.util.Scanner(System.in).nextInt();
```

Using plain numbers in code:
Since everything needs to allocate memory, plain integer numbers will always be assumed to be int. If longer numbers are needed the datatype has to be declared:

	e.g. 98123987123987L ... L for long

To make literal numbers more readable underscore is supported within numbers:

	1234567890L or 12_3456_7890L


## Operands and operators:
	operand ... literals and variables
	operators ... connect operands

	=	... assignment operator ... assigning a value to a variable
	==	... binary comparison operator ... result is true or false

	Multiple assignments	... 	e.g. a = b = c = 0;


### Arithmetic operators
	+,-,*,/		... self explanatory
	%			... modulo operator 

After an arithmetic operation the resulting datatype is of the largest datatype of the involved operands.

e.g.	1.0 / 3	leads to 0.33333333 (datatype double)
		1 / 3.0	leads to 0.33333333 (datatype double)
		1 / 3	leads to 0 (datatype int)


### Compound assignment operators: used to shorten syntax
	a = a + 2;	... a += 2; (a += "concatenate"; works as well)
	a = a * 2;	... a *= 2;
	a = a / 2;	... a /= 2;

But keep in mind:	a *= 2+3; is actually a = a * (2+3)

### Increment and decrement:
	i = i + 1	... i++
	j = j - 1	... j--


### Relational and comparison operators
	>, <, >=, <=
	instanceof ... testing of reference properties
	==, !=


### Logical operators
	!	... is not ... inverts boolean value of operand
	&&	... logical and ... only returns true if operands are both true
	||	... logical or ... returns true if any of the operands is true
	^	... XOR ... returns true, if exactly one of the operands is true, but not if both are true


# Typecasting
- implicit typecasting ... java automatically casts a type of the next higher class, if an assignment makes it necessary. e.g. short to long, if after an arithmetic calc the result requires long.
- explicit typecasting ... a user can force a type which can result in loss of information:
	e.g. 	int n = (int) 3.1415;	// n = 3

Note: typecasting has a very high priority and will be executed before any following arithmetic calculations; therefore use brackets when typecasting explicitly!
Note: when converting float to integer, the decimal point number will be discarded wihthout rounding.
Note: if datatypes smaller than int are arithmetically touched, the complier will convert them automatically to int. if the result should be byte or small, a cast is required!
	e.g. the only working way to add short and byte datatypes
		short s3 = (short)(s1 + s2);
		byte b3 = (byte)(b1 + b2);


# Control structures

## if clause
``` java
	if ( var1 operator var2 ) { /* add code */; }
```

### if else clause
``` java
	if ( var1 operator var2 ) { /* code */ }
	else { /* code */ }
```

### if else if clause
``` java
	if ( var1 operator var2 ) { /* code1 */ }
	else if ( clause2 ) { /* code2 */ }
	else { /* code3 */ }
```

## Conditional operator
	operator ? parts the boolean clause from the actual assignment
	operator : parts the two operands

	e.g. max = ( a > b ) ? a : b;

does the same as

	if ( a > b ) max = a;
	else max = b;

Useful code snipets:
	maximum of two operands:	a > b ? a : b;
	minimum of two operands:	a < b ? a : b;
	absolute value of an operand:	x >= 0 ? x : -x;


## switch clause
``` java
	switch( userValue ) {
		case( 'conditionValue1' ) {
			code1;
			break;	// without this break, the next clause would be accessed as well! usefull for fall through
		}
		case( 'conditionValue2' ) {
			code2;
			break;
		}
		case( 'conditionValue3' ) {
			code3;
			break;
		}
		default {
			/* if no clause is fulfilled, this will be executed
			   if any case before has no break statement, this will be
			   executed as well.
			*/
			code4;
		}
	}
```

Switch can only use integer values. boolean, long, float, double cannot be used. byte, short and Strings will be automatically cast to type int. Strings will therefore be compared by their integer values.



## Loops
### while loop
``` java
	while( condition ) {
		/* code 
		has to contain something
		which modifies a variable leading
		to an end of the while loop sometime
		in the future... */
	}
```

### do-while loop
``` java
	do {
		/* code including reaching the ending condition */
	}
	while( condition ); //IMPORTANT! note the semicolon after the while
```

### for loop
``` java
	for( int i = 1; i <= 10; i++ ) {
		code;
	}
```

for loops support multiple initialisations and in- or decrements:
``` java
	for ( int i = 1, j = 100; i <= 10; i++, j-- ) {
		code;
	}
```

Note! Try to avoid float and double as conditions. due to inaccuracies in floating point calculation within the loop it can happen, that the breaking condition can never be reached if there is a == or != condition. Use relational operators (<,>,<=, >=) instead.

``` java
	double d = 0.0;
	while( d != 1.0 ) {
		d += 0.1;
		System.out.println( d );
	}

	output:
	0.1
	...
	0.7
	0.7999999999999999
	0.8999999999999999
	0.9999999999999999
	1.0999999999999999
	1.2
```

Note! breaks can be used within loops. But within nested loops a break will only end the loop it resides in.


# Methods of a class

A method consists of header and corpus. The header consists of return type (e.g. void), method name (e.g. main) and an optional list of parameters (e.g. String[] args)

Static methods are also called class methods. They are not bound to an object of a class and can be called without an instance of the class object.
If the definition of a method and the call of the method are in the same class, the method can be called without the prefix. If the method is called from another class
the name of the method parent class has to be included.

``` java
	class FriendlyGreeter {
		static void greet() {
			System.out.println( "Hallo und so!" );
		}
		public static void main( String[] args ) {
			greet(); 
		}
	}

	// from outside of class FriendlyGreeter
	FriendlyGreeter.greet();
```

If a method contains parameters, their type has to be declared in the method declaration. These parameters will be used as local variables further down in th corpus of the method.

``` java
	static void printMax( double a, double b ) { code; }
```

Note: When variables are handed over to a method, the method receives the VALUE contained by the variable, NOT the REFERENCE to the original memory location. Except of course the value of an object is a reference e.g. in arrays.


# Methods with dynamic number of parameters
Methods can also be defined with a dynamic number of parameters using the "..." operator after the parameter type declaration

``` java
	static int mySum( int firstNum, int... ListOfInts ){
		//doStuff
	}

	public static void main(String[] args){
		int x = mySum( 1,2,3,4,5 );				// dynamic number of parameters, separated by ","
		int y = mySum( 2,3,4 );
	}
```

Note! A dynamic number of parameters declaration has to be the last declaration in the parameter list! Otherwise the compiler is not able to distinguish which parameter is specifically declarated and which is aready a dynamic one.


# Premature end of a method call
If a method is called normally, it will execute all of its corpus code, if specified return appropriate variables and end. The return stastement can be also used to end a method call prematurely.

``` java
	class FriendlyGreeter {
		static void greet() {
			if( condition ){
				return;			// if the condition is met, the method will end before printing the message.
			}
			System.out.println( "Hallo und so!" );
		}
		public static void main( String[] args ) {
			greet();
		}
	}
```


# Methods with return values
Methods with return values require a definition of the return class (sthg of type not void) and a return statement with the return value; This return value has to be adressed by the calling class/method, otherwise there will be errors.

``` java
	static String returnStuff(){
		return "ThisIs Stuff";
	}
```


# Blocks and visibility and scope (Geltungsbereich) of variables
To logically structure parts of code, multiple blocks of code can be defined within methods using the { } brackets. Variables can only be seen and accessed within their defined scope. Usually a variable is usable within its own and deeper nested blocks. Furthermore a variable is only functional within the block its been declared and can only be accessed, if its been initialized.

``` java
	public class Scope {
		public static void main( String[] args ) {
			int foo = 0;
			{
				int bar = 0;			// bar is only valid within this block, its a local variable
				System.out.println( bar );
				System.out.println( foo );
				double foo = 0.0;			// Error: Duplicate local variable foo
			}
			System.out.println( foo );
			System.out.println( bar ); // Error: bar cannot be resolved
			}

		static void qux() {
			int foo, baz;		// foo is unrelated to foo from main()
			{
				int baz;		// Error: Duplicate local variable baz
			}
		}
	}
```


# Overloading
As already stated above, methods can be overloaded - println automatically decides which of the declared methods is the appropriate for the input given to println().
Furthermore, overloading also works with different numbers of input parameters:

``` java
	static double tax(double cost, double taxrate){
		return cost * taxrate / 100;
	}
	static double tax(double cost){
		return tax(cost, 19.0);
	}
```


# final local variables and references
parameters and local variables can be declared with modifier final. this protects the value of this variable from any changes after the initialization.

``` java
	static void blub(final int c){
		final int b = 3;
		int a = 1;
		a = 5;
		b = 5;	// leads to an error
		c = 5;	// leads to an error
	}
```

The initialisation of a final variable does not have to occur at the same time like the declaration, but at the moment of the initialisation, the variable is finalized.

References handed over to a method can also be declared as final. In this case the method cannot change the reference in any way. What does work is changing the content of a referenced object.

``` java
	static void clear( final int a, final Point p ) {
		a = 10;				// Error: The final local variable a cannot be changed
		p.x = 10;			// Even though the reference to the object p is final, the contents of the referenced object can be modified.
		p = new Point();	// Error: The final local variable p cannot be assigned. ... the reference p to the corresponding object cannot be changed.
	}
```


# recursion

Calling a method from within itself. This of course can lead to infinite calls of the same method, if the recursion is not properly terminated. To terminate, use return to end the method for good. This of course also means, that there is a stack of active methods until a termination has been reached. only then each method will be terminated in the reverse order compared to how they were called. Code after the recursive method call will only be executed, after the recursive method below has ended.

``` java
	static void goDown( int n ) { // this codes for a count down ... the current number will be printed and the method will be called again will n-1
		if ( n <= 0 )
			return;  // end of recursion
		System.out.print( n + ", " );
		goDown( n – 1 );
		System.out.print( "End current recursion " + n + ", " );
	}
```

This leads to the fact, that there is a call back stack referring to which recursion of a method is the parent recursion method. The size of this stack is usually capped to avoid infinite recursion loops. The stack size can be manually adjusted, if larger stacks are required.

Due to the necessity of a stack it can be performance optimized to use loops instead.


# Classes and Objects (YAY! \o/)

Objects in software development show three main features:
- identity
- state
- behavior

Objects are unchangeable as long as an object exists.
Objects take care of memory and computation of their inherent properties, variables and methods.


## Properties of a class
Classes are the most important aspect of object oriented programming languages. Each object is an instance of a class.

A class declaration states the properties of a class, the properties are:
- attributes (what an object has ... implemented via variables)
- operations (what an object is able to do ... implemented via methods)

To design a class well in respect to attributes and operations, the most intuitive approach would be to say "I am ..." for attributes and "I can ..." for operations. (try with apple, car, etc.). To properly define a data model for classes, UML is the tool of choice.


## Creating an object
Objects in Java have to be explicitly created using the new operator:

``` java
	java.awt.Point p;			// p ... reference variable, required to access the later initialized object
	p = new java.awt.Point();

or:

	java.awt.Point p = new java.awt.Point(); // declaration and initialization of the object in one line
```


This creates a new object of class Point, the empty brackets are required for accessing the constructor of the class to initialize the object.
If there was enough free memory and there was no error during the initialization, new returns a reference to the new object.


## Heap, stack, garbage collector

Heap ... maximum piece of the overal available memory, which java is allowed to use. required so that the JRE does not use up memory thats i.e. required to run the operating system of a machine.

If the JRE receives the request to create a new object, it reserves as much memory as the objects attributes will need from the Heap. If there is no sufficient memory, Java tries to free up memory automatically from its own heap, if there is no memory left even than, an OutOfMemoryError occurs and terminates the program.

The garbage collector automatically checks all objects for existing references in the heap on a regular basis. If no references are left, the memory corresponding to this object is released.


## Accessing object attributes and methods

Attributes and methods of the object of a class can be accessed using the point operator:

``` java
 	java.awt.Point p = new java.awt.Point();
	p.x = 1;		// accessing/writing to attribute x
	p.y = 2 + p.x;  // accessing/reading from x, writing to y
```


# Java packages and the java standard library
A package is a thematically coherent group of types. Packages may contain sub-packages, to access a subpackage, its name is separate by the point operater from the name of its parent.

``` java
	java.io.File  // File is a subpackage within the package io
```

The java standard library contains many packages containing hundreds of classes. java standard library packages can be accessed by java. and javax.


To use contents of the library at runtime, the compiler has to know beforehand to include the required packages into the compiled bytecode and therefore the whole path of a library package method has to be provided.

``` java
	class usePoint {
		public static void main( String[] args )
		{
			java.awt.Point p = new java.awt.Point();
		}
	}
```

To use methods from packages without stating the whole package path over and over again, the import command used before any class declaration can simplify things:

``` java
	import java.awt.Point;

	class usePoint {
		public static void main(String[] args){
			Point p = new Point();
		}
	}
```

Importing all types of a package:

``` java
 	import java.awt.*;		// imports all types contained in package java.awt
	import java.*;			// does not import anything, because java. contains only packages but no types.
```

NOTE: if packages contain two or more types of an identical name, an import from these will lead to an error. In this case only one of the types can be imported, the other have to be accessed using the full path.

Designing packages:
Packages are mostly also physical entities, the subfolders usually correspond to subpackages. Therefore special characters should not be used when naming packages.

``` java
	com.tutego ... physically there is a folder com containing all folders corresponding to subpackages e.g. tutego: com/tutego
	com.tutego.DatePrinter() ... resides in com/tutego/DatePrinter.class
```

To create class DatePrinter within package tutego, it has to physically reside in folder com/tutego and contains a package declaration at the starting position of the source code.

``` java
	package com.tutego;

	import java.util.Date;

	public class DatePrinter {
	public static void printCurrentDate() {
			System.out.printf( "%tD%n", new Date() );
		}
	}
```

Classes not defined specifically within its own package will reside within the "default package". But classes from proper packages will not be able to use classes from the default package. that might work for development but its a good idea to always create proper packages.


## Import static methods
If a class provides static methods, they can be imported and used similar to classes from packages:

``` java
	import static javax.swing.JOptionPane.showInputDialog;
	import static java.lang.Integer.parseInt;
	import static java.lang.Math.max;
	import static java.lang.Math.min;
		
	class StaticImport{
		public static void main(String[] args){
			int i = parseInt(showInputDialog("number"));
		}
	}
```

Static methods can also be imported on bulk:

``` java
	import static java.lang.Math.*;

instead of:

	import static java.lang.Math.max;
	import static java.lang.Math.min;
```


## Working with references

### The null reference
Java knows three different references: null, this and super. null is used for the initialisation of reference variables (we remember ... the reference to a newly created object). The null reference has no type. It acts like it is a subtype of every other type. Its basically required to state, that a reference variable does not refer to an actual object at the moment. Within lists or trees i.e. it can be used to state that there are no valid children at the moment.

since null does not refere to an existing object, it cannot be used to access a method or attribute. If such an access occurs, JVM triggers a NullPointerException.

### check for null reference

``` java
	if(object == null)
		/* do stuff */
	else
		/* do other stuff */
```

NOTE:

	if (obj != null && obj.isEmpty())	// this actually works even if obj is null, because the whole term will lead to false 
										// as soon as the first clause of the condition has been evaluated.
										// the other way around would lead to a NullPointerException if obj is null


### Assigning references
References are used to access the referenced object. There can be multiple copies of a reference with different names all refering to the same object.

``` java
	Point p = new Point();
	Point q = p;			// both p and q are references to the same Point object
```

Since there is only one object, if reference p changes the contents of its object, then reference q will access the changes made by reference p - since they refere to the same object.

When handing over a reference to a method, it is not the object itself that is handed over. This reference is actually a number which decodes to the memory position where the object resides. If we overwrite a handed down reference within a method with a new object and modify its properties, the original object is still present in the parent under the same name.

``` java
	import java.awt.Point;

	public class JavaIsAlwaysCallByValue {
		static void clear( Point p ) {
			p = new Point();
		}

		public static void main( String[] args ) {
			Point p = new Point( 10, 20 );
			clear( p );
			System.out.println( p ); // java.awt.Point[x=10,y=20]
		}
	}
```

if we want to reset the referenced object in a child method with the changes also taking part in the main method, we need to access the properties itself.

``` java
	static void clear( Point p ) {
		p.x = p.y = 0;
	}
```


## Identity of objects
When using == and != with objects, whats actually tested is, if the value of the provided references are the same so that the references both point to the same object. This test actually checks if the references are the same, but not, if the contents of two different objects are identical.

To check for identical content of two different objects, method equals is used.

``` java
	Point p = new Point( 10, 10 );
	Point q = new Point( 10, 10 );
	System.out.println( p == q );	// false
	System.out.println( p.equals(q) ); // true. Da symmetrisch auch q.equals(p)
```


# Arrays

Arrays can only contain values of the same datatype:
- primitive datatypes like int, long, char etc
- reference types
- reference types of exisiting arrays for multidimensional arrays

To work with arrays, there are three steps
- declaration of an array variable
- initialization of an array variable
- access (read and write)


## Declaration and initiation of an array

Requires the declaration of the used datatype and addition of [] at the end of the type declaration.

``` java
	int[] primes;
	Point[] points;
```

The brackets can also be used after the array name:

``` java
	int primes[];
	Point points[];
```

Since arrays are objects, they have to be initialized using the "new" operator. Furthermore, the length of the array has to be specified at the initialization:

``` java
	int[] values;
	values = new int[ 10 ];	// initialized array object with reference "values" and array size "10"
or
	int[] values = new int[ 10 ];
```

Using the operators == and != will only compare references but not content of arrays (since they are objects...).


## Adding values to a declared but not initialized array

``` java
	int[] primes = { 2, 3, 5, 7, 7+4 };
	String[] string = { "Cat", "Mouse", "hillarious", "dog".toUpperCase() };
or
	int[] primes;
	primes = new int[] { 2, 3, 5, 7, 7+4 }
```

The second version is useful to hand new arrays to methods:

``` java
	avg( new double[]{ 1.23, 4.94, 9.33, 3.91, 6.34 } );
```


## Accessing values of an array

Array can be accessed the index of the contained values, the index itself is always a positive integer.

NOTE! In java an array starts with index 0!

``` java
	char[] name = { 'C', 'h', 'r', 'i', 's' };
	char first = name[ 0 ];	// C
	char last = name[ name.length – 1 ]; // s
```

Note! if we try to access an index thats negative or too large, there will be an IndexOutOfBoundsException during runtime:

``` java
	int[] array = new int[ 100 ];
	array[ –10 ] = 1; // Error
	array[ 100 ] = 1; // Error
```

The length of an array can be accessed using the "length" attribute:

``` java
	int[] primes = { 2, 3, 5, 7, 7+4 };
	System.out.println( primes.length );
```


## Multidimensional arrays

``` java
	int[][] myMatrix = new int[2][4];								// declares and initializes a 2x4 matrix-like 2 dimensional array
	int[][] myValueMatrix = {{1.1,1.2,1.3,1.4}, {2.1,2.2,2.3,2.4}};	// declares and initializes a 2 dimensional array with values
	int[][] myMultiArray = {{1,2}, {3,4,5,6}, {7}};					// a multidimensional array does not need to be rectangular
```

To access multidimensional arrays, each individual index is required (remember, index starts with 0):

``` java
	myMultiArray[1][2];
```

The method length always refers to the array level its used with:

``` java
	myMultiArray.length;			// returns 3, the first level has 3 array elements
	myMultiArray[0].length;			// returns 2, the array contained in the first field of the main array has 2 elements
	myMultiArray[1].length;			// returns 4
```

Initialisation of Subarrays in multidimensional arrays ... since multidimensional arrays dont have to be rectangular, we might need to specifically initialize subarrays, if we cannot initialize them at the declaration.

``` java
	int[][] myUndefMultiArray = new int[12][];	// initializes only the first level array with a length of 12
	myUndefMultiArray[0] = new int[5];			// The first sub array of our multiarray has been initialized with a length of 5
	myUndefMultiArray[1] = new int[3];			// The first sub array of our multiarray has been initialized with a length of 3
	//etc.
```

		
## Multiple return values using arrays

Arrays can be used to implement multiple return values from methods.

``` java
	public class MultipleReturnValues {
		static int[] productAndSum( int a, int b ) {
			return new int[]{ a * b, a + b };			// returns array with multiple values
		}
			
		public static void main( String[] args ) {
				System.out.println( productAndSum(9, 3)[ 1 ] );	// prints sum of 9 and 3 (second value of the returned array)
			}
		}
```


## Cloning arrays and copying array values

Using the array method.clone(), a copy of an array can be easily obtained.

NOTE! clone only copies the first layer of a multi dimensional array. The further layers still are only copies of the references and therefore are no new objects but rather reference to the same subobjects as in the original multidimensional array.

``` java
	int[] sourceArray = new int[ 6 ];
	sourceArray[ 0 ] = 4711;
	int[] targetArray = sourceArray.clone();
	System.out.println( targetArray.length ); // 6
	System.out.println( targetArray[ 0 ] ); // 4711
```

A method to duplicate, move, etc the actual values within an array or onto other initialized arrays is java.lang.System.arraycopy()

``` java
	System.arraycopy( f, 1, f, 0, f.length – 1 ); // copy all values of array f to the left
```


To copy only a subpart of an array the methods copyOf and copyOfRange are available; this also supports to change the size of the new array.
``` java
	String[] snow = { "Neuschnee", "Altschnee", "Harsch", "Firn" };
	String[] snow1 = Arrays.copyOf( snow, 2 ); // [Neuschnee, Altschnee]
	String[] snow2 = Arrays.copyOf( snow, 5 ); // [Neuschnee, Altschnee, Harsch, Firn, null]
	String[] snow3 = Arrays.copyOfRange( snow, 2, 4 ); // [Harsch, Firn]
	String[] snow4 = Arrays.copyOfRange( snow, 2, 5 ); // [Harsch, Firn, null]
```


## String of an array

The method Arrays.toString() converts the content of an array to String:

``` java
	String[] dogs = {"rottweiler", "german shepherd", "hush puppy"};
	String myDogsString = Array.toString(dogs);
```


## Sorting an array

``` java
	import java.utils.Arrays;

	int[] numbers = { –1, 3, –10, 9, 3 };
	String[] names = { "Xanten", "Alpen", "Wesel" };
	Arrays.sort( numbers );
	Arrays.sort( names );
	System.out.println( Arrays.toString( numbers ) ); // [-10, –1, 3, 3, 9]
	System.out.println( Arrays.toString( names ) ); // [Alpen, Wesel, Xanten]
```

Java 8 supports parallel sorting for large arrays. For this multiple parallel threads which can result in shorter computational times - but only if the arrays are really large. In detail the original array is split up into smaller arrays which are presorted in parallel

``` java
	Arrays.parallelSort( numbers );
```


## Comparing contents of arrays

Array.equals(array1, array2)					compares the content of two arrays, does not include multidimensional arrays
Array.deepEquals(multiArray1, multiArray2)		includes multidimensional arrays as well.


## Auto-fill of fields of an array

``` java
	char[] chars = new char[ 4 ];
	Arrays.fill( chars, '*' );						// automatically fills each field of the whole array with '*'
	System.out.println( Arrays.toString( chars ) ); // [*, *, *, *]
```

It is also possible to autofill a subset of array fields from startIdx to endIdx


## Binary search (only for sorted arrays!)

Binary search can be used to quickly identify the position of a value within an array. It returns an int value corresponding to the index of the value un question.
NOTE! this search only works, if the array is sorted!

``` java
	int[] numbers = { 1, 10, 100, 1000 };
	System.out.println( Arrays.binarySearch( numbers, 100 ) ); // 2
```

If a value is not found within the sorted array, a value will be returned corresponding to an idx where the value could be entered to keep the sorting.


## Searching unsorted arrays

Usually if an unsorted array is to be searched, a loop has to be programmed. The method Arrays.asList() can be used to treat arrays like lists and enables the use of list methods for arrays:

``` java
	Arrays.asList(myArray).contains();
	Arrays.asList(myArray).equals();
	Arrays.asList(myArray).subList();
```


## Array loop

There is a special short syntax to use a for loop to iterate through all fields of an array - for (type placeHolder : array)

``` java
	int[] myArray = {4,5,10,28};
	for (int i : myArray){
		System.out.println(i);
	}
```


# Generics
As stated above, Java is a type based language, each variable has a type that will not change during runtime. Unfortunately thats not true for every circumstance:

	List myPointsList; // myPointsList is now of type List, but its unclear, to which objects the list will reference.

Generics help solve this problem, it can be stated which type of objects a list will contain:

	List<Point> myPointsList;	// we now have a List which is only able to contain objects of type Point


# Annotations
static and public/private statements are metadata for the compiler to define how the following code should be treated. Annotiations provide a method to add metadata that provide information for java libraries during runtime. Annotations are used at the point of Type and Method declarations; Annotations require the key character @ at the beginning

	@Override public String toString()

java.lang provides five different annotation types:
- @Override				... overrides a method of the same name from the super class (?)
- @Deprecated			... functions as a tag for methods which are outdated and should be replaced by newer version. Elicits a corresponding compiler message.
	e.g.	@Deprecated
			public void myOldMethod() { ... }

- @SuppressWarnings		... suppresses compiler warnings, all or specific
``` java
	@SuppressWarnings("all")				// will suppress all warnings indiscriminately
		public class SuppressAllWarnings
		{
			public static void main( String[] args )
			{
				java.util.ArrayList list1 = new java.util.ArrayList();
				list1.add( "SuppressWarnings" );

or	@SuppressWarnings( { "rawtypes", "unchecked" } )	// will suppress only warnings of type rawtypes and unchecked
		public class SuppressSomeWarnings
		{
			public static void main( String[] args )
			{
				java.util.ArrayList list1 = new java.util.ArrayList();
				list1.add( "SuppressWarnings" );
```

	The following SuppressWarnings are available:
		all, 	rawtypes, 	unchecked
		deprecation ... for deprecated java elements
		incomplete-switch ... for switch-case
		resource ... for not closed AutoClosable interfaces
		unused ... for unused elements e.g. not accessed private methods

- @SafeVarargs			... special tag for methods using variable number of arguments and generic argument types
- @FunctionalInterface	... used by interfaces with exactly one method



# Dealing with Strings!

## Unicode / UTF-16
Java uses the unicode standard 6.2. and internally the UTF-16 coding (requiring 2-4byte*number of characters).

## Escape sequences
To access special functions within a string, the following escape sequences are supported:
	\b	... backspace				\n	... newline
	\f	... formfeed				\r	... carriage return
	\t	... horizontal tab			\\	... backslash
	\'	... single quite			\"	... double quote


## Unicode escape sequences
To use unicode characters within strings the unicode hexadecimal value is used after a backslash:
	e.g. 	Ä ...\u00c4, ä ... \u00e4


## character strings

All strings in java are private arrays of characters. There are three different classes how strings of characters can be handled in java:

- class String ... this deals with immutable strings - strings of characters, which are immutable during runtime. Every manipulation of a string object creates a new string object. Therefore strings are threadsafe.

- classes StringBuilder and StringBuffer ... these classes refer to dynamically changeable character strings.

Strings are indicated by double quotes. Everything within double quotes is automatically of type String. This also leads to the fact, that the new operator is not required when initializing a new string:

``` java
	String s = new String("hurra");		// error
	String s = "hurra";					// this works
```

Strings can be concatenated by "+", number values will be automatically converted to characters.
``` java
	String s = "Age: "+ 39 +", Height: "+ 1.61;
```

Note: concatenation using the + operator is not performant, avoid this in loops.


## Useful string manipulation methods

java.lang.Character.isXXX(char c) ... Boolean test for specific characters: Character.isDigit(), .isLetter, .isWhitespace, .isLetterOrDigit, .isLowerCase, .isUpperCase
``` java
	Character.isDigit('0');	//true
	Character.isDigit('-');	//true
```

java.lang.Character.charAt(idx) ... retrieving character from string at index position idx
``` java
	String input = new java.util.Scanner( System.in ).nextLine();
	char c = input.charAt(10);
```
Note: if the provided index is negative or larger than the length-1 of the string, a StringIndexOutOfBoundsException will be triggered

java.lang.Character.toLowerCase(char c) & .toUpperCase() ... convert input character to lower/upper case. Works for single characters and strings.
	myString.length()					... length of a string, string has to be initialized
	myString.isEmpty()					... checks if string object contains characters at all, string has to be initialized
Note: To check if a string is null has to be done manually by a boolean comparison (if myString == null)

	myString.contains("substring");		... returns true or false if the substring is contained within myString.
Note! this works only if substring is larger than one character!

	myString.indexOf('e');				... returns the index of the first occurence of character 'e' ... can be used to search for a single character within a string.
	myString.indexOf('e', 20);			... overloaded method indexOf ... enables to start the search from a specified index

Note: the above search methods search from left to right.

	myString.lastIndexOf('e'[, 20]);	... starts searching from the right of the string. has an overloaded method to supply starting index.

	myStrObj1.equals(myStrObj2);		... compares stringObject1 with stringObject2 for identical content. Works for String, since String is an Obect of its own 
										(... its an inherited method from class object).

	myStr1.equalsIgnoreCase(myStr2);	... compares two strings while ignoring casing.

	myStr1.compareTo(myStr2);			... checks the alphanumerical order of the two compared strings.
										return value == 0 ... the strings are identical
										return value < 0 ... str1 is alphanumerical before str2
										return value > 0 ... str1 is alphanumerical after str2
Note: works only well if the strings do not include special characters (ä, ö, etc), since the comparison is based on the internal numerical coding of Unicode-characters.
			if special characters are used, the Collator class should be used for comparison.

	myStr1.compareToIgnoreCase(myStr2)	... like compareTo() but ignoring casing.

	myStr.startsWith("Hurra");
	myStr.endsWith("Hurra");
	myStr.regionMatches("alternateStr");	... checks if any part of alternateStr is contained within myStr. But this method requires exact positions of 
											the two phrases to be compared. This method also supports caseinsensitive comparison as an overloaded method.
	String s = "I like coffee";
	boolean b = s.regionMatches(7, "coffee makes me sleepy.", 0, 6);	// compares phrase of length 6 from position 7 in s to phrase of length 6 
																		// from position 0 in provided second string.

	myStr.matches()						... matches using regular expression

	myStr.substr(int idx);				... retrieves everything from the idx position in myStr to the end of myStr. 
Note: remember, String is an array and arrays start with idx 0!

	myStr.substr(int idx, int stopIdx);			... retrieves everything from the idx position in myStr until stopIdx.


	myStr.toCharArray();				... converts myStr to an explicit array containing each character in its own field. can be used to iterate over a string.
``` java
	String myStr = "Ich sag nur hurra die Gams";
			for ( char currChar : myStr.toCharArray() )		// nice solution, but doubles memory, since a new array containing contents of myString is created.
				System.out.println( currChar );
```

	myStr1.concat(myStr2);			... concatenates myStr1 with myStr2 and works faster than the + operator.
``` java
	String s1 = "The current date is: ";
	String s2 = new Date().toString();
	String s3 = s1.concat( s2 );
```

myStr.join(CharSequence delimiter, CharSequence... elements);	... creates a sequence of elements, that are seperated by a defined delimiter:
``` java
	String s = String.join( ";", "One", "Two", "Three", "Four", "Five" );
```

	myStr.trim();			... removes all leading and trailing whitespaces.

	myStr.replace(char oldChar, char newChar);						... replaces all characters oldChar with character newChar. This returns the reference to a 
																		new string object. The original content of the object myStr is not modified.
	myStr.replace(CharSequence target, CharSequence replacement);	... like replace but replaces substrings rather than single characters.

	myStr.replaceAll(String regex, String replacement);					... replaces all string sequences that fit to a regular expression.
	myStr.replaceFirst(String regex, String replacement);				... like replaceAll, but replaces only the first occurrence

	myStr.getChars(int scrBegin, int scrEnd, char[] newCharArray, int newCharArrayIdx)	... returns an array, which fields contain all characters 
																							from scrBegin to scrEnd of the string myStr.

## Changeable character strings StringBuilder & StringBuffer

In contrast to String objects, the contents of StringBuilder and StringBuffer can be modified during runtime. Replace methods for example change the content of the object and do not create a new object, returning only a reference to the new object.

StringBuilder myStrB = new StringBuilder();

when a new StringBuilder or StringBuffer object is created, memory for 16 characters is reserved. If more memory is required along the line, the buffer will be adjusted automatically. The memory can also be allocated manually:
	StringBuffer/Builder();					... creates a new StringBuffer / Builder object and reserves 16 characters of memory. 
	 											If more memory is required later on, it will be allocated automatically.
	StringBuffer/Builder(int defLength);	... creates a new StringBuffer/Builder object and allocates defLength number of characters of memory.
	StringBuffer/Builder(String str);		... creates a new StringBuffer/Builder object with the contents of String str and allocates memory for 16 additional characters.
	StringBuffer/Builder(CharSequence seq)	... like above but using the contents of a CharSequence.

The following methods can be used with StringBuilder/Buffer objects:
	myStrBuilder.length();
	myStrBuilder.charAt(int);
	myStrBuilder.getChars();
	myStrBuilder.substring(int start);
	myStrBuilder.substring(int start, int end);


	myStrBuilder.append(appendStr);			... appends the string appendStr to the existing myStrBuilder string
	Note!	.append() supports a number of input types e.g. boolean, char, char[], CharSequence, double, float, int, Object, String

	myStrBuilder.toString();				... creates a String object from the changeable StringBuilder object and returns the reference to the String object.

	myStrBuilder.setCharA(int idx, char mySetChar);		... sets or replaces the character at index position idx with mySetCharacter
	myStrBuilder.insert(int offste, element);			... inserts the content of element at index position offset into the existing StringBuffer/Builder string.
	myStrBuilder.delete(int startIdx, int endIdx);		... deletes the content of an existing StringBuffer/Builder from index startIdx to endIdx. Throws an error,
															if any index is out of bound.
	Note! the endIdx is not included! endIdx is the position where the delete statement HAS already ended.

	myStrBuilder.replace(int startIdx, int endIdx, String str);		... deletes from startIdx to endIdx and inserts the content of string str at position startIdx.
	Note! Has the same endIdx restriction as above.

	myStrBuilder.reverse();			... reverts the string.

	myStrBuilder.capacity();		... tells the length of the StringBuilder/Buffer content plus the additionally reserved memory for manipulation.

	myStrBuilder.setLength(int newLength);		... sets the length of the StringBuilder/Buffer object to newLength. If newLength is smaller than the actual content,
													then the rest of the content will be discarded. if newLength is smaller, the rest of the string will 
													be filled with zeros.

Why can this be usefull?
``` java
	String[] elements = { "Manila", "Cebu", "Ipil" };
	String separator = ", ";
	StringBuilder sb = new StringBuilder();
	for ( String elem : elements )
		sb.append( elem ).append( separator );
	sb.setLength( sb.length() - separator.length() );
	System.out.println( sb );		// Manila, Cebu, Ipil
```

Comparing the content of a String with a StringBuilder/Buffer:
``` java
	String s = "wie dem auch sein mag";
	StringBuffer sb = new StringBuffer("Wie dem auch sein mag");
	System.out.println( s.equals(sb) );		// false
	System.out.println( s.equals(sb.toString()) );		// true
	System.out.println( s.contentEquals(sb) );		// true
```

Comparing the contents of StringBuilder/Buffer objects with each other requires a conversion to String:
``` java
	StringBuilder sb1 = new StringBuilder( "www.tutego.de" );
	StringBuilder sb2 = new StringBuilder( "www.tutego.de" );
	System.out.println( sb1.equals( sb2 ) ); // false
	System.out.println( sb1.toString().equals( sb2.toString() ) ); // true
	System.out.println( sb1.toString().contentEquals( sb2 ) ); // true
```

## java.lang.CharSequence as basic type

String, StringBuilder and StringBuffer all have CharSequence as parent class. This means, that CharSequence has fewer specialized methods, but if interchangability of content is required, then conversion of StringBuilder/Buffer to String will always cost additional resources, so it does make sense to use CharSequence as basic type in some cases, if the additional funcionality is not required. Type CharSequence leads to unchangeable, read only sequences.

CharSequence provides the following methods (return type after the colon):
- charAt(int idx): char
- length(): int
- subsequence(int startIdx, int endIdx): CharSequence
- toString(): String
- default IntStream chars()			// Java8 only
- default IntStream codePoints()	// Java8 only


## StringJoiner:
StringJoiner is a type which has CharSequence as parent class. It can be used to concatenate String expression. 

	StringJoiner sj = new StringJoiner( ", " );
	sj.add( "1" ).add( "2" ).add( "3" );		// The add method easily joins Strings using a defined delimiter
	System.out.println(sj.toString);			// 1, 2, 3

Merging two StringJoiner datasets
 	myStrJ1.merge(myStrJ2);			// merges two StrinJoiner Datasets

Using Prefix and Suffix:
	StringJoiner sj = new StringJoiner( ", ", "{", "}");	// defines delimiter, starting character { (prefix) and ending character } (suffix)


## String conversions
### Conversion of anything to String

The toString() method has already been described above. To retrieve a String object from any other primitive datatype or object the String.valueOf() method can be used.
 String.valueOf() actually uses the .toString() method that each class is using.

``` java
	String str1 = String.valueOf( 1 );						// int to string
	String str2 = String.valueOf( 1.225 );					// float to string
	String str1 = String.valueOf( 'c' );					// char to string
	String str1 = String.valueOf( new java.util.Date() );	// date object to string
```

### Conversion of String to primitive datatypes:
java provides a conversion for every primitive datatype using the syntax XXX.parseXXX(String s), where XXX corresponds to the primitive datatype.
``` java
	Boolean.parseBoolean(String s);
	Byte.parseByte(String s);
	Float.parseFloat(String s);
	/* etc. */
```
Note: If a conversion cannot be done from String to required type, a NumberFormatException will be raised. e.g. when the decimal place holder is "," instead of "." in a float.
Note: the parseXXX methods do not include an automatic trim() ... leading and trailing whitespaces have to be taken care of manually.


### Conversion of String to binary, octal and hexadecimal
	Integer.toBinaryString( int i );
	Integer.toOctalString( int i );
	Integer.toHexString( int i );

``` java
	Integer.toHexString(15); //f
	Integer.toHexString(16); //10
```


## Disecting strings
String[] split(String regex[, int limit])	... splits provided string at every occurrence fitting the regular expression and returns a string array containing the 
												resulting parts. If a literal is to be used as a split pattern, then the method Pattern.quote should be used.
												The optional parameter limit can be used to define how often the split should be executed.

``` java
	String path = "www.tutego.com";
	String[] segs = path.split( Pattern.quote( "." ) );
	System.out.println( Arrays.toString(segs) ); // [www, tutego, com]
```


## Scanner class
Scanner can be used to deal with input from various different sources from String over InputStreams to reading files. If the input is of type Closable, the Scanner should be ended using the close() method, i.e. in the case of a Writer type.

Scanner can be used with types String, Path, File, InputStream, Readable, ReadableByteChannel.

``` java
	import java.io.File;
	import java.io.FileNotFoundException;
	import java.util.Scanner;
	public class ReadAllLines {
		public static void main( String[] args ) throws FileNotFoundException {
			try ( Scanner scanner = new Scanner( Paths.get( "EastOfJava.txt" ),
					StandardCharsets.ISO_8859_1.name() ) ) {
					while ( scanner.hasNextLine() )
						System.out.println( scanner.nextLine() );
			}
		}
	}
```

Scanner provides a multitude of the methods .hasNext<type>() and .next<type>():
``` java
	Scanner scanner = new Scanner( "tutego 12 1973 12,03 True 123456789000" );
	System.out.println( scanner.hasNext() );		// true
	System.out.println( scanner.next() );			// tutego
	System.out.println( scanner.hasNextByte() );	// true
	System.out.println( scanner.nextByte() );		// 12
	System.out.println( scanner.hasNextInt() );		// true
	System.out.println( scanner.nextInt() );		// 1973
```

Tokens which are not of interest can be skipped using the Scanner skip([Pattern/String] pattern) method.
Method useRadix(int) is able to change the base of numbers.


## Formating Strings
static String format(String format, Object... args)

	%[specifier]	... print as specified e.g. %x as hexadecimal
	%n ... Newline		%b ... boolean		%% ... % sign
	%s ... String		%c ... Unicode		%d ... decimal
	%x ... hexadecimal	%t ... date/time	%f ... float
	%e ... scientific notation

``` java
	String s = String.format( "Hi %s. There was a call from %s.", "Chris", "Joy" );
	System.out.println( s );	// Hi Chris. There was a call from Joy.
```

	Note! Its better to use System.lineSeparator() to get a newline instead of using %n.

System.out.printf() ... immediately prints in specified format. Is actually a forward to method format().
``` java
	for ( int i = 0x0; i <= 0xf; i++ )
		System.out.printf( "%x%n", i ); // prints hexadecimal from 0-16 at every new line
```

Using dates when formatting string:
%t always required a suffix which specifies the required format in more detail.
	e.g.	%tA , %ta	... full/abreviated name of the weekday
			%tB , %tb	... full/abreviated name of the month
			%tY			... year, using 4 digits
			%tR			... hours and minutes in format %tH:%tM
			etc			... there are many more ways to format date, look them up



# Writing classes
The declaration of a class may contain:
- attributes
- methods
- constructors
- classes and initializer
- inner classes, inner interfaces, inner listings

When writing classes, the following order is encouraged:
- class variables
- object variables
- constructors
- static methods
- setter/getter
- object methods


## Class declaration

``` java
	class Player { }	... // the compiler will provide the constructor, new Player() can be used to create an object of class Player.
```


## Attribute declaration

``` java
	class Player {
		String name;		// declaration of two attributes of the class player, will throw an error if used and not initialzed, since its a reference to an object
		String item = "";	// will not throw an error, since its initialized
		Boolean myBool;		// will return false, if not initiialized
		int myInt;			// will return 0 if not initialized, works for all numeric values and Char.
	}

	class Playground {		// declaration of a class with a main method to create and modify class Player
		public static void main( String[] args ){
			Player p = new Player();		// with this, method main of class Playground is dependent on class Player
			p.name = "Funny Playername";
			p.item = "slice of pie";
			System.out.printf( "%s is eating a %s. yum.", p.name, p.item );
		}
	}
```


## Namespace, visibility, lifecycle of class properties
Local variables are only valid within the block they were declared and initialized in. They cannot be used any longer as soon as the block ends and their visibility ends.
Object variables are accessible from the time they are declared using the new operator until the garbage collector removes the object. Object variables are visibile within the whole object and in all blocks, with the exception of static methods/initialisation blocks.


## Methods declaration
``` java
	class Player {
		String name = "";
		String item = "";

		void resetName() {		// this method has not return value
			name = "";			// replaces the content of the Player attribute name with an empty string
		}

		boolean compoundName() {	// this method returns a boolean value
			return (name == null) ? false : name.contains( " " );	checks, if the name of the player consists of two or more words delimited by a whitespace
		}
	}
```

UML diagram of the classes so far:
public is indicated by +, private by -, protected #, visible in package ~
static methods are underlined (I use _ after the visibility modifier)

	Player							<-----			Playground					// Playground depends on Player since it requires a Player object in main
	------											----------
	+ name											----------
	+ item											+ main(args: String[])
	------
	+ clearName()
	+ hasCompundName(): boolean


hasCompound() only reads the content of an object variable. clearName() changes the content of an object variable.


## "Covered" object variables
If a local variable in a method of a class carries the same name like an object attribute, then the local variable "covers" the object attribute.

``` java
	class Player {
		String name; // Object variable name
			
		void quote( String name ) {
			System.out.println( "'" + name + "'" ); // local parameter variable name covers the object variable name
		}
	}
```

The object variable is of course still there. To access it, the special reference "this" can be used.
``` java
	class Player {
		String name;
		void quote( String name ) {
			System.out.println( "'" + this.name + "'" );	// accessing object variable
			System.out.println( "'" + name + "'" );			// accessing parameter variable
		}
	}
```

"this" is mainly used when initializing object attributes.
``` java
	class Player {
		String name;

			void setName ( String name ) {
			this.name = name;			// object variable name is initialized with the object received by setName()
		}
	}
```

To be able to cascade methods, the method has to return the modified object - only then the method in the next cascade receives the correctly modified object
e.g.	to append one item after the other to a Player object:
``` java
	class Player {
		String name = "", item = "";
		Player name( String name ) {
			this.name = name;		// overwriting attribute name with local variable name
			return this;
		}
		String name() {
			return name;		// required to access attribute name
		}
		Player item( String item ) {
			this.item = item;
			return this;		// overwriting attribute item with local variable item
		}
		String item() {
			return item;		// required to access attribute item
		}
		String id() {
			return name + " likes " + item;
		}
	}
```

UML:
	Player
	------
	+ name
	+ item
	------
	+ name ( name: String ): Player
	+ name (): String
	+ item (item: String): Player
	+ item (): String
	+ id(): String

The following method call would add to the object attribute item or name:
	Player myPlayer = new Player;
	myPlayer.item("Bacon").name("Mr Hungrypants");
	System.out.println(myPlayer.item());
	System.out.println(myPlayer.id());

Note: usually setXXX and getXXX methods should be used without returning the object.


## Privacy and visibility
Within a class, all attributes and methods are visible to other methods. The privacy and visibility level to other classes has to be defined. There are four visibility modifier
- public
- private
- protected
- package visibility (results automatically from not using a visibility modifier like public)

### Public vs private:
public and private are usable for classes, constructors, methods, etc. Everything is visible to other classes. Is a class public but an attribute private, the outside cannot access this particular attribute. If a class is private and an attribute public, the outside cannot access the class and also not the attribute. As an example a class Password would implement everything public except for the attribute private String password. The class methods are able to access the attribute because they belong to the same class, but from the outside the attribute is not visibile and therefore also not accessible. The access is therefore restricted to methods of the class Password; here all eventualities when a password is allowed to be changed etc. can be defined and handled.

Using a mix of private and public attributes and methods can be used to control the data flow from an to objects. e.g.
- define a range of numbers allowed for a private attribute. to set new values, define a set method that checks if the input falls into the range. a public get method provides read access for everyone.
- define dependencies of attributes e.g. when entering the age, a second private attribute ofAge can be automatically set appropriately
- when defining set and get methods, logging can be easily implemented
- data modifying methods or attributes can be set private, if they are not useful for other classes, access to them can be restricted


## setter and getter according to the JavaBeans specification
Attributes are private, access is only available through setter (setXXX) and getter (getXXX) methods where XXX refers to the corresponding attribute.
	void setXXX(String xxx)		... setter, has exactly one parameter, no return type.
	String getXXX()				... getter, has no parameters, return type String (or rather exactly the same type as the setter parameter type), 
									in case of a Boolean attribute the getter may be called isXXX().


## Package visibility (results automatically from not using a visibility modifier like public)
If the modifiers public or private are not set, then classes are only visible within their own package. Classes from other packages cannot access classes that are not labled public.
If a class A within a package is defined as public, but an attribute or a method x of this class A is not defined public, then classes from outside the package can see the class A, but they cannot see attribute or method x.



## static variables and methods
If attributes or methods cannot be assigned to an individual object, static methods are used. e.g. to calculate the sine of an angle, no specific math object is required. 

An object variable is created, when an object of the class is created by using the operator new and is destroyed, once there are no more references to the object it belongs to.
A static variable on the other hand is created, when the class it belongs is loaded by the JVM and also ends, when the JVM unloads the corresponding class.

static attributes can be accessed by using object references, but this is bad practice...
``` java
	public class GameUtils {
		public static final int MAX_ID_LEN = 20 /* chars */;

		public static boolean isGameIdentifier( String name ) {
			if ( name == null )
				return false;
			return name.length() <= MAX_ID_LEN && name.matches( "\\w+" );
		}
	}

	System.out.println( GameUtils.MAX_ID_LEN );		// good practice for static attribute access
	GameUtils ut = new GameUtils();
	System.out.println( ut.MAX_ID_LEN );			// bad practice for static attribute access
```

Naming conventions (class names starting upper case, variable names starting lower case) make it easier to distinguish between static and object methods.

Note: A static method cannot set an object attribute, if it does not get an object handed (this.name = "xy"; will never work, since this refers to itself, and a static method has no self object)
``` java
	class InStaticNoThis {
		String name;

		static void setName() {
			name = "Amanda";
			this.name = "Amanda";
		}
	}
```


## Constants
Some variables e.g. pi should only be defined once and never ever be changed. To ensure, that variables will not change, the modifier "final" is used with static variables.

	public static final float PI = 3.1415927	// constants are written fully upper case


## Enumerations
Enumerations are constant variables that are defined in a list like manner:
``` java
	public enum Weekday {
		MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
	}

	// The compiler actually does the following:
	public class Weekday {
		public static final Weekday MONDAY;
		public static final Weekday TUESDAY;
		/* ... */
	}

	// to access the enum:
	Weekday day = Weekday.SATURDAY;
	// or
	if ( day == Weekday.MONDAY )
		System.out.println( "'I hate Mondays' (Garfield)" );
```

Note: enums are by definition static, using the modifier static with enum is not necessary.


## Creating and destroying objects
As soon as an object is created using the new operator, memory is reserved from the heap. Once an object is no longer referenced, the garbage collector releases the reserved memory.


## Constructors
When an object is created using the new operator, the constructor of the corresponding class is called automatically. If a constructor is manually defined, its possible to specify the starting state of this object.

Constructors always have the same name as its class and they do not have any return type, not even void.
``` java
	class Dungeon {
		Dungeon() { }	// constructor of class Dungeon; constructors w/o parameters are called standard constructors
	}
```

A constructor is called as soon as an object using operator new is used.

If no constructor is specified, the compiler creates a default constructor (empty standard constructor) for the class.

Note: Private constructors only make sense in classes that have static methods and attributes exclusively.


### Parametrized and overloaded constructors
Constructors can be used with parameters and can also be overloaded.
``` java
	public class Player {
		public String name;
		public String item;

		public Player( String name ) {					// constructor with one parameter
			this.name = name;
		}
		public Player( String name, String item ) {		// constructor with two parameters
			this.name = name;
			this.item = item;
		}
	}
	// Depending how many parameters are used when creating the Player object with the new operator, the first or the second constructor will be accessed.
```

Note! if any constructor has been manually defined, the compiler will not add another one. if there are only parametrized constructors and an object is supposed to be
created w/o supplying parameters, a compilation error will be raised.


### Copy constructor
A copy constructor is a method which takes the state of an object, duplicates it exactly and copies all values to a newly created object, resulting in a copy of the original object.
``` java
	public class Player {
		public String name;
		public String item;

		public Player() { }					// empty constructor
		public Player( Player player ) {	// copy constructor
			name = player.name;
			item = player.item;
		}
	}

	// and use it:
	Player patric = new Player();			// using the empty constructor
	patric.name = "Patric Circle";
	patric.item = "Knoten";
	Player patricsClone = new Player( patric );		// using the copy constructor
```


### Calling a constructor from a constructor
If there are multiple possible ways how a constructor can be required with different parameters but leading to the same result, then it does make sense, that only one constructor will be responsible for actually doing any inizialisation, all other constructors provide the required information and call the main constructor. If modifications in the corpus of the constructor have to be done, only one constructor has to be modified.

``` java
	public class Player {
		public String name;
		public String item;
		public Player() {
			this( "", "" );								// calling the main constructor
		}
		public Player( Player player ) {
			this( player.name, player.item );			// calling the main constructor
		}
		public Player( String name, String item ) {		// the main constructor
			this.name = name;
			this.item = item;
		}
	}
```

Note: when calling a constructor from within a constructor "this(" has to be the first statement of the calling constructor.


## how the GC works
The garbage collector automatically removes objects, that are no longer referenced to free up memory. To do this in an efficient way the Oracle JVM discerns two different groups of objects: most objects are used only for a short period of time. The remaining objects are used for a very long time. When objects are created they are periodically checked for references. If they still show references after a certain period of time, these objects are marked and only after longer timespans again checked for referencelessness. 


## Initialization of object variables
object variables which are not static, can be declared at any line of the class. but all declared object variables will be intialized when the constructor is called and an object is created. The compiler actually rewrites the code and enters declaration and initialization of object variables into all available constructors.
If object variables are not explicitly initialized, the compiler will add an initialization for these variables using 0, null, false as values depending on their type.

``` java
e.g.what we write									what the compiler rewrites
	class InitObjectVariable {						class InitObjectVariable {
		int j = 1;										int j;
		InitObjectVariable() { }						InitObjectVariable() {
															j = 1;
														}
		InitObjectVariable( int j ) {					InitObjectVariable( int j ) {
			this.j = j;										this.j = 1;
		}													this.j = j;
														}
		InitObjectVariable( int x, int y ) { }			InitObjectVariable( int x, int y ) {
															j = 1;
														}
	}												}
```


## static blocks as class initiator
static blocks within a class will always be executed before any other methods. Every static block is executed as soon as a class is loaded into the JVM.

``` java
	class StaticBlock {
		static {
			System.out.println( "Hey" );
		}
		public static void main( String[] args ) {
			System.out.println( "Lets go" );
		}
		static {
			System.out.println( "Ho" );
		}
	}								// results in the output "Hey" "Ho" "Lets go"
```

Since static blocks are always executed at the loading time of the class by the JVM, there can be classes w/o any main() method.


## Initialization of class variables
class variables are initialized in the static block at the class loading time. if the code does not contain a specified static block, the compiler rewrites the source code so that a static block is included and the class variables are inside it.

``` java
	what we write							what the compiler does

	public class InitStaticVariable {		public class InitStaticVariable {
		static int staticInt = 2;				static int staticInt;
	}											static {
														staticInt = 2;
												}
											}
```


## Specimen initializer
Later on subclasses will be discussed. subclasses allow the declaration of classes within a class. this can lead to the fact that there are multiple constructors within one class, potentially repeating the same initialisation source code multiple times. to gain the chance to reduce source code redundance, there is the option to define a specimen initializer. this is always called after the new operator has been used and before the corresponding constructor has been accessed.

``` java
	public class JavaInitializers {
		static {
			System.out.println( "Static Initializer");
		}
		{
			System.out.println( "Speciment initializer" );
		}
		JavaInitializers() {
			System.out.println( "Constructor" );
		}
		public static void main( String[] args ) {
			new JavaInitializers();
			new JavaInitializers();
		}
	}		// leads to the output "Stat Init" "Spec init" "Constructor" "Spec init" "Constructor"
```

Note: The code itself does not reduce, since the compiler copies the source code of the speciment initializer back into all the constructors it can find.



# Object oriented relations

## Associations between objects
The exchange of data between objects is called association. there are different types of association depending on the visibility status of the interacting objects
- unidirectional association ... an object knows another object, but not the other way around, data flow in one direction only.
- bidrectional association ... both objects interact with each other, 1:1 relationship
- multidirectional association ... more than two objects interact, 1:n relationship


### Unidirectional 1:1 association
``` java
	// using the independent class files Player.java, Room.java, Playground.java in the same folder:
	public class Player {
		public Room room;
	}

	public class Room { }

	public class Playground {
		public static void main(String[] args) {
			Player buster = new Player();
			Room tower = new Room();
			buster.room = tower;
		}
	}
```


### Bidirectional 1:1 association
``` java
	using the independent class files Player.java, Room.java, Playground.java in the same folder:
	public class Player {
		public Room room;
	}

	public class Room {
		public Player player;
	}

	public class Playground {
		public static void main(String[] args) {
			Player buster = new Player();
			Room tower = new Room();
			buster.room = tower;
			tower.player = buster;			// bidirectional relationship between buster(Player) and tower (Room).
		}
	}
```

Note: this leads to a problem if there should be a consistent 1:1 relationship between Player and Room. If one reference e.g. buster.room is set null, by definition of the 1:1 bidirectional relationship, tower.player has to be set null as well, since they refer to one another.
Furthermore if another room e.g. toilet is created and toilet adds buster as player, we will have two references from rooms to buster, but only one reference from buster to a room, violating our 1:1 relationship.
To deal with consistency issues, access methods like setRoom(), setPlayer() are required.


### Unidirectional 1:n association
In an 1:n association, the 1 side needs to store multiple references. Depending on context, this can either be a fixed or a dynamic number of references. Arrays are well usable for the fixed case, but not well suited for dynamic cases due to efficency reasons.

## Preview: The dynamical datastructure ArrayList
java.util.ArrayList is able to efficiently deal with a dynamic number of elements. this datatype will be explained in detail later. the following methods are interesting already:
- boolean add( E o )	... adds an object o of type E to the list
- int size()			... returns the number of elements contained in the list
- E get( int idx )		... returns element of type E from index position idx

``` java
	public class Player {
		public String name;
		public Player( String name ) {
			this.name = name;
		}
	}

	import java.util.ArrayList;
	public class Room {
		private ArrayList<Player> players = new ArrayList<>();		// declares and initializes an ArrayList which is able to contain objects of class Player
		public void addPlayer( Player player ) {
			players.add( player );									// adds the reference of Player object player to the Room objects ArrayList
		}
		public void listPlayers() {
			for ( Player player : players )
			System.out.println( player.name );
		}
	}
```	



# Inheritance
Objects communicate with each other through associations. These associations are object specific can be translated into "x has y" relationships. e.g. human has leg, apple has seeds, car has steering wheel.
Objects can also be associated by generalisation e.g. apple and pear are fruits, cats and mice are mammals ... apples and pear both share common fruit, cats and mice mammal characteristics, this common characteristics are inherited from the generalised class.


## Inheritance in java
Java uses a hierarchical approach to align different types that share a generalized association. In these kind of relationships the highest class in this hierarchy is called the superclass. New declarated classes can inherit all visible properties (methods and attributes) of the superclass, if the keyword "extends" is used; such classes are called subclasses of the superclass.

``` java
	public class GameObject {	// superclass
		public String name;
	}

	public class Player extends GameObject { }

	public class Room extends GameObject {
		public int size;
	}

	Player theDoc = new Player();
	theDoc.name = "Dr. Schuwago"; // inherited attribute

	Room clinic = new Room();
	clinic.name = "Clinic";		// inherited attribute
	clinic.size = 120000;		// own attribute
```

Note: if a class is declarated without the "extends" keyword, the class automatically extends class Object. All classes inherit the public methods of superclass Object e.g. toString().


## Visibility modifier protected
Using the modifier protected has two main points:
- protected properties will be inherited by subclasses like private
- protected properties cannot be accessed or seen outside of a package.


## Constructors and inheritance
Constructors of a superclass are not inherited by subclasses. But when a new instance of a subclass is created, the default constructor of the superclass will be executed before the constructor of the subclass is to ensure, that all properties of the superclass are available for the subclass. To ensure this, the method super() is automatically added by the compiler at the appropriate parts in the source code.

``` java
	what we write							what the compiler does
	class GameObject { }					class GameObject extends Object {
												GameObject() {	// constructor inserted by compiler
													super();	// standard constructor call of Object inserted by compiler
												}
											}
	class Player extends GameObject { }		class Player extends GameObject {
												Player() {		// constructor inserted by compiler
													super();	// standard constructor call of GameObject inserted by compiler
												}
											}
	class Room extends GameObject {			class Room extends GameObject {
		public int size;						public int size;
		public Room(int size) {					public Room(int size) {
			this.size = size;						super();	// standard constructor call of GameObject inserted by compiler
													this.size = size;
		}										}
	}										}
```

Note: super() always has to be the first entry in the constructor of an object.

Note! if a superclass Sup has only parametrized constructors and a subclass Sub is used to extend Sup without specifying a fitting parametrized super(parameters), an error will occur. As a rule a superclass should either have a default constructor w/o parameters or the subclass has to specify a fitting parametrized super() call.

### super() and this() conflict
Within a constructor the methods super() and this() by definition have to be the first statement, which leads of course to a conflict. To resolve this, initialization of objects variables requiring the this() method should be outsourced to a private method handing over any required parameters. This way super() can be the first statement in the constructor and the private method will be called next.

``` java
	import java.awt.Color;
	import javax.swing.JLabel;

	public class ColoredLabel extends JLabel {
		public ColoredLabel() {
			initialize( Color.BLACK );		// private method call w/o constructor parameters
		}
		public ColoredLabel( String label ) {
			super( label );					// constructor call of the superclass
			initialize( Color.BLACK );		// private method call w/o constructor parameters
		}
		public ColoredLabel( String label, Color color ) {
			super( label );					// constructor call of the superclass
			initialize( color );			// private method call with constructor parameters, would require to use this.setForeground(color) w/o private method
		}
		private void initialize( Color color ) {
			setForeground( color );
		}
	}
```


## Types and hierachies
Subclasses extend superclasses, they inherit the superclasses public properties and possess properties of their own. In some cases it can be useful to convert a superclass object into an object of a specific subclass. since this subclass has more properties than its superclass the conversion cannot happen automatically it has to be explicitly called.

``` java
	GameObject go	= new GameObject(); // in our example GameObject has the attribute name.
	Room theStudy	= go;				// this would lead to an error, since Romm has an additional attribute size, which GameObject does not provide.

	GameObject go	= new GameObject(); // in our example GameObject has the attribute name.
	Room theStudy	= (Room) go;		// this will explicitly convert object GameObject to Room
```


## The substitution principle
Player and Room are both subclasses to the superclass GameObject. If a method expects an object of type GameObject it is possible to hand it over objects of Player and Room as well. The extended information of Player and Room are in this case simply disregarded and only the information specific to GameObject are kept. As an example the method println(Object) accepts everything, since in the end everything has Object as superclass. All other information that are not defined in class Object are disregarded if an object is handed over to println.


## Overloading and overwriting of superclass methods by subclasses
When a subclass inherits public methods from its superclass the situation can occur, that there will be two methods of the same name. If the two methods have different signature (same name but different parameters), the method will be normally overloaded. In case that the signature of the superclass method and the subclass method is exactly the same, then the subclass method will overwrite/replace the superclass method. This replacing process will happen in any case. To make it explicit and tell the compiler, that a method of a superclass is supposed to be replaced, the @Override operator can be used. In this case the compiler will throw an error, if for some reason (typos, no method with the exact same signature etc) the declared method is not able to replace its superclass method.

``` java
	public class Room extends GameObject {
		private int size;
		public void setSize( int size ) {
			if ( size > 0 )
			this.size = size;
		}
		public int getSize() {
			return size;
		}
		@Override public String toString() {					// in all Room objects the Object.toString() method is replaced by the Room.toString() method
			return String.format( "Room[name=%s, size=%d]", getName(), getSize() );	// toString() now returns useful information about the Room object 
		}																			// instead of the generic return string of a simple object
	}
```

Note: methods outside the own package/hierarchical subclass-superclass tree will not be overwritten e.g. java.util.Date does not replace java.sql.Date and vice versa.

To first overwrite a superclass method to deal with a specific case and afterwards access and execute the overwritten superclass method, the super method can be used as well

``` java
	public class Garage extends Room {
		private static final int MAX_GARAGE_SIZE = 40;
		@Override public void setSize( int size ) {			// overwrite the Room superclass method setSize
			if ( size <= MAX_GARAGE_SIZE )
				super.setSize( size );						// everything is good, call the overwritten Room superclass method setSize, now we can use it
		}
	}
```

Note: if a subclass overwrites a superclass method, but requires to use the superclass method at some point, the super method can always be used.
Note: if a superclass is the subclass of a superclass and already has an overwritten method, it is not possible to use e.g. super.super.method to retrieve the uppermost method, only the last overwritten one.


## Final classes and methods
To avoid having subclasses of a specific class the modifier final prohibits exactly this. To avoid that a method is overwritten in a subclass the same holds true. In both cases a compilation error will occur if and extends or an overwrite is tried respectively.


## Dynamic binding

GameObject myGameObj = new Room();

We create a GameObject using the constructor of the GameObject subclass Room. this is valid, the room specific attrbute size will be initialized with 0. if we try to access this attribute using myGameObj.size, this will not work, since a GameObject does not have a size attribute. The GameObject attribute name nonetheless can be accessed, since Room is a subclass of GameObject. If there is an overwritten method in Room and we access this method using the GameObject myGameObj to access it, the overwritten method from Room will be used, since it has been used to create the object.

Object myObj = new Room();

We created an Object myObj using the constructor of the subsubclass Room. We cannot access the attributes name and size, but overwritten methods from GameObject or Room will be used instead of the superclass methods of Object.

``` java
	public class GameObject {
		public String name;
		@Override public String toString() {
			return String.format( "GameObject[name=%s]", name );
		}
	}

	public class Room extends GameObject {
		public int size;
		@Override public String toString() {
			return String.format( "Room[name=%s, size=%d]", name, size );
		}
	}

	Room rr = new Room();
	rr.name = "Affenhausen";
	rr.size = 7349944;
	System.out.println( rr.toString() );	// ... Room[name=Affenhausen, size=7349944]. uses the normal overwritten toString method from its own class
	GameObject rg = new Room();
	rg.name = "Affenhausen";
	System.out.println( rg.toString() );	// ... Room[name=Affenhausen, size=0]. uses the toString method from Room, even though its type GameObject
	Object ro = new Room();
	System.out.println( ro.toString() );	// ... Room[name=null, size=0]. uses the toString method from Room, even though its type Object
```

The point here is, that the compiler does not know, which toString method is going to be used. This will only be clear after the compiler has rewritten the source code according to the above discussed rules. The binding of the toString method is therefore a dynamic binding.

Note: if a method is defined as private in a superclass, the subclass will not be able to overwrite it.


## Abstract classes and method
If a class is only supposed to provide superclass properties, but is not supposed to actually provide objects of itself, the modifier "abstract" can be used.

``` java
	public abstract class GameObject {		// we will never need an actual object of this type, we only need it
		public String name;					// to provide the name attribute for all of its subclasses.
	}
```

Abstract methods simply provide the signature (name and parameters), but do not specify anything else. Subclasses will specify these methods further.
Objects sharing the same superclass will interact with one another, the superclass provides an abstract method, that each subclass will implement individually to enable directed interaction between the different subclasses in an orderly fashion. 
As an example we have the abstract superclass GameObject which provides the abstract method with signature boolean useOn(GameObject). This now can be used by the subclasses Chicken, Key, Door to check, if they can be used with one another.
The Key method useOn(GameObject) will return true if it is provided with an instance of type Door and false with type Chicken. In this fashion the status of the provided object can also already be modified e.g. change the status of a door from closed to open.

``` java
	public abstract class GameObject {
		public String name;
		public abstract boolean useOn( GameObject object );
	}

	public class Key extends GameObject {
		int id;
		public @Override boolean useOn( GameObject object ) {
			if ( object instanceof Door )
				if ( id == ((Door) object).id )
					return ((Door) object).isOpen = true;
			return false;
		}
	}

	public class Door extends GameObject {
		int id;
		boolean isOpen;
		@Override public boolean useOn( GameObject object ) {
			return false;
		}
	}
```


## Interfaces
Classes can only immediately inherit properties from direct superclasses, but not from parallel classes. To enable access to properties of parallel classes, Java uses the notion of Interfaces. The declaration of an interface contains only the head of a method - modifier, return value and signature (name and parameters). The actual body of the method will be defined within an actual class.

<<interface>>
    Buyable
-------------
-------------
+ price(): double

UML representation of an interface

``` java
	interface Buyable {		// contained in file Buyable.java
		double price();
	}
```

An interface must not have a constructor, instances of interfaces cannot be created. Everything in an interface is always public, therefore the modifier is not explicitly required.


### The implementation of an interface
If a class wants to use an interface, the keyword "implements" and the name of the interface is required after the classname

``` java
	// used files for code are Buyable.java, Chocolate.java and Magazine.java respectively

	interface Buyable {		// interface
		double price();
	}

	public class Chocolate implements Buyable {		// class implementing interface Buyable
		@Override public double price() {			// provide method price() - forced by interface Buyable
			return 0.69;
		}
	}

	// class using interfaces Buyable and Comparable and being subclass of superclass GameObject
	public class Magazine extends GameObject implements Buyable, Comparable<Magazine> {
		private double price;
		public Magazine( String name, double price ) {
			super( name );
			this.price = price;
		}
		@Override public double price() {							// provide method price() - forced by interface Buyable
			return price;
		}
		@Override public int compareTo( Magazine that ) {			// override the Object method compareTo ... forced by interface Comparable
			return Double.compare( this.price(), that.price() );	// method implements comparison of price
		}
		@Override public String toString() {
			return name + " " + price;
		}
	}
```

Now we use the interface to access different classes that are not associated by super/subclass by using the interface.

``` java
	// in PriceUtils.java, calculateSum()
	static double calculateSum( Buyable price1, Buyable... prices ) {	// we can access the different objects via their common interface Buyable, 
		double result = price1.price();									//because everything Buyable has a retrieve method price().
		for ( Buyable price : prices )
			result += price.price();
		return result;
	}

	// in Playground.java, main()
	Magazine madMag = new Magazine();
	madMag.price = 2.50;
	Buyable schoki = new Chocolate();
	Magazine maxim = new Magazine();
	maxim.price = 3.00;
	System.out.printf( "%.2f", PriceUtils.calculateSum( madMag, maxim, schoki ) ); // 6,19
```

Interfaces force a specific format for a method. This in turn enables other methods to rely if they use this interface, that independent on the object provided, the interface method can be accessed an leads to the desired result

With this objects created with class Magazin can be initialized as the following types:

	Magazine				m1	= new Magazine("Mad Magazine", 2.50);	// all methods of superclasses and interfaces are available 
																		// (name(GameObject), price(Buyable), compareTo(Comparable))
	GameObject				m2	= new Magazine("Mad Magazine", 2.50);	// attribute name is accessible (GameObject) and all Object methods
	Object					m3	= new Magazine("Mad Magazine", 2.50);	// only Object methods are available
	Buyable					m4	= new Magazine("Mad Magazine", 2.50);	// only price(Buyable) is available
	Comparable<Magazine>	m5	= new Magazine("Mad Magazine", 2.50);	// only comparableTo(Comparable) is available


### Subinterfaces
Interfaces can extend other interfaces similar to inheritance of classes

``` java
	interface Disgusting {
		double disgustingValue();
	}
	interface Stinky extends Disgusting {
		double olf();
	}
```


### Constants in interfaces
Interfaces only provide the signature of methods, but they can provide constants. All attributes of an interface are implicitly public static final, after the initialization their content cannot be changed any longer.

``` java
	interface Buyable {
		int MAX_PRICE = 10_000_000;
		double price();
	}
```

Note: it is bad practice to use mutable variables like StringBuilder, Arrays, Lists, etc. in interfaces since their content by definition is supposed to change.


### static methods in interfaces
Since Java 8 its possible to declare static methods in interfaces, which are implicitly public but they explicitly have to have the modifier static. The big difference to class methods is, that they cannot be inherited!

``` java
	interface Buyable {
		int MAX_PRICE = 10_000_000;
		static boolean isValidPrice( double price ) {
			return price >= 0 && price < MAX_PRICE;
		}
		double price();
	}

	Buyable.isValidPrice(123);			// returns true
	Magazine myMag = new Magazine();
	myMag.isValidPrice(123);			// raises a compilation error, classes implementing interfaces to not inherit interface methods 
```

Note: Interfaces can be modified after they have been established. If a method has to be added, the keyword default can be added to a new interface method. In this case the interface method may contain not only the signature but also a whole method. Just fyi, dont use it, its nasty because: an interface extending an interface with a default method can have the same signature default method. it will replace the superinterface method. a class can implement an interface with a default method. if it has a method with the same signature as the interface it will replace the interface method - here the normal inheritance rules apply. but since interfaces were developed to separate inheritance from multiple classes using the same methods this gets nasty ideawise. use it only if really required (read chapter 6.7.11&12)



# Exceptions

Java uses exceptions to deal with occurring errors. If an error occurs, the JVM ends the programm and returns information about where and why. Since a lot of errors can be already anticipated, a lot of classes implement specific description to specify the occurring error. As an example we want to parse an integer with a non integer string, an error will occur. Since this is an error that can be anticipated a fitting exception called "NumberFormatException" has already been prepared and implemented and will be returned by the JVM with informations where exactly in the source code this has happened.

``` java
	public class MissNumberFormatException {
		public static int getVatRate() {
			return Integer.parseInt( "19%" ); // will raise NumberFormatException
		}
		public static void main( String[] args ) {
			System.out.println( getVatRate() );
		}
	}

	/* Output: */
	Exception in thread "main" java.lang.NumberFormatException: For input string: "19%"
		at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
		at java.lang.Integer.parseInt(Integer.java:580)
		at java.lang.Integer.parseInt(Integer.java:615)
		at MissNumberFormatException.getVatRate(MissNumberFormatException.java:3)
		at MissNumberFormatException.main(MissNumberFormatException.java:6)
```

## Stack trace
To return informations about an error, the JVM keeps track which method has called which method in a so called stack trace. A typical stack would i.e. look like this: parseInt/getValRate/main ... main called getValRate which called parseInt.


## try and catch
Since its not very nice, if a programm simply crashes, java implements the try and catch construct. It can be used to catch a specified or unspecified error, deal with the result and continue with the program.
If an error occurs, the JVM immediately creates an exception object and stops the execution of the source code. If there is a try/catch block, the JVM jumps to the first catch clause. If the defined Exception in the first catch clause does not match the occured Exception, the next catch clause is entered and so forth.

``` java
	try { /* code */ }
	catch ( /* specify exception */ ) { /* deal with an exception */ }
	/* continue with code */

in our int 19% example the try catch would look like this:

	String stringToConvert = "19%";
	double vat = 19;
	try {
		vat = Integer.parseInt( stringToConvert );
	}
	catch ( NumberFormatException ourExc ) {
		System.err.printf( "'%s' is not convertable to integer.%n", stringToConvert );
	}
	System.out.printf( "Lets continue with MwSt=%g%n", vat );
```

NumberFormatException is of class exception. Class exception provides the following methods:

	catch ( NumberFormatException ourExc ) {
	String name = ourExc.getClass().getName();		// returns the whole path of the Exception java.lang.NumberFormatException
	String msg = ourExc.getMessage();				// returns For input string: "19%"
	ourExc.printStackTrace();						// prints the stack trace


### Forced exception handling and multiple exceptions

Some classes explicitly require the user to deal with defined exceptions if they are to be used at all. E.g. java.lang.URL requires a try catch block, if a new instance of URL is initialized, as does the use of URL.openStream().

``` java
	URL url = new URL( urlString );							// throws "unreported exception java.net.MalformedURLException"
	Scanner scanner = new Scanner( url.openStream() );		// throws "IOException"

but this works fine:

	try {
		URL url = new URL( urlString );
		Scanner scanner = new Scanner( url.openStream() );
	}
	catch ( MalformedURLException ourExc ){
		System.err.println( "No valid URL!" );
	}
	catch ( IOException ourExc ){
		System.err.println( "Opening of URL failed!" );
	}
```

As can be seen in the example above, there can be more than one catch clauses.


### Method throws exception
A method can be ordered to do nothing in case of an error and hand an exception to the method from which it was called.

``` java
	static void printAllEMailAddresses( String urlString )
		throws MalformedURLException, IOException {			// if an error occurs, the execution of the code is immediately stopped
		/* code */											// and the Exception is handed over to the calling method, in our case main().
	}

	public static void main( String[] args ) {
		try {
			printAllEMailAddresses( "http://www.galileocomputing.de/hilfe/Impressum" );
		}
		catch ( MalformedURLException ourExc ) {
			System.err.println( "No valid URL!" );
		}
		catch ( IOException ourExc ) {
			System.err.println( "Opening of URL failed!" );
		}
	}
```

### Using finally when handling exceptions
If an error occurs and a try catch block is available, then the fitting catch clause will be accessed and the code inside will be executed. Thats nice. But if there is code that is to executed regardless of the type of an exception, this code would have to be duplicated within all of the catch clauses. Since this is disgustingly uncool, there is the option to add a "finally" clause at the end of the try/catch block, which will be accessed and executed regardless which catch clause has been accessed before.

ATTENTION: The "finally" clause will *ALWAYS* be accessed, regardless if an error has occurred *OR NOT*!

``` java
	RandomAccessFile f = null;							// f has to be declared and initialized outside of the try catch block, since its required in the finally clause
	try {
		f = new RandomAccessFile( "duke.gif", "r" );
		f.seek( 6 );
		System.out.printf( "%s x %s Pixel%n", f.read() + f.read() * 256,
		f.read() + f.read() * 256 );
	}
	catch ( FileNotFoundException ourExc ) {
		System.err.println( "File not found" );
	}
	catch ( IOException ourExc ) {
		System.err.println( "General I/O Exception" );
	}
	finally {
		if ( f != null )
			try { f.close(); } catch ( IOException e ) { }		// the try within the try is required, since close() itself could throw an exception.
	}
```

Since I am sick in body and mind we will personalize the script for the moment: if we want to ignore all errors, naughty, naughty, but want to execute code always and regardless if an error has occurred or not, we can use a try{}finally{} block w/o bothering about catch clauses.

``` java
	import java.io.*;
	public class ReadGifSizeWithTryFinally {
		public static void printGifSize( String filename )
		  throws FileNotFoundException, IOException {
			RandomAccessFile f = new RandomAccessFile( filename, "r" );
			try {
				f.seek( 6 );
				System.out.printf( "%s x %s Pixel%n", f.read() + f.read() * 256,
				f.read() + f.read() * 256 );
			}
			finally {
				f.close();		// closing access to the file should always happend, regardless if the code above was successful or not.
			}
		}
		public static void main( String[] args )
		  throws FileNotFoundException, IOException {
			printGifSize( "duke.gif" );
		}
	}
```


## RuntimeExceptions
RuntimeExceptions are errors that occur, when during runtime something happens thats usually a programmers fault: division by zero, non existing array index etc. There are many, many (many many) possible ways to screw stuff up, so there are a lot of Subclasses of RuntimeExceptions:

e.g.	ArithmeticException			ArrayIndexOutOfBoundsException			ClassCastException					
		EmptyStackException			IllegalArgumentException				IllegalMonitorException				
		NullPointerException		UnsupportedOperationException		

Runtime exceptions do not have to be mandatorily caught like e.g. an IOException, but they can be.


## Exceptions are hierarchical classes
An exception is an object, thats a subclass of superclass java.lang.Throwable. Throwable objects provide a wide range of methods and is superclass to the classes Error and Exception as independent subclasses.
The class Exception itself is superclass to many sub- and sub-subclasses. Since this is the case, its possible (and convenient) to use a superclass when trying to catch an exception. E.g. FileNotFoundException is subclass to IOException which is subclass to Exception. We can therefore simply state catch(IOException ourExc) and we will be able to catch a FileNotFoundException in the process. Furthermore we can use e.g. "ourExc instanceof FileNotFoundException" to identify the type of IOException in the process and handle it, if we want a specific handling. which is nice.

``` java
	try { /* code */ }
	catch ( IOException e ) {/* catches IOException and all subclasses */}
```

Note: knowing this, there is an issue, if we want to handle a specific IOException subclass (e.g. FileNotFoundException). In this case this catch clause of course has to be BEFORE the catch clause that handles IOExceptions, otherwise the FileNotFoundException will already have been handled in the IOException clause. (duh)


## Multi-catch
If we want to deal with two unrelated subclasses of Exceptions in the same way, but we dont want to duplicate the source code for every catch clause, we can use the multi-catch clause. The catch clause contains all the different Exceptions we want to handle, separated by "|" and viola! no more duplicode!

``` java
	try { /* code */ }
	catch (IOException | SQLException ourExc) { /* code */ }
```

Note: the compiler will anyway rewrite everything to individual catch clauses containing the same source code before turning it to byte code, but for us puny humans its more comfortable to read. yay and such.


## Errors
Errors that are of type java.lang.Error are created when something happens with the JVM. It is dubious to catch Errors, but sometimes it does make sense e.g. in case of an OutOfMemoryError.


## Manually raise exceptions
Existing Exceptions can be raised manually using the keyword "throw".

``` java
	Player( int age ) {
		if ( age <= 0 )
			throw new IllegalArgumentException( "Age <= 0 is invalid!" );

		this.age = age;
	}
```


## Examples of "throwable" exceptions
- IllegalArgumentException			... handed over parameter does not fit requirements
- IllegalStateException				... if the state of an object does not fit requirements
- UnsupportedOperationException		... if an interface or abstract class method is not supported by a class
- IndexOutOfBoundsException			... if the range of an array is violated, the JVM raises this error. Can be used if an index is used incorrectly


Testing parameters for consistency and throwing IllegalArgumentExceptions on a controlled basis:
org.apache.commons.lang.Validate		http://commons.apache.org/lang/
com.google.common.base.Preconditions	http://code.google.com/p/guava-libraries/

Testing for null:
Objects.requireNonNull(reference); // throws an IllegalArgumentException if the reference is null


## Declaring new Exception classes
New Exceptions should be subclasses of superclass Exception and not of superclass Throwable.

``` java
	package com.tutego.insel.exception.v2;

	public class PlayerException extends RuntimeException {		// RuntimeException is subclass to superclass Exception
		public PlayerException() { }							// Constructor for empty Exception call
		public PlayerException( String s ) {					// Constructor for Exception call with Message
			super( s );											// calls Constructor of superclass RuntimeException
		}
	}
```

Note: its always good to extend existing subclasses of Exception to distinguish our special Exception from other Exceptions.

RuntimeExceptions are not subject to try-catch blocks, the JVM simply crashes the program in case of a RuntimeException. This can be exploited when writing own Exceptions: if we dont care if the program crashes or not, we can simply extend RuntimeExceptions and do not have to worry about try catch blocks.


## Rethrowing exceptions

Exceptions can be caught in a catch clause, execute some neat code and raise the same exceptions again, to let the calling method deal with it again and execute some more neat code.

``` java
	import java.net.*;
		public class Rethrow {
		public static URI createUriFromHost( String host ) throws URISyntaxException {
			try {
				return new URI( "http://" + host );
			}
			catch ( URISyntaxException e ) {
				System.err.println( "Hilfe! " + e.getMessage() );
				throw e;
			}
		}
		public static void main( String[] args ) {
			try {
				createUriFromHost( "tutego.de" );
				createUriFromHost( "%" );
			}
				catch ( URISyntaxException e ) {
				e.printStackTrace();
			}
		}
	}
```


## Modifying the stack-trace

If the uppermost stacktrace information is not really interesting, we can rebuild the stacktrace at any method we want using exception.fillInStackTrace(). this method deletes the current stacktrace and refills it until the method the call resides in.

``` java
	public static URI createUriFromHost( String host ) throws URISyntaxException {
		try {
			return new URI( "http://" + host );
		}
		catch ( URISyntaxException e ) {
			System.err.println( "Help! " + e.getMessage() );
			e.fillInStackTrace();
			throw e;
		}
	}
```


## Resource management
Besides memory resources, which are managed by the garbage collector, there are even more resources for us to squander:
- files system resources
- Network resources e.g. socket connections
- database connections
- native resources of the graphics system
- synchronization objects

All of these resources have to be managed and be released after the days work is done. If objects are used, that deal with resources AND are of interface type AutoClosable, we can use a specific try-syntax - try( autoCloseableResource ) {/*code*/} - that automatically closes these resources at the end of the try block w/o us having to do anything. lazyness ftw!

``` java
	InputStream in = ClassLoader.getSystemResourceAsStream( "EastOfJava.txt" );
	try ( Scanner res = new Scanner( in ) ) {
		System.out.println( res.nextLine() );
	}
```

In the example above, Scanner is of interface type AutoClosable, we can use out magic try( autoClosResource ) {/*code*/}, the compiler handles the "finally" clause and enters the close() stuff on its own.

REMEMBER: the type of the used resource has to be AutoClosable, we can check using e.g. res instanceof AutoClosable.


### Using multiple resources

To use AutoClose with multiple resources, please check out the following awesome syntax n stuff:

``` java
	try ( InputStream in = Files.newInputStream( srcPath );
			OutputStream out = Files.newOutputStream( destPath ) ) {
		/* code */
	}
```

This also applies, if more than one resources of the same type are used:

``` java
	try ( InputStream in1 = Files.newInputStream( srcPath1 );
			InputStream in2 = Files.newInputStream( srcPath2 ) ) {
		/* code */
	}
```

	
### Suppressed exceptions
If the very unfortunate event happens, that an exception occurs in the try block and a second one while trying to close(), the exception of the try block will be the main one, the exception that happens at close() will be the second message.

``` java
	NotCloseable.java
	public class NotCloseable implements AutoCloseable {
		@Override public void close() {
			throw new UnsupportedOperationException( "close() is on vacation" ); // Exception
		}
	}

	SuppressedClosed.java
	public class SuppressedClosed {
		public static void main( String[] args ) {
			try ( NotCloseable res = new NotCloseable() ) {
				throw new NullPointerException();	// Exception
			}
		}
	}

leads to the output:

	Exception in thread "main" java.lang.NullPointerException
	at SuppressedClosed.main(SuppressedClosed.java:4)

	Suppressed: java.lang.UnsupportedOperationException: close() is on vacation
	at NotCloseable.close(NotCloseable.java:3)
	at SuppressedClosed.main(SuppressedClosed.java:5)
```


## Assertions

Assertions can be used to dictate, that certain circumstances have to be met during runtime. if these circumstances are not met, exceptions will be raised!

assertions can be used thusly:

	assert AssertConditionExpression;						// AssertConditionExpression ... definition of the conditions that have to be met lest there be trouble!
	assert AssertConditionExpression : MessageExpression;	// MessageExpression ... message for the poor users

``` java
	public class AssertKeyword {
		public static double subAndSqrt( double a, double b ) {
			double result = Math.sqrt( a – b );
			assert ! Double.isNaN( result ) : "Calculation result is NaN!";		// if the result of the calculation does not yield a proper result, go exception!
			return result;
		}

			public static void main( String[] args ) {
			System.out.println( "Sqrt(10-2)=" + subAndSqrt(10, 2) );
			System.out.println( "Sqrt(2-10)=" + subAndSqrt(2, 10) );
		}
	}
```

Note: by default assertions are deactivated to increase speed. java has to be started thusly: "$ java –ea" to enjoy the wonders of assertions.
Note2: assertions can also be activated for packages or even classes only: "$ java -ea:com.tutego.App" to activate assertions in class App, "$ java -ea:com.tutego..." to activate assertions for the tutego class including all subclasses.



# Outer and inner classes
## nested classes and interfaces

Classes and interfaces can be defined within other classes. Depending on the declaration we can distinguish four different types of inner classes:
- static inner class		class Out { static class In { /* code */ } }
- member class				class Out { class In { /* code */ } }
- local class				class Out { Out() { class In { /* code */ } } }
- anonymous inner class		class Out { Out() { new Runable() { public void run() { /* code */ } }; } }


## Static inner classes

Inner classes only have access to static variables of the outer class. The outer class can access the inner class like it was a static method.

``` java
	public class Lamp {
		static String s = "Huhu";
		int i = 1;
		static class Bulb {
			void output()
			{
				System.out.println( s );	//
				System.out.println( i );	// Compiler error: i is not static
			}
		}

		public static void main( String[] args ) {
			Bulb bulb = new Lamp.Bulb();	// or Lamp.Bulb bulb = ...
			bulb.output();
		}
	}
```

Note: after compilation an inner class counts as an individual class residing in the same package as the outer class. Therefore the inner class does not have access to object variables of the outer class. Other classes can access the inner class using "OuterClass.InnerClass"

Note: An outer class my be public and package secure, the inner class can use any of the available visibility modifier. If an inner class is private, only the Outer class itself has access to it, other top level classes don't see the inner class.


## Member classes
A member class is also comparable to an attribute of the outer class, but in comparison to a static inner class, a member class has full access to all outer class variables, even the private ones. BUT: a member class is not allowed to declare static methods itself.

``` java
	class House {								// outer class
		private String owner = "Its me!";
		class Room {							// inner member class
			void ok() {							
				System.out.println( owner );	// inner member class has access to outer class private attributes
			}									// Compiler error: static void error() { }
		}
	}
```

To initialize an instance of the inner class, an instance of the outer class has to exist. to ensure, that there is an instance of the outer class, an initialization of an instance of the inner class has to modify the new operator thusly: referenceOuterClass.new innerClassObj

``` java
	House h = new House();
	Room r = h.new Room();

or
	Room r = new House().new Room();
```

If a member class wants to access the this reference of the outer class, the syntax Out.this is used. If an attribute of the inner class hides an attribute of the outer class, the syntax Out.this.attribute is used.

``` java
	class FurnishedHouse {
		String s = "House";
		class Room {
			String s = "Room";
			class Chair {
				String s = "Chair";
				void output() {
					System.out.println(s );						// Chair, inner attribute
					System.out.println(this.s );				// Chair, inner attribute
					System.out.println(Chair.this.s );			// Chair, inner attribute
					System.out.println(Room.this.s );			// Room, outer attribute of class Room
					System.out.println(FurnishedHouse.this.s );	// House, outer attribute of class House
				}
			}
		}
		public static void main( String[] args ) {
			new FurnishedHouse().new Room().new Chair().output();
		}
	}
```

Note: the compiler creates the files House.class and House$Room.class. When accessing the outer class from the inner class the compiler rewrites the code like blub:
``` java
	class House$Room {
		final House this$0;				// this$0 is a reference to House.this.
		House$Room( House house ) {
			this$0 = house;
		}	// ...
	}
```

Note: the compiler rewrites inner classes as new classes within the same package. Now the problem is, that normally, when a class has private properties, only this class is able to access these properties. an inner class is by the compiler now a new class within the same package as an outer class with a private propertie. to provide the inner class access to the private property of the outer class, the compiler includes a static access method to this property within the outer class, marking this method with $ to indicate usability only by inner classes.
``` java
	class House {
		private String owner;
		static String access$0( House house ) {		// method added by compiler;
			return house.owner;						// if we had more than one outer class attribute, access$1, access$2, access$3... would be added additionally
		}
	}
```

IMPORTANT: this leads to access problems. If someone puts a class within a package containing inner class files, then the additional class may access the access$x methods, because they are visible within the package. to put a stop to this, java archives should be "sealed" to avoid any tampering with the package.


## Local classes (NO local interfaces)
Local classes are declared within constructors, initialization blocks and methods of an outer class. Local classes may only access outer class attributes and local methods variables that are declared final.

``` java
	public class FunInside {
		public static void main( String[] args ) {
			int i = 2;
			final int j = 3;
			class In {
				In() {
					System.out.println( j );
					System.out.println( i );	// Compiler error: i is not final
				}
			}
			new In();
		}
	}
```

Note: There are no local interfaces! Local classes cannot use visibility modifiers.

``` java
	public class SportReminder {
		public static void main( String[] args ) {

			class SportReminderTask extends TimerTask {
				@Override public void run() {
					System.out.println( "Los, beweg dich du faule Wurst!" );
				}
			}

			new Timer().scheduleAtFixedRate( new SportReminderTask(),			// using the local class to repeately send a custom message.
				0 /* ms delay */,
				1000 /* ms period */ );
			/* other code */
		}
	}
```


## Anonymous inner classes/interfaces
Anonymous classes combine declaration and object creation in one step.
	new anonymousName() { /* anonymous class code */ }

- if the anonymous thing is a class, then the created object is a subclass to anonymousName. We can e.g. declare additional attributes to the constructor of the base class.
- if the thing is an interface, it inherits from class Object and implements interface "anonymousName"; but it has to implement methods, otherwise it would be quite useless.

``` java
	new Timer().scheduleAtFixedRate( new TimerTask() {
					@Override public void run() {
						System.out.println( "aaaaaaAAND - GO!" );
					}
				},
				0 /* ms delay */,
				1000 /* ms period */);
```

Anonymous inner classes are like fire and forget things, even more than local classes.


## this in subclasses
``` java
	public class Shoe {
		void out() { System.out.println( "I'm just a shoe, lark, oh deary me!" ); }
		class LeatherBoot extends Shoe {
			void what() { Shoe.this.out(); }
			@Override
			void out() { System.out.println( "I'm a fancypancy Shoe.LeatherBoot." ); }
		}
		public static void main( String[] args ) {
			new Shoe().new LeatherBoot().what();		// I'm just a shoe, lark, oh deary me! ... accessing the outer class out() function through this
			new Shoe().new LeatherBoot().out();			// I'm a fancypancy Shoe.LeatherBoot ... accessing the inner class out()
		}
	}
```


# Special types in Java SE

We often use types of the Java standard library even if we dont realize it. e.g.:
- if a superclass does not extend a class, its automatically inheriting from java.lang.Object
- If we concatenate strings using the + operator, we actually use type java.lang.StringBuilder
- If we use try with resources, the compiler expects type AutoClosable and uses the close() method.
Lets have a close look at some of these behind the scenes types.


## Comparison of objects for ordering
If we want to order objects, we need to compare their contents with one another, so we can decide how we can order them. Java provides two different interfaces to implement such comparisons.

- Comparable ... implements a class Comparable, then objects of this class can compare themselves to other objects. Comparable should be used where the class implementing it already offers a natural order e.g. strings (alphabetical), Number types, Date, File, etc.

- Comparator ... a class implementing Comparator receives two objects and compares them according to a specific order criterium. therefore there can be many different Comparators realizing many different order criteria.


### Interface Comparable
compares itself with another object

	interface java.lang.Comparable<T>
	- int compareTo(T o)

if a class implements Comparable, the following return states have to be provided:
- return value of o1.compareTo( o2 ) < 0		... o1 is "smaller than" o2.
- return value of o1.compareTo( o2 ) == 0		... o1 "equals" o2.
- return value of o1.compareTo( o2 ) > 0		... o1 is "larger than" o2.


### Interface Comparator
compares two arguments in terms of their order. This one requires a class that specifically implements the comparator functionality.

	interface java.util.Comparator<T>
	- int compare(T o1, T o2)

If a class implements Comparator, the following return states have to be provided, in our case the class implementing the interface Comparator is called compCeObj:

return value of compCeObj.compare( o1, o2 ) < 0		... o1 is "smaller than" o2.
return value of compCeObj.compare( o1, o2 ) == 0	... o1 "equals" o2.
return value of compCeObj.compare( o1, o2 ) > 0		... o1 is "larger than" o2.

As an example for a Comparator we will write a class, that sorts Rooms according to their size - we use Comparator since there is no natural order to Rooms. Two rooms will be compared for the larger content of their attribute squaremeters sqm, the rooms shall be sorted from small to large.

``` java
	/* Room.java */
	public class Room {
		private int sqm;
		public Room( int sqm ) {
			this.sqm = sqm;
		}
		public int getSqm() {
			return sqm;
		}
	}

	/* RoomComparatorDemo.java */
	class RoomComparator implements Comparator<Room> {
		@Override public int compare( Room room1, Room room2 ) {
			return Integer.compareTo( room1.getSqm(), room2.getSqm() );		// compareTo from Integer fits our needs, but we could implement anything 
		}																	// that satisfies the <0, == 0, > 0 return rules

		public static void main( String[] args) {
			List<Room> list = Arrays.asList(new Room(1123), new Room(100), new Room(123));
			Collections.sort( list, new RoomComparator() );
			System.out.println( list.get(0).getSqm() );		// 100
		}
	}
```


## Wrapper classes and autoboxing
Wrapper classes help us with the follwing stuff:
- static methods to convert primitive datatypes to String (formatting) and from String to primitive datatype (parsing)
- java is only able to store references in arrays and lists. the values of primitive datatypes have to be wrapped in objects to enable use with arrays/lists
- generics also require wrapper types to work with primitive data types

There is a wrapper class for every primitive datatype. e.g. 	Byte (wrapper) / byte (primDT), Short (wrapper) / short (primDT), etc
Note: void is no datatype, but a class Void declares the constant Class<Void> TYPE.


### Initializing wrapper objects

There are three ways to initialize wrapper objects:
- static valueOf() Methods		e.g.	Integer int1 = Integer.valueOf( "30" );
- boxing						e.g.	Integer int2 = 42;						// boxing actually uses the valueOf method, difference only in source, not in byte code...
- wrapper class constructors	e.g.	Integer int3 = new Integer( 23 );

Wrapper objects are final and therefore immutable. To change the content of a wrapper object, a new one has to be created:

``` java
	int i = 12;										// primitive datatype
	Integer io = Integer.valueOf( i );				// wrapper object
	io = Integer.valueOf( io.intValue() + 1 );		// creating a new wrapper object with changed content
	i = io.intValue();
```


### toString conversion using wrapper classes
All wrapper classes provide a toString method for the conversion of the primitive datatype to String.


### Numerical wrapper classes extend class Number
Class Number is the superclass of all numerical Wrapper classes.

### Autoboxing

Autoboxing is the procedure of automatically transforming a primitive datatype to its Wrapper and vice versa.

``` java
	int i = 12;
	Integer j = i;		// the compiler actually does ... j = Integer.valueOf(i);
	int k = j;			// the compiler actually does ... k = j.intValue();


## Object is the superclass
All classes have object as superclass. Object provides a lot of useful methods, but some of them should be overwritten using @override in the subclasses.

### equals()

Equals checks, if the contents of two objects are the same. Object provides this method, but to actually check, if the contents of a subclass are identical, the method has to be overwritten.

``` java
	@Override
	public boolean equals( Object o ) {
		if ( o == null )					// if the handed over object is null, the objects cannot be equal
			return false;
		if ( o == this )					// if the handed over object has the same reference as our object, the contents are identical by definition
			return true;
		if ( ! o.getClass().equals(getClass()) )	// check if the handed over object is actually of our class
			return false;
		Club that = (Club) o;				// now we cast our class to the handed over object, since the parameter receives it initially as type Object ... has to be, 
											// since we have to overwrite the original method from class Object!
			return this.numberOfPersons == that.numberOfPersons && this.sm == that.sm;	// now compare the contents of all the attributes of our and 
																						// the handed over object and return if they are the same
	}
```

Note: Arrays cannot be compared using the normal equals() method from class object since they dont contain primitive datatypes but references. To compare arrays, the class Array has its own method Array.equals(Object[] a, Object[] b) which should be used. If the array is actually a multidimensional array, then Array.deepEquals() should be used, which also compares the nested arrays.


### clone()
Object has the method clone() which is supposed to create a new object with the same contents as the original one. In Object itself this method is private, other classes are not able to see it, but it can be overwritten. To do this, we implement the interface Clonable to ensure we get our methods right.

``` java
	package com.tutego.insel.object.clone;

	public class Player implements Cloneable {
		public String name;
		public int age;

		@Override
		public Player clone() {
			try {
				return (Player) super.clone();			// method clone() of Object creates a duplicate of the Object and also duplicates the values of the attributes
			}
			catch ( CloneNotSupportedException e ) {	// Kann eigentlich nicht passieren, da Cloneable
				throw new InternalError();
			}
		}
	}

	Player susi = new Player();
	susi.age = 29;
	susi.name = "Susi";
	Player dolly = susi.clone();
	System.out.println( dolly.name + " is " + dolly.age ); // Susi is 29
```

Note: if arrays in an object contain nonprimitive values, then only the references to these values will be cloned, the cloned object will reference to the same values as the original one.

Note: to suppress the clone functionality overwrite clone() with content "throw new RuntimeException( new CloneNotSupportedException() );"


## Iterator, Iterable
To get to the contents of arrays, arraylists and hashlists in an orderly manner, the two interfaces Iterator (enables item by item iteration) and Iterable (provides Iterator) were called to life.


### Interface Iterator

A class implementing Iterator has to provide a method that tells if there are elements left and a method, which retrieves the next element in a list; these methods are hasNext() and next(). An iterator is a forward only way to run through a list.

Interface Iterator provides since Java 8 the option to execute an action on each element of a list while cycling through it:

``` java
	default void forEachRemaining( Consumer<? super E> action ) {
		Objects.requireNonNull( action );
		while ( hasNext() )
			action.accept( next() );
	}
```

Its also possible to delete elements using the remove method of the Iterator. remove() removes the last element retrieved by next().


### Interface Iterable

Iterable is interesting when using types that are supposed to be accessed by the enhanced for loop. An enhanced for can access all Iterable types.

``` java
	for ( String s : Arrays.asList( "Eclipse", "NetBeans", "IntelliJ" ) )
		System.out.printf( "%s ist toll.%n", s );
```

In java 8 interface Iterable provides an internal Iteration. using the forEach method of the interface, the method receives an iterable type and an action, thats supposed to be executed on each of the elements.

``` java
	Arrays.asList( 2, 3, 5, 7, 11 ).forEach( e -> System.out.printf( "Primzahl: %d%n", e ) );
```


## Superclass Enum

As discussed above, enums are lists of constants

``` java
	public enum Weekday {
		MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
	}
	class Weekday extends Enum {
		public static final Weekday MONDAY = new Weekday( "MONDAY", 0 );
		public static final Weekday TUESDAY = new Weekday( "TUESDAY ", 1 );
		/* more constants */

		private Weekday( String s, int i ) {
			super( s, i );
		}
		/* more methods */
	}
```

### Enum method values()
values() enables to call all the constants via a for loop:
``` java
	for ( Weekday day : Weekday.values() )			// or Weekday.class.getEnumConstants()
		System.out.println( "Name=" + day.name() );
```

### Enum method ordinal()
ordinal() receives the ordinal number (ID) of enum elements and enables comparisons to discern their order:
``` java
	static int getOrdinal( String name ) {
		try {
			return Weekday.valueOf( name ).ordinal();
		}
		catch ( IllegalArgumentException e ) {
			return –1;
		}
	}
	// would result in getOrdinal("MONDAY") == 0 and getOrdinal("WEEKDAY") == –1

	// use ordinal to distinguish order:
	System.out.println( Weekday.MONDAY.compareTo( Weekday.FRIDAY ) );	// –4 ... monday is ordered before friday
	System.out.println( Weekday.MONDAY.compareTo( Weekday.MONDAY ) );	// 0
	System.out.println( Weekday.FRIDAY.compareTo( Weekday.MONDAY) );	// 4
```



# Generics<T>
## Introduction
### Compiler and runtime exceptions
The Java compiler checks all types for its methods and attributes, it defines at compilation time how the types look like and if objects and types match up. But the compiler itself is only a first instance to check for object - type relationships, the JVM will check again during runtime and lead to exceptions, if something does not fit.

``` java
	Object o = "String";		
	String s = (String) o;		// cast type String onto type Object. in this case type Object contains a string, all is well and peachy
								// the compiler is happy, since the typecasting is valid, the JVM is happy since everything fits.

	Object o = Integer.valueOf( 42 );
	String s = (String) o;		// the compiler accepts the type casting, but the JVM does not and raises a ClassCastException
```
Generics are there to give the COMPILER more information about the types its supposed to work with, to shorten syntax and make the code more robust against ClassCastExceptions.

### An example
Lets say we have a player which in turn has a pocket. this pocket may contain any objects imaginable, but we cant see which objects they are. due to this, the most common way to describe them is to use type Object, since they can be used to represent ANY object.
In our example a player michael has two pockets, will store numbers in them and we want to know, which pocket contains the larger one.

``` java
	// Pocket.java	
	public class Pocket {
		private Object value;
		public Pocket() {}
		public Pocket( Object value ) { this.value = value; }
		public void set( Object value ) { this.value = value; }
		public Object get() { return value; }
		public boolean isEmpty() { return value == null; }
		public void emptyPocket() { value = null; }
	}

	// Player.java
	public class Player {
		public String name;
		public Pocket rightPocket;
		public Pocket leftPocket;
	}

	// PlayerPocketDemo.java, main()
	Player mp = new Player();
	mp.name = "Michi Peters";
	Pocket pocket = new Pocket();
	Long aBigNumber = 11111111111111L;
	pocket.set( aBigNumber );						// (1) it would be nice to tell here, that a pocket my only contain Long
	mp.leftPocket = pocket;
	mp.rightPocket = new Pocket( 2222222222222222222L );
	System.out.println( "In his pockets "+ mp.name + " hides " + mp.leftPocket.get() + " and " + mp.rightPocket.get() );
	Long val1 = (Long) mp.leftPocket.get();	// (2) it would be nice to tell the compiler, that pocket only contains Long
	Long val2 = (Long) mp.rightPocket.get();
	System.out.println( val1.compareTo( val2 ) > 0 ? "Links" : "Rechts" );
```

This gets a bit convoluted: we want pockets to be able to store everything, so defining what the class can store as Object is A-OK. But later on, when we actually want to use the pocket in our source code 1 we only want to store Objects of type Long, in another case Objects of type Integer! To ensure, that we dont mix anything up and ensure, that we dont make any mistakes. We could do that to declare a new Pocket class for every datatype which should be contained inside e.g. PocketInteger, PocketString, etc, but thats tiresome. we dont want to write redundant code but we also want safety while compiling! hence generics (which in C++ are called templates).


### Declaring generic datatypes
Instead of using Object as the type of choice in the generic class, we now use a placeholder for any type, <T>. The classname also has to contain this information

``` java
	// Pocket.java	
	public class Pocket<T> {
		private T value;
		public Pocket() {}
		public Pocket( T value ) { this.value = value; }
		public void set( T value ) { this.value = value; }
		public T get() { return value; }
		public boolean isEmpty() { return value == null; }
		public void emptyPocket() { value = null; }
	}

	// PlayerPocketDemo.java, main()
	/* same Player mp initialization as above*/
	Pocket<Integer> ourIntPocket		= new Pocket<Integer>();	// this instance may only contain integer objects
	Pocket<String>	ourStringPocket		= new Pocket<String>();		// this instance my only contain string objects

	ourIntPocket.set( 1 );
	int x = ourIntPocket.get();		// since we ensured that our pocket will only contain int, we dont need type casting any longer.

```


### Generics naming convention

	generic type				Pocket<T>
	formal type parameter		T
	parameterized type			Pocket<Long>
	actual type parameter		Long
	raw type					Pocket

	T	...		type
	E	...		element
	K	...		key
	V	...		value

Note: these formal type parameter conventions are purely for making code easier to read for other programmers. the placeholder is freely choosable, we could also use <Blub> if we wanted to.


### Nested generics
A <T> can be of any type. So a <T> can actually also be a <Type <T>>

``` java
	// PocketPlayer.java, main()
	Pocket<Pocket<String>> pocketOfPockets = new Pocket<Pocket<String>>();
	pocketOfPockets.set( new Pocket<String>() );
	pocketOfPockets.get().set( "Inner Pocket<String>" );
	System.out.println( pocketOfPockets.get().get() );		// Inner Pocket<String>
```


### The diamond operator
To initialize a nested list of multiple parameterized types the following is required:
	List<Map<Date,String>> listOfMaps = new ArrayList<Map<Date,String>>();

To shorten this, we can use the so called diamond operator <>, which infers from the left hand side which types to include:
	List<Map<Date,String>> listOfMaps = new ArrayList<>();


### Generic interfaces
Interfaces can be generic as well. When they are implemented by a class, these classes can furthermore either define a datatype for the generic elements or keep it generic.
``` java
	// resolves generic interface Comparable to type Integer
	public final class Integer extends Number implements Comparable<Integer> {
		public int compareTo( Integer anotherInteger ) { /* ... */ }
	}

	// keeps extended classes and implemented interfaces generic ... only the user of class HashSet will parametrize element <E>
	public class HashSet<E>
		extends AbstractSet<E>
		implements Set<E>, Cloneable, java.io.Serializable { /* ... */ }
```


### Generic methods
The general syntax for methods using generic types looks like this:
	Modifier <Type variable(s)> returntype Methodname( [parameters] ) throws-clause

``` java
	public class GenericMethods {
		public static <T> T random( T m, T n ) {		// both input type as return type and parameters are now declared generic.
			return Math.random() > 0.5 ? m : n;
		}
		public static void main( String[] args ) {
			String s = random( "Cheese", "Ham" );
			System.out.println( s );
		}
	}
```

In our example m and n are both declared generic and both receive String as common datatype. If the two variables are not of the same type, the compiler looks for the same common ancestor type.

e.g.	call								identified types			common basetypes

		random("Food", 1)					String , Integer			Object , Serializable, Comparable
		random(1L, 1D)						Long, Double				Object, Number, Comparable
		random(new Point(), 				Point, StringBuilder		Object, Serializable, Cloneable
			new StringBuilder())

Only the common base types are valid return types:

e.g.	Objects1 = random( "Food", 1 );
		Serializable s2 = random( "Food", 1 );
		Comparable s3 = random( "Food", 1 );


### Using generic factory method

``` java
	// class Pocket
	public static <T> Pocket<T> newInstance() {
		return new Pocket<T>();
	}

	// PocketPlayer.java, main()
	Pocket<String> p = Pocket.newInstance();						// the type String is inferred by the newInstance method
	Pocket<Map<String,List<Integer>>> p2 = Pocket.newInstance();	// here we already safe a lot of time w/o having to write the whole generic type string again.
```

Note: the compiler is not always able to infer which type should be used

``` java
	boolean hasPocket = true;
	Pocket<String> pocket = hasPocket ? Pocket.newInstance() : null;			// Error: Type mismatch: cannot convert from Pocket<Object> to Pocket<String>

	Pocket<String> pocket = hasPocket ? Pocket.<String>newInstance() : null;	// this works
```


## Type erasure
Java deletes all generic datatypes during the compiling process and replaces them with Object datatypes, which is called type erasure. Type erasure is required since new java code is supposed to be backward compatible to previous JVMs.

``` java
	before type erasure									after type erasure

	// class Pocket										// class Pocket
	public class Pocket<T> {							public class Pocket {
		private T value;									private Object value;
		public void set( T value ) {						public void set( Object value ) {
			this.value = value; }								this.value = value; }

		public T get() { return vale; }						public Object get() { return value; }
	}													}

	// PocketPlayer.java, main()
	Pocket<Integer> p = new Pocket<Integer>( 1 );		Pocket p = new Pocket( 1 );
	p.set( 1 );											p.set( 1 );
	Integer i = p.get();								Integer i = (Integer) p.get();
```


### Problems of type erasure
- this of course leads to the fact, that we cannot use "p instanceof Pocket<String>" since at runtime the <String> part is missing.
- furthermore due to this a generic class cannot extend an Exception, e.g.:
``` java
	class MyException<T> extends Exception {}
		
	try { }
	catch (MyException<Type1> e) { }		// after compiliation these two blocks would be identical
	catch (MyException<Type2> e) { }
```

- generics cannot be used with static attributes or methods, since they are not bound to an instance of a class. Generics only make sense in combination with instances of a class.


## Raw types
Raw types are generic types which are not parametrized, e.g.:

``` java
	// class Pocket
	public class Pocket<T> {
		private T value;
		public void set( T value ) {
			this.value = value; }
		public T get() { return vale; }
	}

	// PocketPlayer.java, main()
	Pocket p = new Pocket();				// use of generic type as raw type
	p.set( "Nothing much" );
	String content = (String) p.get();
```


## Constraining generic types using Bounds
### Constraining using extends
Its common, that a class only uses a few different types. In this case the generic declaration can contain the "extends" keyword followed by all types that are allowed.

``` java
	public class BondageBounds {
		public static <T extends CharSequence> T random( T m, T n ) {		// now the method only accepts types of class CharSequence
			return Math.random() > 0.5 ? m : n;										//  and its subclasses String, Stringbuffer/StringBuilder
		}

		public static void main( String[] args ) {
			String random1 = random( "Shinju", "Karada" );									// works
			CharSequence random2 = random( "Ushiro", new StringBuilder("Takatekote") );		// works
			Point random3 = random( "", new Point() );				// compiler error: "Bound mismatch". Point is not of Type CharSequence, like at all
		}
	}
```

Note: we have to remember, that the compiler uses the last common ancester for all given parameter types. This can lead to compiler errors if not thought through properly

``` java
	public class BondageBounds {
		public static <T extends Comparable<T>> T max( T m, T n ) {
			return m.compareTo( n ) > 0 ? m : n;
		}

		public static void main( String[] args ) {
			System.out.println( max( "Kino", "Lesen" ) );	// Lesen
			System.out.println( max( 12, 100 ) );	// 100
			System.out.println( max( 12L, 100F ) ); // Compiler error »Bound mismatch«; the last common ancestor 
													// of long and float is Number, which does not implement Comparable
		}
	}
```

Note: we can add ONE class and MULTIPLE interfaces using the "extend" keyword and the "&" operator (only one class due to inheritance reasons):

e.g.	T extends Charsequence & Comparable & Serializable


## Generics and exceptions
As we have stated above, generic classes may not extend Exceptions. But the generic type itself my extend class Exception and use the throw clause; later on classes extending the generic class or interface, they can raise specific Exceptions:

``` java
	public interface CharIterable<E extends Exception> {
		boolean hasNext() throws E;
		char	next()	throws E;
	}

	class StringIterable implements CharIterable<RuntimeException>

	class WebIterable implements CharIterable<IOException>
```


## Wildcard types

If we want to use very basic methods with instances of generic objects, where we cannot be sure, what datatype they will contain during runtime, we can use the so called wild card type ?.

``` java
	public static boolean isOnePocketEmpty( Pocket<?>... pockets ) {		// we can hand over mainy different Pockets to the method, the handed over type is not known.
		for ( Pocket<?> pocket : pockets )
			if ( pocket.isEmpty() )											// this method has to be indifferent to the actual Pocket<type> at runtime.
				return true;
		return false;
	}

	public static void main( String[] args ) {
		Pocket<String> p1 = new Pocket<>( "Bad-Bank" );
		Pocket<Integer> p2 = new Pocket<>( 1500000 );
		System.out.println( isOnePocketEmpty( p1, p2 ) );	// false
		System.out.println( isOnePocketEmpty( p1, p2, new Pocket<Byte>() ) ); // true
	}
```


### Bounded wild cards

?					wildcard type					contains any type								restriction
? extends Type		Upper-bounded Wildcard-Typ		has to be of defined type or a subclass			output read only
? super Type		lower-bounded Wildcard-Typ		has to be of defined type or a superclass		write only

e.g.	<? extends CharSequence> has to be of type CharSequence or String, StringBuilder, StringBuffer
		<? super String> has to be of type String or CharSequence or Object

``` java
	interface Portable {
		double getWeight();
		void setWeight( double weight );
	}

	abstract class AbstractPortable implements Portable {
		private double weight;
		@Override public double getWeight() { return weight; }
		@Override public void setWeight( double weight ) { this.weight = weight; }
		@Override public String toString() { return getClass().getName() + "[weight=" + weight + "]"; };
	}

	class Pen extends AbstractPortable { }

	class Cup extends AbstractPortable { }


	class PortableUtils {
		public static boolean areLighterThan( List<? extends Portable> list, double maxWeight ) {
			double accumulatedWeight = 0.0;
			for ( Portable portable : list )
				accumulatedWeight += portable.getWeight();
			return accumulatedWeight < maxWeight;
		}
	}

	public class PortableDemo {
		public static void main( String[] args ) {
			Pen pen = new Pen();
			pen.setWeight( 10 );
			Cup cup = new Cup();
			cup.setWeight( 100 );
			System.out.println( PortableUtils.areLighterThan( Arrays.asList( pen, cup ), 10 ) ); //false
			System.out.println( PortableUtils.areLighterThan( Arrays.asList( pen, cup ), 120 ) ); //true
		}
	}
```

## Type token
To avoid that a generic type will be replaced with Object by the compiler we can use the type token

``` java
	public static <T> T newInstance( Class<T> type ) {
		try {
			return type.newInstance();
		}
		catch ( ReflectiveOperationException e ) {
			throw new RuntimeException( e );
		}
	}
	
	Class<String> clazz1 = String.class;				// with this we create a Class object of runtime type <String>
	String newInstance = clazz1.newInstance();			// with this we get an instance of the class which returns as string since thats what we defined above
	Class< ? extends String> clazz2 = newInstance.getClass();	// with getClass() which is a method of class Object we get the same Class<String> as clazz1
	System.out.println( clazz1.equals( clazz2 ) ); // true
```

[xxx]  I dont get at all what this is for! [xxx] check out REFLECTIONS http://openbook.galileocomputing.de/javainsel9/javainsel_25_001.htm
I think some stuff using generics cannot be compiled. But using the type token the same constructs can be compiled, any runtime errors can be dealt with by the exceptions.


## Super type token
The class object can represent any other type, but it cannot represent generic datatypes:

	type parameter		Class object representing type parameter
	String				Class.String
	Integer				Class.Integer
	Pocket<String>		Class.Pocket<String> ... does not work!	... read the solution if the problem really arises


## Generics and arrays
Arrays cannot easily used with generics. The compiler would rewrite a generic <T> array T[] to an Object[]. If we now would use this method with a <String>, the returning Object[] would be cast to a (String[]) which does not work:

``` java

	class TwoBox<T> {
		T[] array = new T[ 2 ]; // not allowed
		T[] getArray() { return array; }
	}

	// some main()	
	TwoBox<String> twoStrings = new TwoBox<String>();
	String[] stringArray = twoStrings.getArray();

compiler rewrites to:

	class TwoBox {
		Object[] array = new Object[ 2 ];
		Object[] getArray() { return array; }
	}

	// some main()	
	TwoBox<String> twoStrings = new TwoBox<String>();
	String[] stringArray = (String[]) twoStrings.getArray();	// this cast cannot work since an Object[] would be returned
```

we would have to use:

``` java
	class TwoBox<T> {
		T[] array = (T[]) Array.newInstance( clazz, 2 ); // but the Class type of clazz has to be known and handed over as parameter
		T[] getArray() { return array; }
	}

```


## Bridge methods
Java bridge methods are synthetic methods implemented so that older JVMs can use generics. In detail this works that the signature in the byte code also contains the return value. If we have a method, which has been modified in a new version of java, this method exists twice, once in the old and once in the new version and they are clearly distinguishable, but only in the byte code. The compiler simply adds a forward from the old method to the new one, which is the bridge method.


Check out
http://gafter.blogspot.com/2006/12/super-type-tokens.html
http://www.artima.com/weblogs/viewpost.jsp?thread=206350
java.util.Collections



# Lambda expressions

Lambda expressions are very short pieces of software which can be described as small, compact methods without declaration. They follow the general syntax

	"(" lambda parameter(s) ")" "->" "{" code "}"

- lambda expressions cannot contain variable parameters
- lambda parameter names cannot overwrite existing local variable names, a lambda expression behaves similar to code blocks in this respect, they are not a new namespace

```java
	String var = "";
	var.chars().forEach( var -> { System.out.println( var ); } ); // compilation error due to local variable "collision"
```


## Functional interfaces
Interfaces which have exactly one method are called functional interfaces. A lambda expression is always an instance of a functional interface. This does not mean, that every interface with exactly one method should be used as a functional interface for lambda expressions... Due to this every interface thats supposed to be used by lambda expressions should be marked with the following annotation:

```java
	@FunctionalInterface
	public interface MyFunctionalInterface {
		void foo();
	}
```

Depending on how an interface is declared, the actual applied method when using the lambda expression differs from whats written in the interface. To use any lambda expression, the following syntax is required;
	@FunctionalInterface
	public interface MyInterfaceName { R apply(T t); }
	// R ... return type
	// T ... input type
	// t ... input parameter
	// apply() ... apply the actual lambda expression declared in the source code using input t of type T

```java
	// interface
	interface Consumer<A> { void apply( A a ); }

	// actual usage of interface by lambda expression - System.out.printf substitutes interface placeholder apply()
	Consumer<String> printQuoted = s -> System.out.printf( "'%s'", s );
	printQuoted.accept( "Chris" );		// 'Chris'
```


## The type of a lambda expression depends on the context
The compiler infers from the syntax of the lambda expression of which interface type the lambda expression actually is. The following lambda expression is for example of type Comparator<String>

```java
	Comparator<String> c = (String s1, String s2) -> { return s1.trim().compareTo(s2.trim()); }
```
In this example the inference is quite easy for the compiler, "Comparator<String>" tells it about the allowed input (String) and the output (int)

A more complicated inference is required, if the type is not explicitly declared:
```java
	Arrays.sort( words,
			(String s1, String s2) -> { return s1.trim().compareTo(s2.trim()); }
	);
```
The compiler infers in multiple steps of which type a lambda expression is
1) the two input parameters are both Strings and the return type is int and its using the compareTo method
2) the method call of this would be "int compare( String, String )"
3) hence the functional interface has to be Comparator<String>


## Lambda expression syntax
There are different ways to construct a lambda expression
```java
	// extensive syntax
	Comparator<String> c = (String s1, String s2) -> { return s1.trim().compareTo(s2.trim()); };
	// the input parameters are explicitly stated as String.

	// type inference syntax
	Comparator<String> c = (s1, s2) -> { return s1.trim().compareTo(s2.trim()); };
	// since the type is defined as Comparator<String>, the two input parameters can only be String, they are implicitly inferred by the compiler

	// single statement syntax
	Comparator<String> c = (s1, s2) -> s1.trim().compareTo( s2.trim() );
	(a,b) -> a + b;								// instead of ( a, b ) -> { return a + b; }
	() -> System.out.println();					// instead of () -> { System.out.println(); }
	// if the expression of the lambda corpus is only one statement, the return operator and the block parenthesis can be omitted.

	// single parameter syntax
	s -> s.length();						// instead of ( String s ) -> s.length();		or	( s ) -> s.length();
	i -> Math.abs( i );						// instead of ( int i ) -> Math.abs( i );		or 	( i ) -> Math.abs( i );
	// if the type of the input parameter can be inferred and there is only one, the syntax can be significantly reduced.
```


## Lambda access to variables
Lambda expressions have access to the following variables:
- object variables
- class variables
- local variables IF they are EFFECTIVE FINAL (which means they have been initialized and cannot be changed any longer)

```java
	public class CompareIgnoreCase {
		public static void main( String[] args ) {
			final boolean compareIgnoreCase = new Scanner( System.in ).nextBoolean();

			Comparator<String> c = (s1, s2) -> compareIgnoreCase ?
						s1.trim().compareToIgnoreCase( s2.trim() ) :
						s1.trim().compareTo( s2.trim() );

			String[] words = { "M", "\nSkyfall", " Q", "\t\tAdele\t" };

			Arrays.sort( words, c );
			System.out.println( Arrays.toString( words ) );
		}
	}
```


## Lambda and the this reference
The this reference of Lambda expression always points to the instance where the Lambda expression resides in, it does not have its own namespace - which has the same reason, why lambda references are not allowed to use variables with the same name as other local variables.

```java
	class InnerVsLambdaThis {
		InnerVsLambdaThis() {	// constructor
			Runnable lambdaRun = () -> System.out.println( this.getClass().getName() );
			Runnable innerRun = new Runnable() {
				@Override public void run() { System.out.println( this.getClass().getName()); }
			};
			lambdaRun.run();	// print: InnerVsLambdaThis
			innerRun.run();		// print: InnerVsLambdaThis$1
		}

		public static void main( String[] args ) {
			new InnerVsLambdaThis();
		}
	}
```


## Lambda expressions and exceptions
If exceptions during the execution of a lambda expression arise, they have to be dealt with by the code of the lambda expression itself.

```java
	// functional interface implements boolean test
	public interface Predicate<T> { boolean test( T t ); }

	// some main()
	Predicate<Path> isEmptyFile = path -> {				// lambda
		try {											// only correct syntax to catch lambda exceptions
			return Files.size( path ) == 0;
		} catch ( IOException e ) { return false; }
	};	
```


## Method references
Its exactly what it sounds like. A method reference is a reference to a method w/o calling it.

```java
	// we used this lambda expression previously
	Arrays.sort( words, (String s1, String s2) -> StringUtils.compareTrimmed(s1, s2) );

	// exactly the same method has already been implemented in StringUtils and can be used accordingly
	Arrays.sort( words, StringUtils::compareTrimmed );
```

In our example StringUtils::compareTrimmed returns a new Comparator<String> instance, which trims both input strings before returning the comparison of the String values.


## Constructor references
If we want to use existing classes with existing interfaces to gain functionality from both, we can connect them using constructor references.

```java
	Supplier<Date> factory = Date::new;		// functional interface Supplier<T> has exactly one method ( T get() ), which we can combine 
	System.out.print( factory.get() );		// 		with the standard constructor of Date
```

As we can see in the example above we can use existing stuff like a modular system w/o much actual coding. The factory interface is there, we dont have to write a class to implement the details of this interface, we can simply link it to the already implemented constructor of an existing class and we get what we want.


## java.util.function functional interfaces
The package java.util.function already provides a bunch of predefined functional interfaces so we dont have to write everything from scratch.


### Consumer
	interface java.util.function.Consumer<T>
	- void accept(T t)											... executes operations using t
	- default Consumer<T> andThen(Consumer<? super T> after)	...	executes the first Consumer, executes after and returns new consumer

```java
	import java.util.function.*;
	import java.util.logging.Logger;
	class Consumers {
		public static <T> Consumer<T> measuringConsumer( Consumer<T> block ) {
			return t -> {
				long start = System.nanoTime();
				block.accept( t );
				long duration = System.nanoTime() - start;
				Logger.getAnonymousLogger().info( "Execution time (ns): " + duration );
			};
		}
	}

	// some main()
	Consumer<Void> wrap = measuringConsumer( Void -> System.out.println( "Test" ) );
	wrap.accept( null );
```

another example?
```java
	Arrays.asList( 1, 2, 3, 4 ).forEach( System.out::println );
```


### Supplier
	interface java.util.function.Supplier<T>
	- T get()	...		executes operations using t


### Predicate
	interface java.util.function.Predicate<T>
	- boolean test(T t)		...		executes a test with t, returns false or true

```java
	Predicate<Character> isDigit = c -> Character.isDigit( c );		// we are using interface Predicate with type Character. the function we are using for the interface
																	// function test() is Character.isDigit. This has already been defined as sthg that returns true or false
or
	Predicate<Character> isDigit = Character::isDigit;

	System.out.println( isDigit.test('a') );					// false
```

Predicate has a number of default methods that can be used with the results of the isDigit method e.g. default  Predicate<T> negate() which negates the result of isDigit

```java
	Predicate<Character> isDigit = Character::isDigit;
	Predicate<Character> isNotDigit = isDigit.negate();
	List<Character> list = new ArrayList<>( Arrays.asList( 'a', '1' ) );
	list.removeIf( isNotDigit );	// removeif native method of ArrayList ...  	removeIf(Predicate<? super E> filter) ... 
```


### Function
	interface java.util.function.Function<T, R>
	- R apply(T t)		... applies a function with input t and delivers a return value

```java
	Function<Double, Double> abs = a -> Math.abs( a );		// or Math::abs
	System.out.println( abs.apply( -12. ) );				// 12.0
```

function not only works with static methods but also with object methods. If used with object methods, then the method will be used with the very FIRST object that is handed to the functional interface.

```java
	Function<String, String> func2a = (String s) -> s.toUpperCase();		// or String::toUpperCase;
	System.out.println( func2a.apply( "jocelyn" ) );						// JOCELYN

	Function<Point, Double> func1a = (Point p) -> p.getX();					// or Point::getX;
	System.out.println( func1a.apply( new Point( 9, 0 ) ) );				// 9.0
```


### UnaryOperator
	interface java.util.function.UnaryOperator<T> extends Function<T,T>
	- static <T> UnaryOperator<T> identity()

returns an the UnaryOperator which is required by some methods of other objects, i.e.:

```java
	List<Integer> list = Arrays.asList( 1, 2, 3 );
	list.replaceAll( e -> e * 2 );						// replaceAll requires a UnaryOperator to apply the changes to each and every element of the list
	System.out.println( list ); // [2, 4, 6]
```

Note: works only if input and output are of the same type


### BiXXX functional interfaces.
BiXXX enable the java.util.function.* interfaces to handle two instead of one arguments. The arguments can be of different types.

	Consumer<T>			void accept(T t)		...		BiConsumer<T,U>			void accept(T t, U u)
	Function<T,R>		R apply(T t)			...		BiFunction<T,U,R>		R apply(T t, U u)
	Predicate<T>		boolean test(T t)		...		BiPredicate<T,U>		boolean test(T t, U u)

These interfaces are essential for handling key-value pairs when using associative memory classes!

	interface java.util.Map<K,V>
	- default void forEach(BiConsumer<? super K,? super V> action)

```java
	Map<String, Integer> map = new HashMap<>();
	map.put( "Manila", 25 );
	map.put( "Leipzig", -5 );
	map.forEach( (k,v) -> System.out.printf("%s is at %d degrees%n", k, v) );
```


### BinaryFunction
	interface java.util.function.BiFunction<T,U,R> extends Function<T, T>
	- default <V> BiFunction<T,U,V> andThen(Function<? super R,? extends V> after)

Some Methods require a BinaryFunction as input like i.e.:

```java
	Map<Integer, String> map = new HashMap<>();
	map.put( 1, "one" ); map.put( 2, "tWO" );
	System.out.println( map );												// {1=one, 2=tWO}
	BiFunction<Integer, String, String> func = (k, v) -> v.toUpperCase();
	map.replaceAll( func );													// requires BinaryFunction
	System.out.println( map );												// {1=ONE, 2=TWO}
```


### BinaryOperator
	interface BinaryOperator<T> extends BiFunction<T,T,T>

As with the UnaryOperator some methods require a BinaryOperator for their function. And as with the UnaryOperator this works only if both inputs and the output are all of the same type.

```java
	BiFunction<Double, Double, Double>	max1 = Math::max;
	BinaryOperator<Double>				max2 = Math::max;
```


## Class Optional and the null reference

To avid NullPointerExceptions, Java 8 provides a specific container object, Optional, which can be asked if it contains an element or not. Null will not be required to check if an object is there or not.

```java
	Optional<String> opt1 = Optional.of( "Aitazaz Hassan Bangash" );
	System.out.println( opt1.isPresent() ); 						// true
	System.out.println( opt1.get() );								// Aitazaz Hassan Bangash

	Optional<String> opt2 = Optional.empty();
	System.out.println( opt2.isPresent() );							// false, opt2.get() -> java.util.NoSuchElementException: No value present

	Optional<String> opt3 = Optional.ofNullable( "Malala" );
	System.out.println( opt3.isPresent() );							// true
	System.out.println( opt3.get() );								// Malala

	Optional<String> opt4 = Optional.ofNullable( null );
	System.out.println( opt4.isPresent() );							// false, opt4.get() -> java.util.NoSuchElementException: No value present
```

	final class java.lang.Optional<T>
	- static <T> Optional<T> empty()				... returns an empty Optional object
	- boolean isPresent()							... returns true if Optional has value, false if not
	- static <T> Optional<T> of(T value)			... creates a new Optional Object with some value which must not be null! if it is null, exception!
	- static <T> Optional<T> ofNullable(T value)	... creates a new Optional Object if value is there, and an empty Optional Object if the value is null
	- T get()										... returns the value, if the value is Optional empty() there will be a NoSuchElementException
	- T orElse(T other)								... returns the value of Optional or "other" if its null

Optional methods for use with interfaces:
	- void ifPresent(Consumer<? super T> consumer)			... if the Optional has an element call Consumer with its value, otherwise do nothing
	- Optional<T> filter(Predicate<? super T> predicate)	... if the Optional contains an element and the predicate is true, return the Optional, otherwise empty()
	etc.

Optional objects can be nicely combined with lambda expressions to generate null pointer safe elegant code:

```java
	Optional.ofNullable( NetworkInterface.getByIndex( 2 ) )		// could return null, but will create Optional.empty() if there really was null
			.map( NetworkInterface::getDisplayName )
			.map( String::toUpperCase )
			.ifPresent( System.out::println );
```

Since the next expression in line will only be executed if the one before is actually not null, otherwise we get an Optional.empty.map(...), which simply does nothing. At the end of the expression we get an Optional<String> instead of a String

We also can use predicates to test for stuff and wrap the tests in an Optional to be safe in case of null.

```java
	Predicate<Path> exists = path -> Files.exists( path );
	Predicate<Path> directory = path -> Files.isDirectory( path );
	Predicate<Path> existsAndDirectory = exists.and( directory );

	Optional.ifPresent(existsAndDirectory) // does this work?
```



# Design patterns
The problem with object oriented programming is that we drown in a multitude of classes and interfaces which make projects very unreadable very fast. design patterns help to solve the same problems that occur over and over again to reduce the weedy field of classes.

## Singleton
A singleton is a class which will have exactly one instance in the running program e.g.
- a GUI has only one window
- there is only one I/O stream using a console
- there is only one printer queue

We can use ```enum``` with exactly one element to code for a nice singleton

```java
	public enum Configuration {
		INSTANCE;
		private Properties props = new Properties( System.getProperties() );

		public String getVersion() {
			return "1.2";
		}

		public String getUserDir() {
			return props.getProperty( "user.dir" );
		}
	}

	// in some main()
	System.out.println( Configuration.INSTANCE.getVersion() ); // 1.2
	System.out.println( Configuration.INSTANCE.getUserDir() ); // C:\Users\...
```

Or we can code a method with a private constructor - every time we call a method of this class, the object is created, the method returns the value and the object is gone again ... always only one object at any point in time.

```java
	public class Configuration2 {

		private static final Configuration2 INSTANCE = new Configuration2();

		public final static Configuration2 getInstance() {
			return INSTANCE;
		}
		private Configuration2() {
		}

		private Properties props = new Properties( System.getProperties() );

		public String getVersion() {
			return "1.2";
		}
		public String getUserDir() {
			return props.getProperty( "user.dir" );
		}
	}

	//in some main()
	System.out.println( Configuration2.getInstance().getVersion() ); // 1.2
	System.out.println( Configuration2.getInstance().getUserDir() ); // C:\Users\...
```


## Factory methods
Factory methods create an instance of an object not by using the constructor but by using methods. This leads to the advantage, that 
- old objects can be retrieved from cache
- subclasses can handle the creation of instances
- null can be returned even though that's not to be encouraged, NullPointerE and all that noise. (a constructor always returns the created object)

Naming convention: factory methods can be identified by using Classname.getInstance().

```java
	// factory methods can be parametrized and overloaded
	Calendar.getInstance()
	Calendar.getInstance( java.util.Locale )
	Calendar.getInstance( java.util.TimeZone )
```


## Observer pattern
Lets use a metaphor for describing the problem. At a party we have talking participants and listening participants. The listening participants observer the talking participants therefore the first are called the Observers and the later ones the Observables. If Observables are not observed any longer, they go dormant. This metaphor can be applied to data structures.
A data structure can be observed, it can be implemented by an object of class Observable (```java.util.Observable```). If an Observable object is changed anything, it informs all of its listeners via interface Observer about the changes.

### Class Observable and interface Observer
Every object of class Observable has to communicate every change its undergoing to its Observers. This is implemented by the methods ```setChanged()``` and ```notifyObservers()```. ```setChanged()``` internally sets a flag, which is reset, when ```notifyObservers()``` is called. In turn ```notifyObservers()``` is only executed, if the setChanged flag is actually set.

The class also provides the methods ```hasChanged()``` which reveales if the flag has been set, and ```clearChanges()``` to manually reset the flag.

Classes that are interested in an Observable object, have to implement interface Observer and have to "log in" to the Observable object touching method ```addObserver(o: Observer)```


```java
	// class implementing Observable
	package com.tutego.insel.pattern.observer;
	import java.util.*;

	class JokeTeller extends Observable {
		private static final List<String> jokes = Arrays.asList(
			"Sorry, aber du siehst so aus, wie ich mich fühle.",
			"Eine Null kann ein bestehendes Problem verzehnfachen.",
			"Wer zuletzt lacht, hat es nicht eher begriffen.",
			"Wer zuletzt lacht, stirbt wenigstens fröhlich.",
			"Unsere Luft hat einen Vorteil: Man sieht, was man einatmet."
		);

		public void tellJoke() {
			setChanged();						// set "change has happened" flag
			Collections.shuffle( jokes );
			notifyObservers( jokes.get(0) );	// send changes to observers, reset flag
		}
	}

	// class implementing Observer
	package com.tutego.insel.pattern.observer;
	import java.util.*;

	class JokeListener implements Observer {
		private final String name;

		JokeListener( String name ) {
			this.name = name;
		}
		@Override public void update( Observable o, Object arg ) {
			System.out.println( name + " laughs about: \"" + arg + "\"" );
		}
	}

	// meanwhile, in some main()
	Observer achim = new JokeListener( "Achim" );
	Observer michael = new JokeListener( "Michael" );
	JokeTeller chris = new JokeTeller();
	chris.addObserver( achim );
	chris.tellJoke();
	chris.tellJoke();
	chris.addObserver( michael );
	chris.tellJoke();
	chris.deleteObserver( achim );
	chris.tellJoke();
```

Note: here we can only hand over Objects and have to do class casting at the Observer side, if we want to actually use the handed over information. To use generics, we would need to implement ```interface Observer<T>``` and ```public class Observable<T>``` ourselves. Check out ce book p818 for details.


### Listeners
Listeners are similar to Observer/Observable, but require a three component approach. We have a class event objects (1), an interface event listeners (2) and a trigger class which uses specific methods as event trigger(3). The naming convention for class (1) is ```XXXEvent```, interface (2) implementation ```XXXListener``` and (3) methods ```addXXXListener()``` and ```removeXXXListener()```.

```java
	// (1) the event object class
	package com.tutego.insel.pattern.listener;
	import java.util.EventObject;

	public class AdEvent extends EventObject {
		private String slogan;
		public AdEvent( Object source, String slogan ) {
			super( source );
			this.slogan = slogan;
		}
		public String getSlogan() {
			return slogan;
		}
	}

  // (2) the listener interface
	package com.tutego.insel.pattern.listener;
	import java.util.EventListener;

	interface AdListener extends EventListener {
		void advertisement( AdEvent e );
	}

  // (3) the trigger class, adding and removing listeners and triggering events
	package com.tutego.insel.pattern.listener;
	import java.util.*;
	import javax.swing.event.EventListenerList;

	public class Radio {
		private EventListenerList listeners = new EventListenerList();
		private List<String> ads = Arrays.asList( "Jetzt explodiert auch der Haarknoten",
													"Red Fish verleiht Flossen",
													"Bom Chia Wowo",
													"Wunder Whip. Iss milder." );
		public Radio() {
			new Timer().schedule( new TimerTask() {
				@Override public void run() {
					Collections.shuffle( ads );
					notifyAdvertisement( new AdEvent( this, ads.get(0) ) );
				}
			}, 0, 500 );
		}
		public void addAdListener( AdListener listener ) {
			listeners.add( AdListener.class, listener );
		}
		public void removeAdListener( AdListener listener ) {
			listeners.remove( AdListener.class, listener );
		}
		protected synchronized void notifyAdvertisement( AdEvent event ) {
			for ( AdListener l : listeners.getListeners( AdListener.class ) )
				l.advertisement( event );
		}
	}

  // (4) somewhere in the main() wastelands there was a lone listener
	Radio r = new Radio();
	class ComplainingAdListener implements AdListener {
		@Override public void advertisement( AdEvent e ) {
			System.out.println( "Oh nein, schon wieder Werbung: " + e.getSlogan() );
		}
	}
	r.addAdListener( new ComplainingAdListener() );
```


# JavaBeans
Are modular software components which can be accessed and modified from the outside w/o changing the software itself(?). core attributes are:
- introspection ... a class is readable from the outside. a bean can be read by an outside program, the bean itself knows if its being read
- properties ... beans have well defined properties e.g. background color
- customization ... the properties of a bean e.g. background color can be accessed and modified from the outside. access to these properties are only allowed through specific property-design-patterns.
- events ... parts of the bean can trigger events which inform listeners outside of the bean.
- persistence ... the internal state of a bean can be saved and restored at a later moment in time.


## Properties
Java beans can use four different types of properties:
- common properties ... if a bean has a property ```Name``` the bean provides the methods ```getName()``` and ```setName()```
- indexed properties ... when managing similar properties with an array.
- bound properties ... used to inform listeners if a bean changes its state
- constraint properties ... used to ensure data consistency if beans interact with other beans e.g. if one bean wants to change its state but other beans cant allow this.


### Common properties
The only methods allowed for common properties are the following:
```java
	public T getXXX()
	public void setXXX(T value)
	public boolean isXXX()		// in case the property is boolean
```


### Indexed properties
If a property is stored in an array the following methods are implemented to gain access:
```java
	public T[] getXXX()
	public T getXXX( int index )
	public void setXXX( T[] values )
	public void setXXX( T value, int index )
```


### Bound properties
Bound properties are required to implement PropertyChangeEvents and enable PropertyChangeListeners to be informed if properties of the bean are changed. For the implementation java provides the PropertyChangeSupport class. The most important methods of this class are ```addPropertyChangeListener()```, ```removePropertyChangeListener()``` and ```firePropertyChange()```

```java
	package com.tutego.insel.bean.bound;
	import java.beans.PropertyChangeListener;
	import java.beans.PropertyChangeSupport;

	public class Person {
		private String name = "";
		private PropertyChangeSupport changes = new PropertyChangeSupport( this );
		public void setName( String name ) {
			String oldName = this.name;
			this.name = name;
			changes.firePropertyChange( "name", oldName, name );
		}
		public String getName() {
			return name;
		}
		public void addPropertyChangeListener( PropertyChangeListener l ) {
			changes.addPropertyChangeListener( l );
		}
		public void removePropertyChangeListener( PropertyChangeListener l ) {
			changes.removePropertyChangeListener( l );
		}
	}

	// some main()
	Person p = new Person();
	p.addPropertyChangeListener( new PropertyChangeListener() {
		@Override public void propertyChange( PropertyChangeEvent e ) {
			System.out.printf( "Property '%s': '%s' -> '%s'%n", e.getPropertyName(), e.getOldValue(), e.getNewValue() );
		}
	} );
	p.setName( "Ulli" ); // Property 'name': '' -> 'Ulli'
	p.setName( "Ulli" );
	p.setName( "Chris" ); // Property 'name': 'Ulli' -> 'Chris'
```


### Constraint properties
We can define properties as constraint properties. If the value of such a property is supposed to be changed, this change is communicated to all listeners. Listeners which are allowed to intervene can then prevent such a change.

```java
	package com.tutego.insel.bean.veto;
	import java.beans.*;
	public class Person {
		private boolean bigamist;
		private PropertyChangeSupport changes = new PropertyChangeSupport( this );
		private VetoableChangeSupport vetos = new VetoableChangeSupport( this );
		public void setBigamist( boolean bigamist ) throws PropertyVetoException {
			boolean oldValue = this.bigamist;
			vetos.fireVetoableChange( "bigamist", oldValue, bigamist );
			this.bigamist = bigamist;
			changes.firePropertyChange( "bigamist", oldValue, bigamist );
		}
		public boolean isBigamist() {
			return bigamist;
		}
		public void addPropertyChangeListener( PropertyChangeListener l ) {
			changes.addPropertyChangeListener( l );
		}
		public void removePropertyChangeListener( PropertyChangeListener l ) {
			changes.removePropertyChangeListener( l );
		}
		public void addVetoableChangeListener( VetoableChangeListener l ) {
			vetos.addVetoableChangeListener( l );
		}
		public void removeVetoableChangeListener( VetoableChangeListener l ) {
			vetos.removeVetoableChangeListener( l );
		}
	}

	// some main()
	Person p = new Person();
	p.addPropertyChangeListener( new PropertyChangeListener() {
		@Override public void propertyChange( PropertyChangeEvent e ) {
			System.out.printf( "Property '%s': '%s' -> '%s'%n",
			e.getPropertyName(), e.getOldValue(), e.getNewValue() );
		}
	} );

	try {
		p.setBigamist( true );
		p.setBigamist( false );
	}
	catch ( PropertyVetoException e ) {
		e.printStackTrace();
	}

	// until here everything is fine, now we add a contraint
	p.addVetoableChangeListener( new VetoableChangeListener() {
		@Override
		public void vetoableChange( PropertyChangeEvent e )
		throws PropertyVetoException {
			if ( "bigamist".equals( e.getPropertyName() ) )
				if ( (Boolean) e.getNewValue() )
					throw new PropertyVetoException( "Nimm zwei ist nichts für mich!", e );
		}
	} );

	try {
		p.setBigamist( true );
	}
	catch ( PropertyVetoException e ) {
		e.printStackTrace();
	}
```


# JavaFX Beans
These are like java beans but not really, since they use additional classes which are only available with javafx.beans but not with java.beans.

```java
	public class Person {
		private final StringProperty name = new SimpleStringProperty();
		private final IntegerProperty age = new SimpleIntegerProperty();

		public StringProperty nameProperty() { return name; }
		public final String getName() { return name.get(); }
		public final void setName( String name ) { this.name.set( name ); }

		public IntegerProperty ageProperty() { return age; }
		public final int getAge() { return age.get(); }
		public final void setAge( int age ) { this.age.set( age ); }
	}
```

JavaFX uses a special set of container types, the SimpleXXXProperty, to declare its properties. These are interfaces, not classes. For these we dont have to implement the actual interfaces, we use the already implemented SimpleXXXProperty classes. When we implement the actual setter and getter, we use the set() get() methods from the SimpleXXXProperty classes.
Note: we are only allowed to use the .set() and .get() methods in the actual setter and getter, NO OTHER CODE IS ALLOWED! since we can actually call .set and .get from somewhere else ... information would be lost! Thats why the setter and getter are final.

Note: we actually have JavaBeans common properties (getter/setter) vs JavaFX properties (XXXProperties). Depending on the design, we can actually forgo the getter/setter and just use the .set/.get methods.


## Sidenote: FX listener
Java FX also provides Observer/Observable classes which are implemented by the XXXProperty types (so rum richtig?)

javafx.beans.Observable, javafx.beans.value.ObservableValue<T>


## FX Beans property binding using interface Property
The java API provides a nice range of functions when binding the state of objects to listeners and automatically updating stuff. Using property binding, a property is coupled with a target. if the target changes its state, the property itself changes automatically as well. These dependencies are managed by the java library, 

All XXXProperty classes also implement the interface Property, which requires the implementation of various binding methods.

```java
	interface javafx.beans.property.Property<T> extends ReadOnlyProperty<T>, WritableValue<T>
	- void bind(ObservableValue<? extends T> observable)
	- void bindBidirectional(Property<T> other)
	- boolean isBound()
	- void unbind()
	-K void unbindBidirectional(Property<T> other)
```


### Unidirectional binding using method bind()

```java
	IntegerProperty carSpeed	= new SimpleIntegerProperty();
	IntegerProperty driverSpeed = new SimpleIntegerProperty();
	driverSpeed.bind( carSpeed );					// every driver has the same speed as the car. if the speed of the car changes, so should the drivers.
	carSpeed.set( 200 );
	System.out.println( driverSpeed.get() );		// 200
```

Note: once a property is unidirectionally bound to another one, it cannot be changed any longer except by the property its bound to


### Bidirectional binding using method bindBidirectional()
```java
	// if one changes, the other does too
	ObjectProperty<Instant> time1 = new SimpleObjectProperty<>();
	ObjectProperty<Instant> time2 = new SimpleObjectProperty<>();
	time1.bindBidirectional( time2 );
	time1.set( Instant.now() );
	System.out.println( time1.get()); // 2014-03-05T18:46:39.110Z
	System.out.println( time2.get()); // 2014-03-05T18:46:39.110Z
	time2.set( Instant.now() );
	System.out.println( time1.get()); // 2014-03-05T18:46:39.303Z
	System.out.println( time2.get()); // 2014-03-05T18:46:39.303Z
```
Note: the bidirectional binding can be severed by method unbindBidirectional() 


### XXXBinding classes
Binding is an interface that is conveniently implemented by many XXXBinding classes. These classes are Observable classes, they can be bound by other instances. XXX again is a placeholder for implemented datatypes e.g. Number, String, etc.

```java
	IntegerProperty a = new SimpleIntegerProperty( 3 );
	IntegerProperty b = new SimpleIntegerProperty( 2 );
	NumberBinding formular = a.multiply( 2 ).add( b.multiply( 2 ) );	// here we use class NumberBinding
	System.out.println( formular.getValue() ); // 10
	IntegerProperty perimeter = new SimpleIntegerProperty();
	perimeter.bind( formular );										// here we bind our IntegerProperty to the NumberBinding class. perimeter now is bound to formular 
	System.out.println( perimeter.get() ); // 10
	a.set( 6 );														// when we reset a, the content of formular is recalculated and the change is automatically included in perimeter
	System.out.println( perimeter.get() ); // 16
```

Bindings are extremely important when implementing interactive GUIs. if we e.g. implement a slider, we want to provide the user with correlating visual feedback (changing numbers, etc.) so the user knows when the required value has been set.

```java
	package com.tutego.insel.fxbean;
	import javafx.application.Application;
	import javafx.scene.*;
	import javafx.scene.control.*;
	import javafx.scene.layout.VBox;
	import javafx.scene.shape.Circle;
	import javafx.stage.Stage;
	public class JavaFXBeanBindingExample extends Application {
		public static void main( String[] args ) {
			launch( args );
		}
		@Override
		public void start( Stage primaryStage ) {
			Slider slider = new Slider( 10, 100, 40 );
			Circle circle = new Circle();
			circle.radiusProperty().bind( slider.valueProperty() );
			TextField radius = new TextField();
			radius.textProperty().bind( slider.valueProperty().asString( "%.2f" ) );
			TextField area = new TextField();
			area.textProperty().bind( slider.valueProperty().multiply( slider.valueProperty() )
			.multiply( Math.PI ).asString( "%.2f" ) );
			Node[] nodes = { slider, new Label( "Radius:" ), radius,
			new Label( "Fläche:" ), area, circle };
			primaryStage.setScene( new Scene( new VBox( 4, nodes ), 300, 400 ) );
			primaryStage.show();
		}
	}
```


### Invalid bindings
One thing we have to know when using bindings is, that properties and bindings only evaluate their state when they are required ("lazy evaluation"). In detail if a property changes its value, it sets all binding states to invalid. if other changes happen, then the state simply stays invalid. only if a bound property is used, it checks the binding state, if the state is invalid, it gets the value from the main property and the binding state is set to valid again.


### Building custom bindings
We can easily create custom bindings by extending existing ones:
```java
	DoubleProperty a = new SimpleDoubleProperty( 6 );
	DoubleProperty b = new SimpleDoubleProperty( 8 );
	DoubleBinding hypotenuse = new DoubleBinding() {
		{ super.bind( a, b ); }
		@Override
		protected double computeValue() {
			return Math.sqrt( a.get() * a.get() + b.get() * b.get() );
		}
	};
	System.out.println( hypotenuse.getValue() ); // 10.0
	a.set( 15 );
	System.out.println( hypotenuse.getValue() ); // 17.0
```


# Java class library

## Class loader
Class loaders are required to (duh) load a class. The loader is required to get the information from a .class file into the actual JVM.

### how loading a class work:
'''java
	class Person {
		static String now = new java.util.Date().toString();
		public static void main( String[] args ) {
			Dog wuffi = new Dog();
		}
	}
	class Dog {
		Person master;
	}
'''
When a JRE starts the program person, it first has to load classes. in our case of course class Person has to be available. since person also contains a call of the classes Dog and Date, they have to be loaded as well. furthermore all superclasses of the used classes have to be loaded as well.
Note: The JRE loads all included classes automatically, but classes can be manually loaded as well using '''Class.forName(String)''' and the name of the respective class.

### Three layer search for classes
to load classes, the classes have to be found first.
- Bootstrap class loader: Classes like Object, String, etc, reside in a special archive (e.g. rt.jar) which the JVM searches first for required classes. These classes are called bootstrap classes.
- Extension class loader: If the JVM does not find the classes in the bootstrap archives a special folder lib/ext is searched.
- Application class loader: Only then the search is extended to the classpath. This classpath can be set manually and of course should contain the root of the project

'''java
	$ java -classpath classpath1;classpath2 MainClass
or
	$ SET CLASSPATH=classpath1;classpath2
	$ java MainClass
'''

### Accessing operating system environment variables
We can access the operating system environment variables using class '''java.lang.System''' and the static methods:
	static Map<String, String> getEnv()	...	reads in a list of environment variables
	static String getEnv(String name)	... reads specific environment variable "name", returns null if the variable is not set.
	
examples of variable names under windows:
	COMPUTERNAME, HOMEDRIVE, HOMEPATH, PATH, PATHEXT, SYSTEMDRIVE, TEMP, TMP, USERDOMAIN, USERNAME

## Using external programs from java
External programs can be executed by using '''Runtime.getRuntime().exec(command);''' or '''new ProcessBuilder(command).start();''' Return values can be passed back to the java application:

'''java
	ProcessBuilder builder = new ProcessBuilder( "cmd", "/c", "dir" );
	builder.directory( new File( "c:/" ) );
	try {
		Process process = builder.start();
		try ( InputStream in = process.getInputStream();
		Scanner scanner = new Scanner( in ) ) {
			System.out.println( scanner.useDelimiter( "\\Z" ).next() );
		}
	}
	catch ( IOException e ) {
		e.printStackTrace();
	}
'''

Note: ProcessBuilder can e.g. be used to set environment variables.

Using class Process running external programs can be checked and ended.


Note: The classes System.in and Console can be used to get user entries into a program. 
Note: Use package java.time whenever we want to include date or time. do not use any of the other date packages!
Note: using line.separator retrieves the proper line feed value for the current operating system. but if we use "%n" or "\n" we always get the proper line feed any way.


(901)




replace ' '' with ``` at ce end of ce jour














Google clean code talks - dont look for things:
- only ask for things you directly need
- use as little new operators within constructors as possible, rather hand objects over to your constructor.















# UML conventions:

public is indicated by +, private by -, protected #, visible in package ~
static methods are underlined (I use _ after the visibility modifier)

	// Playground depends on Player since it requires a Player object in main
	Player							<-----			Playground					
	------											----------
	+ name											----------
	+ item											+ main(args: String[])
	------
	+ clearName()
	+ hasCompundName(): boolean


There is no specific UML description for constructors, they are simply added as methods.


## Interactions/Associations in UML:

Interactions between classes are indicated by solid connections. Arrow direction indicate information flow. In case of a 1:1 unidirectional connection, there will be only 1 arrow, in case of a 1:1 bidirectional connection, there will be arrows on both ends.

	Player				+room	Room
	------			------->	----
	+ room: Room			1	----
	------

Player has one unidirectional association with Room, Room does not know about Player.


## Inheritance in UML

				GameObject
				----------
				+ name: String
				----------
					^
					|
		-------------------------
		|						|
	Room					Player
	-----------				------
	+ size: int				------
	-----------

Room and Player both extend GameObject, the inheritance is symbolized using a large, empty arrowhead in direction of the superclass.

Abstract classes are annotated as italics in UML.


## Interfaces in UML

	<<interface>>
		Byable
	-------------
	-------------
	+ price(): double







# Naming conventions:
	class names		... start upper case	... makes it easy to distinguish between static and object methods
	variables		... start lower case	... makes it easy to distinguish between static and object methods
	constants		... fully upper case
	package names	... fully lower case

	parameter ... declared variable of a method
	signature ... name and parameters of a method

# Dump interesting stuff:
	easy way to store undirected graphs:
	adjacent matrices ... 2D array, contains information about an edge and the information if this edge is connected to a node (by true/false)



Testing parameters for consistency and throwing IllegalArgumentExceptions on a controlled basis:
	org.apache.commons.lang.Validate		http://commons.apache.org/lang/
	com.google.common.base.Preconditions	http://code.google.com/p/guava-libraries/

Testing for null:
	Objects.requireNonNull(reference); // throws an IllegalArgumentException if the reference is null






Further links

Java tips
http://www.javaworld.com/blog/java-tips/

String performance:
http://java-performance.info/string-intern-in-java-6-7-8/

Trove Collections
http://java-performance.info/primitive-types-collections-trove-library/#more-7
