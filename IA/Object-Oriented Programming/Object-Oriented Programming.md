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

