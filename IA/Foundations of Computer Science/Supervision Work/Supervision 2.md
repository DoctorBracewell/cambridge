**Name:** Brace Godfrey
**CRSId:** dg681
### 4.2
> Code a function that takes a list of integers and returns two lists, the first consisting of all nonnegative numbers found in the input and the second consisting of all the negative numbers.

```ocaml
let seperate_negatives xs = 
  let rec seperate_negatives_inner xs (l1, l2) = 
    match xs with
      | [] -> (l1, l2)
      | x::xs -> 
        if x < 0 then
          seperate_negatives_inner xs (x::l1, l2)
        else 
          seperate_negatives_inner xs (l1, x::l2)
  in seperate_negatives_inner xs ([], [])
```
### 4.3
> How does this version of zip differ from the one above?
> ![[Pasted image 20231018153735.png]]![[Pasted image 20231018153535.png]]

Both functions share a pattern match that matches when both lists have at least one item left.
However in the second version, the only other pattern used is `([], [])` which matches when both of the lists have no items left. This pattern does not cover the case when one list has no items left and one still has items left, so the matching is not exhaustive. If two lists of different lengths are passed in to the function it will cause an exception.
In the first function, the wildcard `_` is used so any extra items in either list will be discarded.
### 4.6
> We know nothing about the functions f and g other than their polymorphic types: `val f : 'a * 'b -> 'b * 'a` and `val g : 'a -> 'a list`. Suppose that `f (1, true)` and `g 0` are evaluated and return their results. State, with reasons, what you think the resulting values will be.

a)
I think that `f (1, true)` would return `(true, 1)`. The function `f` takes a pair tuple where the first item has generic type `'a` and the second a generic type `'b`. Since the output of this function is a pair tuple where the order of the types has swapped from the input, the function is likely to simply have swapped the order of the data in the tuple. Hence inputting `(1, true)` with a type `int * bool`, the output would be in the form `bool * int`  and therefore must be `(true, 1)`.
It would also be impossible for the function to transform the data in any other way or generate new data of the same type(s), as it does not have enough information about the actual type of the data in the input pair. It can only manipulate the order or "parent" data type surrounding them.

b)
I think that `g 0` would return `[0]`, although it is possible that it would return a list of any length (including 0). The function must return a list of same type as the generic type of the input, but this data type (`'a list`) contains no information about the length of the list. 
However, the list would only be able to contain the input element, as the function would not know enough about the input data type to transform it or generate any other value to put in the list other than the one provided as a parameter.

---
# 5.1
> Another sorting algorithm (selection sort) consists of looking at the elements to be sorted, identifying and removing a minimal element, which is placed at the head of the result. The tail is obtained by recursively sorting the remaining elements. State, with justification, the time complexity of this approach

![[Pasted image 20231018212033.png]]
# 5.2
> Implement selection sort using OCaml.

```ocaml
let rec selection_sort = function
  | [] -> []
  | hd::tl -> 
    let rec traverse curr out = function
      | [] -> curr :: selection_sort out
      | hd::tl -> 
        if hd < curr then
          traverse hd (curr :: out) tl
        else
          traverse curr (hd :: out) tl
    in traverse hd [] tl 
```
# 5.3
> Another sorting algorithm (bubble sort) consists of looking at adjacent pairs of elements, exchanging them if they are out of order and repeating this process until no more exchanges are possible. State, with justification, the time complexity of this approach.

The best case complexity of bubble sort would be `O(n)`, as the list needs to be passed through once in order to check if it is in order, so `n-1` comparisons would be required.
The worst case complexity of bubble sort would be `O(n^2)`, because if the list was in reverse order then `n-1` passes would be required to sort the list, and for each pass (depending on implementation) `n-1` or `n-i` comparisons would be needed. This leads to a final complexity function with a highest term of `n^2` so the complexity is `O(n^2)`.
# 5.4
> Implement bubble sort (see previous exercise) using OCaml

```ocaml
let rec bubble_pass = function 
  | [] -> []
  | [x] -> [x]
  | x1::x2::tl -> 
    if x1 < x2 then
      x1 :: bubble_pass (x2 :: tl)
    else
      x2 :: bubble_pass (x1 :: tl)

let rec bubble_sort xs = 
  if xs = bubble_pass xs then
    xs
  else
    bubble_sort (bubble_pass xs)
```

