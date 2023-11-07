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
