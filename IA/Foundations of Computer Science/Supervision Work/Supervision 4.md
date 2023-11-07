**Name:** Brace Godfrey
**CRSId:** dg681
### 10.1
> Suppose that we have an implementation of queues, based on binary trees, such that each operation takes logarithmic time in the worst case. Outline the advantages and drawbacks of such an implementation compared with one presented above.

- The above implementation of the queue has an amortized time complexity of `O(1)` for enqueue and dequeue operations, whereas the binary tree implementation has a less efficient complexity of  `O(log n)`.
- The binary tree method may be more commonly used and easier to write operations for.
### 10.2
> The traditional way to implement queues uses a fixed-length array. Two indices into the array indicate the start and end of the queue, which wraps around from the end of the array to the start. How appropriate is such a data structure for implementing breadth-first search?

This structure may not be appropriate for implementing breadth-first search as there may be more items in the tree that need to be searched than there is space in the array. The number of items in a binary tree is `2^d` where `d` is the depth of the tree, and unless this value is known before creating the queue the data structure is not ideal to use for implementing BFS.
### 10.3
> Write a version of the function `breadth` using a nested `let` construction rather than `match`.

```ocaml
let rec breadth q =
	if qnull q then []
	else
		let qu = qhd q in
		if qu = Lf then
			breadth (deq q)
		else
			let Br (v, t, u) = qu in
			v :: breadth (enq (enq (deq q) t) u)
```
### 10.4
> Iterative deepening is inappropriate if 𝑏 ≈ 1, where 𝑏 is the branching factor. What search strategy is appropriate in this case?

Linear search, as there will only be one connection from the current node to a node in the next level. Each node should be visited and then its singular child visited recursively until the item is found.
### 10.5
> If we regard this function as representing a tree, where the subtrees are computed from the current label, what tree does next 1 represent?
> ![[Pasted image 20231102091510.png]]

`next 1` represents the tree:
	1
   /   \
 2    3
  
---
### 11.1
> Comment, with examples, on the differences between an `int ref list` and an `int list ref`.

`int ref list` is a list of references to integers. For example
```ocaml
let a = ref 1;;
let b = ref 2;;
let c = ref 3;;

let int_ref_list = [a; b; c];;
(* val int_ref_list : int ref list *)
```

`int list ref` is a reference to a list of integers. For example,
```ocaml
let a = [1; 2; 4]

let int_list_ref = ref a;
(* val int_list_ref : int list ref *)
```
### 11.2
> Write a version of function `power` using `while` instead of recursion.

```ocaml
let power p n =
	let acc = ref p in
	let count = ref n in
	while !count > 1 do
		acc := !acc * p;
		count := !count - 1
	done;
	!acc
```
### 11.3
> What is the effect of `while C1; B do C2 done`?

`C1; B` is evaluated as a boolean condition in the `while` construct, such that the command `C1` will be executed once and the value `B` will be used to determine whether the `while` loop should continue running. If `B` evaluates to true, the `C2` command will be run once and then the `C1; B` command chain will be run again.
If `C1` or `C2` affects `B` such that it eventually evaluates to `false`, the `while` loop will stop running. If they never do, it will run forever.
### 11.4
> Write a function to exchange the values of two references, `xr` and `yr`.

```ocaml
let swap_refs xr yr = 
	let (xrv, yrv) = (!xr, !yr) in
	xr := yrv;
	yr := xrv;
```
---
### 2003 Paper 1 Question 6
> a) Explain in detail how a binary search tree represents a dictionary. If reference types are not used, what is the average-case cost of an update operation?

A dictionary is formed of key-value pairs, often as a tuple. A binary search tree can be used to represent a dictionary as long as the keys are orderable and can be compared on. Then a binary search tree can be formed such that the keys of dictionary are in order in the search tree. 
Dictionary items can be looked up using the normal lookup operation of the search tree, using the given key, and the when the correct node is found the value extracted. This has a time complexity of `O(log n)`.
An insert operation can be done by performing the lookup operation until a leaf node is found, and then placing the new item either on the left or right depending on the value of the key above. This has a average cost of `O(log n)`.