---
# 6.1
> Give the declaration of an OCaml type for the days of the week. Comment on the practicality of such a type in a calendar application.

```ocaml
type days_of_week = 
	| Monday
	| Tuesday
	| Wednesday
	| Thursday
	| Friday
	| Saturday
	| Sunday
```

I think this is a practical type for a calendar application. Using an enumeration type for days of the week avoids potential errors that could occur when using strings or numbers to represent days, and is suitable because there are a finite number of distinct options.
It also abstracts the underlying data away from the interface - the application could use the enumeration type to provide multiple different interfaces, such functions that pattern match over the enum to return strings in e.g. short versions (`"mon"`, `"tue"`, `"sun"` etc.) or long versions (`"wednesday"`, `"friday"`, `"saturday"` etc.).
However, the type does not initially present certain operations such as "moving between" adjacent days that would be more simple to implement using other representations (integers, for example). This could be fixed just by ensuring that you implement additional functions that operate on the enum type, as long as these present clean and simple interfaces to other code or module trying to use the data type.
# 6.2
> Write an OCaml function taking a binary tree labelled with integers and returning their sum.

```ocaml
let rec sum_tree = function
  | Lf -> 0
  | Br (v, tr1, tr2) -> v + (sum_tree tr1) + (sum_tree tr2)
```
# 6.3
> Examine the following function declaration. What does `ftree (1, n)` accomplish? ![[Pasted image 20231018165426.png]]

`ftree (1, n)` would error because the function expects two parameters, not a tuple pair.
`ftree 1 n`, however, creates a binary tree with `n` levels of branch nodes. The top node will always be one, and subsequent nodes will contain the integers from `1` to `2^n - 1` in order in a *breadth-first* manner. Changing the value of `k` will simply shift 
# 6.4
> Give the declaration of an OCaml type for arithmetic expressions that have the following possibilities: floating-point numbers, variables (represented by strings), or expressions of the form −𝐸, 𝐸 + 𝐸, 𝐸 × 𝐸.

```ocaml
type expression = 
  | Number of float
  | Variable of string
  | Negate of expression
  | Addition of expression * expression
  | Multiplication of expression * expression
```
# 6.5
> Continuing the previous exercise, write a function that evaluates an expression. If the expression contains any variables, your function should raise an exception indicating the variable name.

```ocaml
let rec evaluate_expression = function
  | Number x -> x
  | Variable _ -> failwith "undefined identifier!"
  | Negate x -> -. (evaluate_expression x)
  | Addition (x, y) -> (evaluate_expression x) +. (evaluate_expression y)
  | Multiplication (x, y) -> (evaluate_expression x) *. (evaluate_expression y)
```

---
# Exam Question 1
> Give an example of an ML function belonging to each of the following complexity classes: 
> (a) O(1)
> (b) O(n)
> (c) O(n log n)
> (d) O(n^2)
> (e) O(2^n)
> 
> Each answer may contain code fragments (involving well-known functions) rather than self-contained programs, but must include justification. (The upper bound in each case should be reasonably tight.)

a)
```ocaml
let o_1 n = 1
```
The function `o_1` has a constant time complexity of `O(1)` because the time taken is the same regardless of the size of input `n` - it just returns a number.

b)
```ocaml
let rec o_n n = 
	if n = 0 then
		0
	else 
		1 + o_n (n - 1)
```
The function `o_n` has a linear time complexity of `O(n)` because it counts down from `n`, adding `1` to a sum each time. As `n` increases by one, the function performs one more step (addition of 1), so time complexity is linear.

c)
```ocaml
let rec o_n_log_n = function

```
? unsure why a particular function would have `O(n log n)`

d)
```ocaml
let rec o_n_2 n = 
	let rec o_n n = 
		if n = 0 then
			0
		else 
			1 + o_n (n - 1)
	in
	if n = 0 then
		0
	else 
		o_n (n) + o_n_2 (n - 1)
```
The function `o_n_2` has a quadratic time complexity of `O(n^2)` because as it counts down from `n`, it performs the same function from b) which has a linear time complexity. As `n` increases by one, the function performs `n` more steps, so time complexity is quadratic.

