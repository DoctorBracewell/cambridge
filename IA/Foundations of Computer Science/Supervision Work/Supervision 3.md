**Name:** Brace Godfrey
**CRSId:** dg681
### 7.1
> Draw the binary search tree that arises from successively inserting the following pairs into the empty tree: `("Alice", 6), ("Tobias", 2), ("Gerald", 8), ("Lucy", 9)`. Then repeat this task using the order `("Gerald", 8), ("Alice", 6), ("Lucy", 9), ("Tobias", 2)`. Why are results different?

1. ![[Pasted image 20231027162208.png]]
2. ![[Pasted image 20231027162155.png]]
The two trees are different depending on the input order. The `update` function does not automatically balance the binary trees, and the first input to an empty tree will always become the root node. Even though both input orders lead to a correct binary search tree, the trees look different.
### 7.2
> Code an insertion function for binary search trees. It should resemble the existing `update` function except that it should raise the exception `Collision` if the item to be inserted is already present.

```ocaml
exception Collision

let rec insert k v = function
	| Lf -> Br((k, v), Lf, Lf)
	| Br((a, x), lt, rt) -> 
		if k < a then
			Br((a, x), (insert k v lt), rt)
		else if k > a then
			Br((a, x), lt, (insert k v rt))
		else 
			raise Collision
	
```
### 7.3
> Continuing the previous exercise, it would be natural for exceptional Collision to return the value previously stored in the dictionary. Why is that goal difficult to achieve?

This would be difficult because the function has no information about the type of the values stored in the tree. OCaml cannot define exceptions that hold data of a polymorphic type, so there is no way to "return" the data value out of the `insert` function if it encounters a collision.
### 7.4
> Describe an algorithm for deleting an entry from a binary search tree. Comment on the suitability of your approach.

The algorithm should take the key of the item it wants to delete. It should then search the tree to find the item (and either simply return or throw an error if it cannot find it). If the item to be deleted only has leaf nodes as children, it can be replaced with a leaf node. If it has just one child sub-tree, it can be replaced with that sub-tree. If it has two sub-trees, the first item of the left sub-tree can be set as the node, and then the remaining sub-trees combined somehow? Unsure how this function should be finished. 
### 7.5
> Code the delete function outlined in the previous exercise.

```ocaml
let delete k = function
	| Lf -> failwith "item does not exist in tree!"
	| Br((key, v) lt, rt) -> 
		if k < key then
			Br((key, v), delete k lt, rt)
		else if k > key then
			Br((key, v), lt, delete k rt)
		else (* k = key *)
			match (lt, rt) with
				| (Lf, Lf) -> Lf
				| (lt, Lf) -> lt
				| (Lf, rt) -> rt
				| (lt, rt) -> ??? (* unsure how to finish *)
```
### 7.6
> Show that the functions `preorder`, `inorder` and `postorder` all require `O(n^2)` time in the worst case, where `n` is the size of the tree.
> ![[Pasted image 20231028225721.png]]

`@` operator has complexity `O(n)` and closed-form recurrence relation: `T(n) = n 

`preorder`, `inorder` and `postorder` all have recurrence relation of the form:
`T(n) = 1 + (n - 1) + (n - 1) + T(n - 1) + T(n - 1)` -> `T(n) = `O(n^2)` 
### 7.7
> Show that the functions `preord`, `inord` and `postord` all take linear time in the size of the tree. ![[Pasted image 20231028230721.png]]
> 

`preord`, `inord`, `postord` all have recurrence relation `T(n) = 1 + T(n - 1) + T(n - 1)` -> `O(n)
### 7.8
> Write a function to remove the first element from a functional array. All the other elements are to have their subscripts reduced by one. The cost of this operation should be linear in the size of the array.

```ocaml
let rec pop = function
	| Lf -> failwith "no first item!"
	| Br(v, Lf, Lf) -> Lf
	| Br(_, Br(v, t1, t2), rt) -> Br(v, rt, pop Br(v, t2, t1))
```

---
### 8.1
> What does the following function do, and what are its uses? ![[Pasted image 20231025110345.png]]

The `sw` function takes in a function and two parameters, and returns the function with the parameters applied in reverse order. This may be useful for functions where the ordering of the parameters matters, but I am unsure of examples.

---
### 9.1
> Code an analogue of map for sequences.

```ocaml
let rec map f = function
| Nil -> Nil
| Cons(x, xf) ->
    let xs = xf () in
    Cons(f x, fun () -> map f xs);;