> b) A *mutable* binary tree is either a leaf or a branch node containing a label and references to two other mutable binary trees. Present the ML datatype declaration for mutable binary trees.

```ocaml
type 'a 'b mut_bin_tree = 
	| Lf
	| Br of ('a ref * 'b ref) * 'a mut_bin_tree ref * 'a mut_bin_tree ref
```

> c) Describe how to use this datatype to implement (mutable) binary search trees. Give ML code for the lookup and update operations. The update operation must never copy existing tree nodes.

This data type could be used to implement binary search trees in the same manner that normal binary search trees can be implemented without mutability. For mutable binary trees, the lookup and update operations would be similar because once the node is found, the reference label can immediately be updated to the new value.

I am unsure what exactly a `lookup` function would return? Is the mutable binary tree meant to be have key-value pairs?

```ocaml
let rec update tree search_label new_v = 
	match tree with
		| Lf -> failwith "not found!"
		| Br((k, v), lt, rt) -> 
			if !k = search_label then
				v := new_v;
			else
				if search_label < !label then
					update !lt search_label new_v
				else 
					update !rt search_label new_v
```

---
### 2007 Paper 1 Question 6
> a) Write brief notes on reference types in ML and on control structures for imperative programming.

- Reference types are used to hold mutable values in OCaml that can change over the course of the program.
- A reference can be instantiated with the `ref` keyword, such as `let a = ref 1`, and references have the type `'a ref` (`a` would have the type: `val a : int ref`).
- Internally all references have a `contents` field used to access the current value. The current value of a reference can be accessed with the `!` symbol, which will return a value of type `'a` of the reference.
- References can be updated using the `:=` command.

- imperative programming can be performed in OCaml using command chains.
- The chain of commands `C1;C2;C3;...Cn` will execute each command in order, and return the value of `Cn`. 
- OCaml has a `while E do B done;` command, which will execute the body `B` as long as the expression `E` remains `true` - after each body execution this expression will be re-evaluated.
- (unsure if there are any other important control structures)

> b) Write a function that is equivalent to snacker below but makes no use of references. Briefly explain why the two functions are equivalent. ![[Pasted image 20231102091857.png]] ![[Pasted image 20231102091906.png]]

```ocaml
let snacker_no_ref m = 
	let rec inner m acc = 
		match m with
			| Snack(x) -> x :: acc
			| Lunch(m1, m2) -> (inner m2 acc) @ (inner m1 acc)
			| Feast(m1, m2, m3) -> (inner m3 acc) @ (inner m2 acc) @ (inner m1 acc) 
	in inner m []
```

These functions are equivalent as they both take a `meal` type and add all of the meal data to a list, recursively going through levels that contain other meals. The `snacker` function does this by adding each item to a reference to a list of meals, whereas the `snack_no_ref` does this with an accumulator value that is built up from the bottom of the meal tree.

> c) Write a function `gluttony` such that `gluttony m1 m2` makes a copy of `m1`, replacing every `Snack` node with `m2`.

```ocaml
let rec gluttony main replace = 
	match main with 
		| Snack(_) -> 
			replace
		| Lunch(m1, m2) -> 
			Lunch(
				gluttony m1 replace, 
				gluttony m2 replace
			)
		| Feast(m1, m2, m3) ->
			Feast(
				gluttony m1 replace, 
				gluttony m2 replace,
				gluttony m3 replace
			)
```

> d) Write a function `glut` such that `glut k m1 m2` makes a copy of `m1`, replacing the kth `Snack` node with `m2`. Nodes are counted from left to right, with the leftmost node being number one. 

```ocaml
let glut k main replace = 
	let count = ref 0 in
	let rec inner main replace = 
		match main with 
			| Snack(m1) -> 
				count := !count + 1;
				if !count = k then
					replace
				else
					Snack(m1)
			| Lunch(m1, m2) -> 
				Lunch(
					inner m1 replace, 
					inner m2 replace
				)
			| Feast(m1, m2, m3) ->
				Feast(
					inner m1 replace, 
					inner m2 replace,
					inner m3 replace
				)
	in inner main replace
```