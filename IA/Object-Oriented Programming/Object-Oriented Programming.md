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
