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

# 3
---
### Lists
**List** - A finite sequence of elements of the same type. Lists can be appended with `@`
`x :: l ` represents a list with length `x` and tail `l`
![[Pasted image 20231011102136.png]]

A list of type `α` can be represented as `α :: α list`
Internally lists are represented as a *linked structure*, each item links to another one until the final one links to an empty list. Taking a list's head or tail (tail being the *list* associated after the head) takes constant time.

Often manipulation uses recursion, but iteration can also be more efficient similar to earlier examples.

### Strings and Characters
**Characters** - A single constant character, primitives defined with `'`
**String** - A collection of characters, primitives defined with `"`. Strings can be appended with `^`.
# 4
---
### More on Lists
**Take** - Create a new list from the first `i` items of an existing list `xs`
```ocaml
let rec take i = function
  | [] -> []
  | x::xs ->
      if i > 0 then x :: take (i - 1) xs
      else []
```
**Drop** - Create a new list by discarding the first `i` items of an existing list `xs`
```ocaml
let rec drop i = function
  | [] -> []
  | x::xs ->
      if i > 0 then drop (i-1) xs
      else x::xs
```

![[Pasted image 20231013102212.png]]

**Linear** - Going through each element in a list one by one, `O(n)` time complexity
**Ordered** - Going through an ordered list with a binary subdivision method, `O(log n)` time complexity
**Indexed** - Looking up an item directly at an index, `O(1)` time complexity

**Zip** - Combine two lists into a list of pair tuples
```ocaml
let rec zip xs ys =
  match xs, ys with
  | (x::xs, y::ys) -> (x, y) :: zip xs ys
  | _ -> []
```
# 5
---
### Sorting
**Sorting** - Once a set of items is sorted, it simplifies many other problems in computer science. Sorting has many applications, including:
- Fast search
- Fast merging
- Finding duplicates
- Inverting tables
- Graphics algorithms

**Sorting Complexity** - Measured such that a single unit of complexity is one **comparison**. Typically you can compare sorting algorithms by counting the number of comparisons `C(n)`
There are `n!` permutations, each comparison eliminates half of the permutations, such that the lower bound of comparison is `O(n log n)`

### Insertion Sort
```ocaml
let rec ins = function
	| x, [] -> [x]
	| x, hd::tl -> 
		if x < hd then
			x :: hd :: tl
		else
			hd :: ins (x, tl)

let rec insort = function
	| [] -> []
	| hd :: tl -> ins (hd, insort tl)
```

Insertion sort works by inserting each element into its correct place then moving on to the next item until it reaches the end of the list.
It's important to understand if there is anything you might know about the input you should decide which algorithm to use. For example, insertion sort performs in the worst case when the input is in reverse order, but very well when the data is mostly ordered already.
Worst case complexity of insertion sort is `O(n^2)`
### Quicksort
**Quicksort** - A very popular divide-and-conquer algorithm for sorting data.
- Chose a pivot element
- Divide the input into two sublists:
	- Those elements that are lower than or equal to the pivot
	- Those elements that are greater than the pivot
- Use recursive calls to sort those sublists
- Combine the two sorted lists by appending to each other

Quicksort's average case complexity is `O(n log n)`, but worst case is `O(n ^ 2)`.
### Merge Sort
**Merge Sort** - A sorting algorithm that recursively splits an input into multiple sub-lists, then combines them together in order.

```ocaml
let rec merge = function
	| (x, []) -> x
	| ([], y) -> y
	| (hdx::tlx, hdy::tly) -> 
		if hdx <= hdy then 
			hdx :: merge (tlx, hdy::tly)
		else
			hdy :: merge (hdx::tlx, tly) 

let rec mergesort = function
	| [] -> []
	| [x] -> [x]
	| inp -> 
		let k = List.length inp / 2 in
		let l = mergesort (take k inp) in
		let r = mergesort (drop k inp) in
		merge (l, r) 
```

Complexity of merge sort is `O(n log n)`, *even in the worst case*, which is better than quicksort. However quicksort is often more used because it has a lower space complexity and most of the time performs better. 

**Match the algorithm to the application**
### Comparing Sorting Algorithms
**Insertion Sort** - Simple to code, quadratic complexity
**Quicksort** - Fast on average, quadratic complexity in the worst case
**Mergesort** - Optimal in theory, often slower than quicksort in practise.
# 6
---
OCaml has a very strong algebraic datatype system, including:
### Enums
**Enumeration** - A datatype formed of a finite number of different options. These options can hold additional values (of different types). Enums are pattern matchable and very powerful in allowing you to not represent invalid states.
### Exceptions
**Exception Handling** - Raising an exception abandons the current expression, and handling the exception attempts an alternative.
Raising and handling can be separated in the source code.

Introduce exception types using the `exception` keyword, and raise any particular one with `raise`.
e.g.
```ocaml
exception Fail
exception FailWithData m

raise Fail
raise FailWithData "oopsie!"
```
### Binary Trees
```ocaml
type 'a tree =
	| Lf
	| Br of 'a * 'a tree * 'a tree
```
