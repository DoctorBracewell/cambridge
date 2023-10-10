# 1
---
### Levels of Abstraction
**Levels of Abstraction** - No one can fully understand every level of how a computer works, but can focus on 2-3 levels to solve a smaller set of problems and create a single system.
**Recurring Issues**
- What services to provide at each level
- How to implement a higher level using lower-level services
- The interface by which two levels should communicate

### Dates Example
**Provided Service** - Represent dates over a certain interval
**Concrete Implementations** - UNIX timestamps, represent in YYMMDD, etc. etc.
Very important to consider how to implement the service, e.g. Y2K crisis or PDP-10 being *inadequate* implementations.

### Floating Point Numbers Example
**Provided Service** - Represent a floating point number. Important to consider how it might interact with existing datatypes. Precision, invalid results, etc.
**Concrete Implementation** - Two integers, base + exponent.
**Interface** - Operations on the number, invalid results (e.g. infinity, /0).

### Goals of Programming
- To describe a computation so that it can be done mechanically
	**Expressions** - Compute values
	**Commands** - Cause effects
	Important to separate these in order to reduce complexity/unknown behaviour of a program.
- To do so efficiently and correctly
- To allow easy modification as our needs change
	Through an orderly structure based on abstraction principles, both at writing-time and run-time
	Should be able to predict the effects of changes
	
### Why OCaml?
- Interactive
- Flexible notion of data type
- Hides the underlying hardware
- Functional programs can be understood mathematically
- Distinguishes naming from updating memory
- Manages memory/storage

### OCaml Basics
```ocaml
let rec npower x n =
  if n = 0 then 1.0
  else x *. npower x (n - 1)
```
# 2
---
### OCaml Tips
- Lazy evaluation of if-else
- Type inference with optional marking
- Different operators for different data types (`*` vs `*.`)
- Mark functions as recursive

### Expression Evaluation
**Functional Programming** - A paradigm that generally focuses on separating expressions from effects.
**Summing Integers** - A recursive approach to summing integers can result in a lot of redundant values being required all the way until the final calculation. Iteratively summing the integers using an "intermediate" state value (accumulator) can be more efficient.
```ocaml
let rec nsum n = if n = 0 then 0 else n + nsum (n - 1)
```
```ocaml
let rec summing n total = if n = 0 then total else summing (n - 1) (n + total)
```
### Recursion vs Iteration
**Iterative** - Normally refers to a loop
**Tail-Recursion** - The recursive function call being the last thing that the expression does.
Iterative code resembles code written with `while` loops in procedural languages.
Often it saves memory space.
Many functions can be expressed elegantly with recursion but awkwardly with iteration.

### Comparing Algorithms
**Asymptotic Complexity** - How program costs grow with increasing inputs. Usually space or time, where time is usually larger than the former.
Complexity should consider the *worst case* possible given an input of size `n`.
![[Pasted image 20231009103820.png]]
Here the `complexity` column represents some arbitrary function that performs some number of steps depending on `n`.

### O-Notation
- `f(n) O(g(n))` provided that `|f(n)| <= c |g(n)|` as `n -> infinity`
- Look for the most significant term
- Ignore constant as they do not dominate as n increases.

**Common Complexity Classes**:
- O(1) - constant
- O(log n) - logarithmic
- O(n) - linear
- O(n log n) - quasi-linear
- O(n^2) - quadratic
- O(n^3) - cubic
- O(a^n) - exponential
**Complexity Units** - The actual complexity of a program depends on what you *choose* as a single unit/step, e.g. a division is often seen as 1 step but can be computationally expensive.

### Recurrence Relations
Defining a recurrence relation can often help illustrate the complexity of a (recursive) algorithm.

`T(n + 1) = T(n) + 1` -> `O(n)`