```
### 9.2
> Consider the list function concat, which concatenates a list of lists to form a single list. Can it be generalised to concatenate a sequence of sequences? What can go wrong? ![[Pasted image 20231025110433.png]]

```ocaml
let rec concat = function
	| Nil -> Nil
	| Cons(xs, xsf) -> 
		let rec traverse_seq = function
			| Nil -> concat (xsf ())
			| Cons(x, xf) -> 
				Cons(x, fun () -> traverse_seq (xf ())) 
		in traverse_seq xs 
```
Whilst this function will concatenate a sequence of sequences, if the first sequence is infinite no items from the second sequence will ever be calculated (because `s1 + s2 = s1` when `s1` is infinite).
### 9.3
> Code a function to make change using lazy lists, delivering the sequence of all possible ways of making change. Using sequences allows us to compute solutions one at a time when there exists an astronomical number. Represent lists of coins using ordinary lists. (Hint: to benefit from laziness you may need to pass around the sequence of alternative solutions as a function of type `unit -> (int list) seq`.)

### 9.4
> A *lazy binary tree* is either empty or is a branch containing a label and two lazy binary trees, possibly to infinite depth. Present an OCaml datatype to represent lazy binary trees, along with a function that accepts a lazy binary tree and produces a lazy list that contains all of the tree’s labels.

```ocaml
type 'a lazy_bin_tree = 
	| Lf
	| Br of 'a * (unit -> 'a lazy_bin_tree) * (unit -> 'a lazy_bin_tree)

let rec tree_to_list = function
	| Lf -> Nil
	| Br(label, lt, rt) -> 
		Cons(
			label, 
			fun () -> concat (
				Cons(
					tree_to_list (lt ()),
					fun () -> Cons(
						tree_to_list (rt ()), 
						fun () -> Nil
					)
				)
			)
		)
```

---
### 2002 Paper 1 Question 5
> a) Declare the function `flip`, which maps a tree to a mirror image of itself.

```ocaml
let rec flip = function
	| Wave(label, current_fans) ->
		let rec flip_fans = function
				| [] -> []
				| hd::tl -> flip (hd) :: flip_fans (tl)
		in
		Wave(label, List.rev (flip_fans current_fans))
```

> b) Declare the curried function `paint f`, which copies a tree while applying the function `f` to each of its labels.

```ocaml
let rec paint f = function
	| Wave(label, current_fans) ->
		let rec map_fans = function
			| [] -> []
			| hd :: tl -> paint (f) (hd) :: map_fans (tl)
		in Wave(f (label), map_fans (current_fans))
```

> c) Declare the function `same_shape`, which compares two trees and returns `true` if they are equal except for the values of their labels and otherwise returns `false`.

```ocaml
let rec same_shape = function
	| (Wave(_, fans1), Wave(_, fans2)) ->
		let rec compare_fans = function
			| ([], []) -> true
			| (_, []) -> false
			| ([], _) -> false
			| (_ :: tl1, _ :: tl2) -> compare_fans (tl1, tl2)
		in compare_fans (fans1, fans2)
```

> d) State the types of functions `flip`, `paint` and `same_shape`.

`val flip : 'a fan -> 'a fan`
`val paint : ('a -> 'b) 'a fan -> 'b fan`
`val same_shape : 'a fan -> 'b fan -> bool`

> e) Describe the computation that results when `paper` is applied to a tree.

When `paper` is applied to a tree, it will call `paper` for each sub-tree in the trees fan list and add one to the accumulator value. Since the function never uses the labels of each fan, the only computation performed is adding one to the accumulator for each label encountered by the function. This has the effect of calculating how many nodes are in the tree (from a starting value `q`, which could be `0`).
### 2003 Paper 1 Question 5
> a) Describe how lazy lists can be implemented using ML.

Lazy lists can be implemented using a enumeration type with a terminating pattern (`Nil`), and a continuation pattern (`Cons`) that holds the current value as well as an anonymous function that has a unit parameter and returns a either a continuation or a terminator. OCaml will not evaluate the functions from `Cons` patterns until necessary, so the datatype implements a lazy list. 

> b) Code a function to concatenate two lazy lists, by analogy to the ‘append’ function for ordinary ML lists. Describe what happens if your function is applied to a pair of infinite lists.

```ocaml
let rec concat = function
	| Nil -> Nil
	| Cons(xs, xsf) -> 
		let rec traverse_seq = function
			| Nil -> concat (xsf ())
			| Cons(x, xf) -> 
				Cons(x, fun () -> traverse_seq (xf ())) 
		in traverse_seq xs 

let concat_two l1 l2 = concat (Cons(l1, fun () -> Cons(l2, fun () -> Nil)))
```
If `concat_two` is applied to two infinite sequences any items from the second sequence will never appear, because the first sequence will always have another item and never have `Nil`.

> c) Code a function to combine two lazy lists, interleaving the elements of each.

```ocaml
let rec interleave xs ys = 
	match xs with 
		| Nil -> ys
		| Cons(x, xf) -> 
			match ys with
				| Nil -> Cons(x, xf)
				| Cons(y, yf) -> 
					let xs = xf () in
					let ys = yf () in
					Cons(x, fun () -> Cons(y, fun() -> interleave xs ys))
```

> d) Code the lazy list whose elements are all ordinary lists of zeroes and ones,

```ocaml
let generate_binary_numbers n = 
	let rec generate acc n = 
		match n with
			| 0 -> [acc] 
			| _ ->  
				generate (0 :: acc) (n - 1) @ 
				generate (1 :: acc) (n - 1) 
	in generate [] n
;;

let seq_zeroes_ones = 
	let rec nums_to_ll n = 
		let rec bins_to_ll = function
			| [] -> nums_to_ll (n + 1)
			| hd::tl -> Cons(hd, fun () -> bins_to_ll tl)
		in bins_to_ll (generate_binary_numbers n)
	in nums_to_ll 0
;;

seq_zeroes_ones
```

> e) Code the lazy list whose elements are all palindromes of 0s and 1s

```ocaml
let rec filter_seq pred = function
	| Nil -> Nil 
	| Cons (x, xf) -> 
		let xs = xf ()
		in 
		if pred x then 
			Cons (x, fun () -> filter_seq (pred) (xs))
		else 
			filter_seq (pred) (xs)
;;

let palin_filter xs =
	xs = List.rev xs
;;

let palins_zeroes_ones = 
	filter_seq (palin_filter) (seq_zeroes_ones)
;;
```

---
# Notes
### Chapter 5
- higher order function - operates on other function, e.g. can take them as parameters or return them.
- infinite lists can be implemented using functions as data, because the tail of the list is a function that only calculates the next item (and next tail) when it's called.
- functions don't need to have a name (anonymous)
- a function can only ever have one argument. Multiple arguments can be used:
	- by passing in a tuple
	- by a function that returns another function as a result
	- a function with a tuple has type `(a * b * c) -> z`, whereas a curried function has type `a -> b -> c -> z`
	- this means that functions can be *partially applied* - some parameters are "filled" with concrete values, returning the remaining to-be-curried functions and whichever parameters they take.
- Anonymous functions are useful to generalise certain other functions - for example, a sorting function might take a *predicate* function that takes two values and compares them. 
- Common examples include `map`, which takes a function and a list and applies that function to every item in the list
- or `filter`, which takes a predicate and removes any item in a list that doesn't pass the predicate.
- elements in a lazy list are not evaluated until necessary - they are infinite and a finite number of values can be computed.
- introduces more risk - e.g. infinite computation time because the program defines recursion with no base case.
- type for a *sequence* is:
```ocaml
type 'a seq =
	| Nil
	| Cons of 'a * (unit -> 'a seq)
```
- `Cons` pattern contains a tuple with the current item and a *function that evaluates the remaining sequence*.
- Depending on implementation, sequences are not always optional because some items "into the future" are calculated but may never be used.
- Lazy lists can have many helper functions such as interleaving, adding, mapping, filtering, etc, but all of these are still done lazily - just by updating the function held in `Cons`.
- Many practical applications, especially for e.g. recurrence-relation-like processes such as Newton-Raphson method, or sieve of eratosthenes.