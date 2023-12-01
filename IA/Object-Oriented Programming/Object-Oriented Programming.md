### Introduction
Large software gets complicated quickly. By 1960s looking for a way to structure code more efficiently and elegantly.

**Maintainability** -
- Locate specific code responsible for feature or failure
- Simple to understand what code does
- Simple to add or remove new feature
- Simple to change existing behaviour
- Make it difficult to introduce new bugs

**Declarative** - Specify what to do, not how to do it - functional, logic, reactive.
**Imperative** - Specify what and how to do - procedural OOP.
`double x` vs `x = x * 2`

OCaML, for example, may optimise your code by replacing your implementation with something entirely different but 100% equivalent.

**OOP** - Based on the idea of "objects" which contain data and code.
![[Pasted image 20231103112319.png]]
### Java
**Java** - Designed as an OOP language, widely used, forgiving for beginners because doesn't require manual memory management.

Java was intended as an early language to connect many different devices. Traditionally source code would be compiled multiple times for different architectures.
JVM compiles to intermediate representation (bytecode) which runs on JVM (which ships with a lot of libraries).
Source code -> bytecode is the significant compilation step, but bytecode -> machine code still takes some time and convert it so slight performance hit.

**Unit Tests** - Tests that test specific one chunk or unit of code (class, function, etc.)
**Integration Testing** - Testing the overall project in the context its designed for
**Regression Testing** - Ensuring that code changes don't break things (e.g. running all tests after a change)

**Testing**:
- Arrange - set up world, instantiate objects, send inputs etc.
- Act - perform action using arranged world
- Assert - ensure that result was as expected
### State
**State** - Data that can be observed and changed, often used for modelling interaction.
**Values** - A piece of data stored in memory, a variable is a name used to refer to this data.
Variables can have a primitive type or a reference type.

**Primitive Bytes** - 
![[Pasted image 20231106111701.png]]

**Type Conversion** - Types can be promoted or narrowed to another type. Types can be implicitly promoted to the widest range, or explicitly narrowed (dangerous because you could lose information).
Local variable type inference can be useful for less boilerplate but shouldn't necessarily be used all the time.

**Arrays** - A sequence of memory locations of the same type.
### Behaviour
**Prototype** - Specifies the function name, arguments and possibly return type.
**Body** - The actual code of the function.

Technically these functions are procedures, as they can have side effects and manipulate state outside of the function. Function names should be camelcase verbs.

**Overloading Procedures** - Same name, different arguments, possibly different return type (not JUST a different return type).
### Objects & Classes
**Object** - A bundle of state and behaviour. State is defined through its fields, behaviours defined through its methods.
**Class** - A blueprint for a specific type of object. Methods of a class should be seen as an API. In Java all source code must be contained in classes.

**Constructor** - A function that generates an object from a class. These must have the same name as the class and no return type.

Classes should be a collection of conceptually related state and behaviour that use and are used by other classes to build up a maintainable project.
### Good Classes
![[Pasted image 20231108111547.png]]

**Has-A Association** - Arrow with a number (or range) to another class. Defines how many variables that somehow links between them. E.G a college has zero or more students, a student has exactly one college.

A single class should represent a sub-unit of code that can be **designed**, **developed** and **tested** separately to all other code.
**Single Responsibility Principle** - The class has one responsibility. E.G. combing the model and view components, forcing people to either use/import your entire implementation or don't use it.
**Benefits of SRP**:
- Easier to understand in smaller chunks
- Class is easier to maintain because changes are isolated
- Simpler to re-use because no unnecessary responsibilities.
**Cohesion** - Measures how strongly related the responsibilities and data of a class are.
- Functional - methods are grouped to solve specific tasks
- Informational - methods are grouped because they work on the same domain
- Sequential - Outputs of methods become inputs of other methods.
### Encapsulation
**Encapsulation** - Supporting tools to hide things away that a user of a class shouldn't even need to know about.

In Java, you can make class properties private so that only class methods can access that data.
Then you can implement getters and setters in the class to make it available to other classes. 
Setters could implement validity checks on the new data.

Often you can write the class API before you write any implementation or state of the class.
![[Pasted image 20231108114252.png]]

`final` access modifier allows immutability of data. Internal properties can be set on initialisation of the class, but not in any subsequent methods. The `record` modifier automatically makes a class immutable.

Immutability benefits:
- Reduce scope for bugs
- Can be thread-safe
- Easier to reason about
### Static Variables
A **static** variable is only created once in the program's execution, despite being declared as part of a class.
Mutating a static variable will change all "instances" of the variable across the program. It is best to reference the static variable using the class name. 
- Auto-synchronised
- Space efficient
- Can be confusing.

