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
# 7
---
**Dictionary** - Attaches values to keys. Contains operations such as:
- lookup
- update/insert
- delete
- empty
- Missing

**Implementations**:
- Association list, a list of tuples. `O(n)` lookup time, `O(1)` update time.
- Binary search tree, a binary tree where keys can be in some order, `O(log n)` lookup, `O(n)` if unbalanced.
- Often OCaml updates structures by using the initial structure to return a new structure with changes. The original is not changed. 

**Tree Traversal**:
- preorder: start with root node, take the label, then go down the left branch recursively, then go down the right recursively
- inorder: start with the root node, go down the left branch recursively, then take the label, then go down the right recursively
- postorder: start with the root node, go down the left branch recursively, go down the right branch recursively, then take the label

**Arrays** - An indexed data structure. Usually updated in place, or *mutable*. Differ from lists and trees as you don't have to start at the "top".
**Functional Array** - Maps integers to data. Can be implemented as binary search trees for efficient `O(log n)` lookup.
![[Pasted image 20231020103937.png]]

Subscripts or indices can be represented in binary, because it can represent *conditional logic*.
Binary is often preferred for digital electronics because of this, not in spite of it.
# 8
---
**Functions as Values** - In ocaml functions can be passed around as parameters, returned as results, exist in lists/arrays etc, but NOT tested for equality.
Functions can be defined without names (lambdas).

`let double = fun n -> n * 2` is the same as `let double n = n * 2`

`function` keyword is used for pattern matching, as it automatically accepts a parameter and matches it against following patterns.

**Currying** - Expressing a function taking multiple arguments as taking *nested functions*. 
`let multiply = fun n1 -> (fun n2 -> n1 * n2)`

Passing in not all parameters will return a version of the curried functions that is not fully completed - you can call this new function with the remaining parameters to get the final results.
e.g
`let prefix a b = a ^ b`
`let professor_prefix = prefix "Professor "`
`let bracewell = professor_prefix "Bracewell"`
this is known as *partial application*.

You can also define custom functions that are then used in other functions, for example a comparison function for a custom type, or a filter function for filtering items in a list (predicates).

**Map** - A popular function used to apply a transformation to every item in a list. It takes in anonymous function.

One use of partially-applied functions is matrix multiplication. Since one row needs to be multiplied with many columns on the other matrix, you can write a function that performs the `row * column ` step, and then partially apply that to a row and then *re-use* that function for all the columns it needs to be multiplied by.
# 9
---
**Sequential Program** - A program that takes some input and runs to completion with an output. For example exhaustive search, data processing.
**Reactive Program** - A program that runs as long as necessary and responds to incoming inputs and gives outputs or performs side effects. For example control tasks, resource allocation, scheduling incoming jobs.

Reactive programs need a functional model to get inputs, evaluate and act upon those inputs, for example a *perception-action loop*.

**Pipeline**:
![[Pasted image 20231025102556.png]]
where:
- Produce sequence of items
- Filter sequence in stages
- Consume results as needed
- Lazy lists join the stages together.

**Lazy Lists** - Lists of possibly infinite length
- Elements computed upon demand
- Avoids waste if there are many solutions
- Infinite objects are a useful abstraction

OCaml implements laziness by delaying evaluation of the tail.
`type unit = ()`

You can delay evaluation of an expression by putting it in an anonymous function with a unit arguments:
`fun () -> E`

**Type of a Lazy List**:
```ocaml
type 'a seq =
	| Nil
	| Cons of 'a * (unit -> 'a seq)

let head (Cons (x, _)) = x
let tail (Cons (_, xf)) = xf ()
```
# 10
---
**Tree Traversal** - Visiting every single node in a tree.

**Preorder** - Visiting the node, then visiting the left tree, then visiting the right tree
**Inorder** - Visiting the left tree, then visiting the node, then visiting the right tree
**Postorder** - Visiting the left tree, then right tree, then node.
^ These are depth-first traversals

**Breadth-First** - Visiting all nodes in each layer
**Depth-First** - Visiting nodes all the way to the bottom of the tree.

Binary trees can act as *decision trees*.
Breadth-first search can be very inefficiently implemented.

```ocaml
let rec nbreadth = function 
	| [] -> [] 
	| Lf :: ts -> 
		nbreadth ts 
	| Br (v, t, u) :: ts -> 
		v :: nbreadth (ts @ [t; u])
```
Works by removing tree from head, and putting sub-trees onto end of list (VERY inefficient)

**Queue** - A new datatype for first-in-first-out.
This can be easily represented just by a pair of lists! 
- Add new items to rear list
- Remove item from front list (if empty then REVERSED REAR becomes FRONT list), initialise new empty rear list
- Amortized cost is `O(n)`

`type 'a queue = Q of 'a list * 'a list`
^ enumeration for extra protection so users don't accidentally instantiate pair of lists as queue.

Can use a queue for breadth first search by adding node, then enqueue both subtrees and dequeue last sub tree. This puts the next "level" of subtrees later in the queue to be traversed, and then continues from the neighbouring tree (enqueued from the level above).

**Iterative Deepening** - Use DFS to get benefits of BFS. Recompute nodes at depth `d` instead of storing them. Essentially this preforms repeated DFS, discarding previous results. Improves space complexity, time complexity stays same.
![[Pasted image 20231030173511.png]]
Start with a factor `k`, go down to `k` level. If compute time left, 

**Stack** - An abstract data structure behind depth-first search, can be implemented in OCaml as just a list. Important to look at operations of each data structure and consider how to implement with minimal complexity.
![[Pasted image 20231030173823.png]]

The data structure determines the search method!
![[Pasted image 20231030173835.png]]
**Priority Queue** - An ordered sequence with some sort of ranking to the nodes. Using heuristics to improve efficiency.
# 11
---
**Procedural Programs**:
- Can change the machine state (variables or sending/receiving data)
- interact with its environment
- use control structures

They use data abstractions of computer memory - e.g. references to memory cells, arrays as blocks of cells, linked structures using e.g. pointers to memory addresses.
In functional program the store is an invisible device inside a computer, in procedural it is visible.

**Reference** - Storage locations, can be created, inspected or updated.
`'a ref` - a reference to type `'a`
`ref E` - create a reference with initial contents `E`
`!P` return the current contents of reference `P`
`P := E` - update the contents of `P` to `E`

Types:
`ref : 'a -> 'a ref`
`! : 'a ref -> 'a`
`:= : 'a ref -> 'a -> unit`

**Aliasing** - When two values refer to the same mutable cell.

**Commands** - Expressions with effects. Basic commands can update references, write to files etc. A typical command will return the empty tuple/unit type. 
`C1;C2;C3...Cn` will evaluate all `C`s and then return the value of `Cn`.
OCaml provides a `while` loop construct.

You could implement *private references* in OCaml by declaring the reference in a let binding for a function, and then returning that function. Or even an entire class such as:
```ocaml
let student name age height subject = 
	let _name = ref name in
	let _age = ref age in
	let _height = ref height in 
	let _subject = ref subject in

	let get_name = fun () -> !_name in
	let get_age = fun () -> !_age in
	let get_height = fun () -> !_height in
	let get_subject = fun () -> !_subject in

	let set_name name = _name := name in
	let set_age age = _age := age in
	let set_height height = _height := height in
	let set_subject subject = _subject := subject in

	let had_birthday = fun () -> _age := (!_age + 1) in
	let lost_both_legs = fun () -> _height := (!_height - 20) in

	(get_name, get_age, get_height, get_subject, set_name, set_age, set_height, set_subject, had_birthday, lost_both_legs)
;;
```

OCaml also implements mutable arrays as collections of references.