e)
```ocaml
let rec o_2_n n = 
	if n = 0 then
		0
	else
		o_2_n (n - 1) + O_2_n (n - 1)
```
The function `o_2_n` has an exponential time complexity of `O(2^n)` because as it counts down from `n`, it performs itself twice. As `n` increases by one, the function performs twice the steps it would with input `n`, so time complexity is exponential as `2 * 2^(n - 1) = 2^n`.

---
# Exam Question 2
> Define an ML function rotations that will compute the list of all rotations of a given list. For example `rotations [1,2,3] = [[1,2,3],[2,3,1],[3,1,2]]`
> The order in which the rotations occur is unimportant.
> 
> Carefully explain how your function works and estimate the time complexity of your solution.

a)
```ocaml
let next_rotation = function
	| [] -> []
	| hd::tl -> 
		let rec remove_last_item acc = function
			| [] -> failwith "no last item"
			| hd::[] -> (hd, acc)
			| hd::tl -> remove_last_item (acc @ [hd]) tl
		in
		let (x, xs) = remove_last_item [] (hd::tl) 
		in
		x :: xs

let rotations orig_n = 
	let rec create_rotations acc curr_n = 
		let next_n = next_rotation (curr_n)
		in
		if next_n = orig_n then
			curr_n :: acc
		else
			create_rotations (curr_n :: acc) next_n
	  in
	  create_rotations [] orig_n	
```

b)
My function uses two processes. The `next_rotation` function takes in a list and calculates the next rightward rotation of the list. It does this by traversing the list to find the last item, removing this item from the list then adding it to the head of the list.
The `rotations` function uses the original list and the `next_rotation` function to create a list of all possible rotations. For the current rotation, it calculates the next rotation. If this rotation is the same as the original list, then all rotations have been calculated and it can just add the current rotation to the accumulated rotations and return that.
If the next rotation is not the same as the original list, then it adds the current rotation to the accumulated rotation and calls the `create_rotations` function again with the updated accumulator and the next rotation, until all rotations have been added.

The time complexity of this function is `O(n^2)`. The `next_rotation` function has a linear time complexity, because it just traverses through the list once to find the last item, then adds this to the start. For a list of size `n`, `n + 1` steps are needed, so it has a time complexity of `O(n)`.
The `rotations` function performs the `next_rotation` function `n` times, until the list has been fully rotated into its original form. This means that for a list of size `n`, it performs `n` steps.
Therefore the time complexity of my `rotations` function is `O(n^2)`.

---
# Exam Question 3
> Code the function `least(n,xs)`, which returns the least `n` elements of the list `xs` of real numbers. The result does not have to be sorted. You may assume `n <= length(xs)`.

My solution is more similar to selection sort than quicksort?
```ocaml
let rec remove_smallest = function
  | [] -> failwith "invalid!"
  | hd::tl -> 
    let rec traverse curr out = function
      | [] -> (curr, out)
      | hd::tl -> 
        if hd < curr then
          traverse hd (curr :: out) tl
        else
          traverse curr (hd :: out) tl
    in traverse hd [] tl 

let least n xs =
  let rec least_inner acc xs n =
    if n = 0 then
      acc
    else
      let (l, xs) = remove_smallest xs
      in
      least_inner (l :: acc) (xs) (n - 1)
  in least_inner [] xs n
```

---
# Notes
### Chapter 3
- Lists can be used to perform very sophisticated tasks, binary arithmetic or matrix operations etc.
- Backtracking means responding to failure, going back to previous decision and trying other choice
- OCaml warns against inexhaustive pattern matches
- Structures can be used to group related labelled properties and functions under one distinct name
- Important to use let bindings/take computations out of recursion when you can, improves efficiency/less wasted memory
- Equality Type - a type whose values are able to be tested for equalty. Forbidden on functions and abstract types:
	- defined for integers, reals, characters, bools, tuples, lists, records
	- functions type contains equality type variables if it performs polymorphic equality
	- e.g. can be used to implement comparisons of homemade data types such as union of sets
- Many differnet sorting algorithms with differnet complexities, advantages, drawbacks.
- Insertion sort inserts items one at a time into their sorted place