Also used for methods attached directly onto the class.
![[Pasted image 20231110111508.png]]
### References
**Primitives** - Held directly in memory and simply copied when moved.
**Reference** - The value of associated data in memory is a memory *address* for where to find all the data associated with that object. Updating or changing the reference will update it for all existing references to that object.
### Stack & Heap
A function's data includes:
- Any local variables created by the function
- Any arguments that the function was called with
- A memory address for where to return to when the function is done
All of these things can be stored in a **stack frame**. 
A stack data structure is used as a last-in-first-out structure with a stack pointer in order to run functions that call each other in order. 

Stack frames have specific amounts of memory and are popped on/off, so are not suitable for objects or mutable values. 

The **heap** is a chunk of memory that often has many holes and gets fragmented, but offers more flexibility for storing mutable memory.
### Pointers
A memory address of the first memory slot used by the variable, and the type tells the compiler how many slots are used.
There are more flexible than references but more dangerous

![[Pasted image 20231110114853.png]]
### Pass-by-value
Passing a value into a function results in a copy of that value. In Java all function calls are pass-by-value, but passing a reference in as a function results in that reference being copied.
### Inheritance
**Inheritance** - The notion of capturing hierarchy in classes. You can see certain classes as specialisations of others.

When a class inherits from another class:
- It inherits its type.
- Incoporates all the attributes and behaviour from that class.
- Can directly access public and protected members of that class but not private
- Can redefine some inherited behaviour or add new attributes and behaviour.

All classes inherit from `Object`.

Child class, subclass, derived class
inherits from
Base class, superclass, parent class 

![[Pasted image 20231113111829.png]]

Inheritance can help with *type casting* - an `Integer` inherits from `Number`, and so any integers can be cast to a `Number`.

A widening cast is making something more general (moving up the inheritance tree) (`Student` -> `Person`).
A narrowing cast makes something more specific (moving down the inheritance tree) but extra care is needed because there might not necessarily be enough information to do this. 

**Overriding** - Replacing an already-existing method from a superclass with the same signature. The `@Override` notation can be used for clarity. 

**Abstract Classes** - A technique to force any subclasses to implement a method while a default implementation in the base class. You cannot instantiate an abstract class, it only acts as an interface for any classes that extend it.
All state and non-abstract methods are still inherited as normal by child classes.

**Interface** - A collection of exclusively abstract functions that force classes to inherit from them to provide concrete implementations.
They have no state whatsoever, 
![[Pasted image 20231113115100.png]]
### Polymorphism
**Parametric Polymorphism** - i.e. generics
**Ad-hoc Polymorphism** - i.e. overloading
**Subtyping Polymorphism** - many types of objects providing the same method that can be invoked without knowing which object it is.

Take
![[Pasted image 20231115111108.png]]
**Static Polymorphism** - Decided at compile-time, since we don't know the true type of `p` just run the parent method.
**Dynamic Polymorphism** - Run the method in the child, must be done at run-time since that's when the child's type is know. 
### Multiple Inheritance
**Multiple Inheritance** - One class inheriting from multiple sources. This introduces some problems:
- Name clashes between both sources, which one should the child inherit.
- Inherit state from both classes, often with same name. Can lead to nasty and confusing syntax.
- Diamond problem, where one class has multiple inheritance which have one parent class, leading to even more name clashes.

In Java, classes can have at most one direct parent. But multiple *interfaces* can be directly inherited because they don't have any state or methods. Often this helps eliminate name clashes.

**API Evolution** - Since the JDK was so widely used, it was difficult to add new features to the API without breaking things. For example, making a commonly-used class implement a new interface would break existing uses because there would now be undefined methods.
They eventually solved this by adding default methods to interfaces.
### Good OOP
Make your classes **open to extension** but **closed to modification**.
Someone using your class shouldn't have to modify it, it should be easy for them to extend (by making new classes, inheriting it, etc. etc.)

**Liskov Substitution Principle** - Subtypes must be behaviourally substitutable for their base types without negative side effects.
For example, creating a sub class that throws an exception for some abstract method on the parent violates this principle because its a negative side effect.
### Object Creation
![[Pasted image 20231117111259.png]]
Essentially this means a lot more work is done on the first instantiation of a class than any subsequent ones.
### Cleaning Up
A typical program makes a lot of objects, not all of which need to be kept in memory all the time.

**Approach 1**:
- Allow the programmer to specify when objects should be deleted from memory, lots of control but can lead to memory leaks if a mistake is made.

**Approach 2**:
- Delete the objects automatically (garbage collection)
- But can get it wrong, an object might still be needed.

Java implements a garbage collector by *counting references* to an object. If there are none then it can never be accessed again so can be safely deleted.
![[Pasted image 20231117111756.png]]

**Mark and Sweep**:
- Mark: Starting at each stack reference, follow the reference of everything reachable. Mark everything you find
- Sweep: Traverse all objects on the stack, delete any that haven't been marked.

Options for deletion:
- **Delete Immediately** - simplest approach, but can take a while. unpredictable pauses
- **Delete next time** - Queue for deletion if too much time already spent deleting this round
- **Don't Delete** - In some cases decide not to delete (either forever or until we hit a memory critical point).

**Heap Division**:
- Eden, where brand-new objects are just created
- Survivors if they survive a few GCs
- Tenured if they survive a few more GCs.
The GC is run frequently on Eden, less so on Survivors and much less so on Tenured.

**Choices**:
- Serial GC - program stops executing entirely while GC runs.
- Parallel GC - program stops executing but GC is run parallel for faster
- G1 GC - garbage collects concurrently. divides memory into chunks and uses parallel marking
- Epsilon GC - Don't do anything at all (no-op GC). Useful if you know your program will take constant time.
### Destructors
Most OO languages have a notion of a destructor too, gets run when object is destroyed.
Allows you to release any resources (open files, etc.) or memory created especially for that object.

In Java, objects has a `finalize` method that runs when GC actually deletes the object but this is problematic/difficult to predict and generally shouldn't be used.
### Copying Objects
Sometimes you do want to actually copy an object, not just create two references to the same object.

**Shallow Copy** - Just copy the core object, including any reference to other objects
**Deep Copy** - Copy all objects recursively - if the original object has references to other objects, create copies of those objects as well.

A **copy constructor** can be used that takes something of the same type and manually copies the data.

In Java, the `Object` class has a `clone` method. By default this does a shallow copy, but there is a `Cloneable` interface, if you call `clone` on anything that doesn't implement this you get an error.

The `Cloneable` interface is empty and is a **marker interface** - it's only there to ensure that you have to mark any structure as cloneable.
### Collections
**Collection** - Any number of items stored together.
**List** - An ordered collection that may contain duplicates
Java STD offers a lot of different types of lists:
- ArrayList
- LinkedList
- Vector
- Queues, especially priority queues

**TreeMap** - A map (key-value pairs) with keys in order, keys used to access values
**HashMap** - A map with keys not in order, efficient access using hashing.

**Iteration** - Looping over all items in a collection.

**Equals** - All objects have an `equals` method that test for referential equality, often this can overridden to compare for value equality per-object.
Hashmaps also have a `hashCode` function

**Comparable** - An interface that allows you to take two items and return less than, equal, greater than. Used for natural, default ordering.
**Comparator** - A way to implement additional ordering for some specific case.
### Generics
Type systems assign types to key terms in our source code. Assigning types to all of your "things" can be modelled mathematically as a logical system, allowing the compiler to help you find more errors.

**Generics** are part of a type system and aim to allow a type or method to operate on objects of various type whilst providing compile-time errors, i.e parameteric polymorphism.
![[Pasted image 20231122111302.png]]

**Implementing Generics** -
Templates, where all instance of the generic (e.g. `T`) is just replace with all possible types used in the code. Ends up with bloat as many classes end up being made. 

Type erasure, where all the type checks are performed, then delete all of the type information in the compiler output. It can replace everything with just an `Object`. Dynamic checks aren't possible.

Pros:
- Bytecode unchanged, backwards compatible
- Can still have compile time checking
- Avoids bloat of templates
Cons:
- No runtime/dynamic checking
- Can't directly instantiate generic types
- Limits method overloading

For example, this is why you can't have a primitive type as a generic - because e.g. `int` is not an `Object`.

**Generics & SubTyping** - 
![[Pasted image 20231122113915.png]]

When the hierarchy is in the generic parameter you run into issues.
**Covariance** - If B is a subtype of A then I should be able to use B everywhere I expect an A.

However for arrays...
![[Pasted image 20231122114336.png]]
This is happens because arrays in java are **reifiable types**, they know their true type at runtime.
This is the same reason why you can't have an array of types `T`, even though `T` is never instantiated.

For lists, you get a compile type error because of type erasure.
![[Pasted image 20231122114442.png]]
### Wildcards
`?` can be used to represent any type. Useful, for example, since type erasure limits method signaures.

`? extends T` - Allows you to read certain types, but can't write.
### Lambdas
**Options for filtering**:
- Filter imperatively in the function flow
- Create filtering predicate classes and a flexible `filter` function.
- Same but with anonymous classes
- Lambda function

**Functional Interface** - An interface with exactly one, abstract, method. Standard ones include:
![[Pasted image 20231129113218.png]]

**Lambdas**:
- A kind of anonymous function
- That can be passed around
- No name, but parameters, body, return type, exceptions, etc.
![[Pasted image 20231129112618.png]]

**Method References**:
- Let you reuse existing method definitions and pass them just like lambdas.
- ![[Pasted image 20231129113241.png]]
### Streams
**Stream**:
- A fancy iterator with database-like operations.
- A sequence of elements from a source that supports aggregate operations.

Intermediate operations can return another stream, and then a terminate operation can turn the stream back to something else.
![[Pasted image 20231129114046.png]]

This is done *lazily* - only when you ask for a result are the intermediate operations actually performed. In the code you are just "defining" the pipeline.
![[Pasted image 20231129114159.png]]

