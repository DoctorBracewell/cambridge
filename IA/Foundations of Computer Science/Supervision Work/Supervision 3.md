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

### 7.5
> Code the delete function outlined in the previous exercise.

```ocaml
let delete k = function
	| Lf -> failwith "item does not exist in tree!"
	| 
```
### 7.6
> Show that the functions `preorder`, `inorder` and `postorder` all require `O(n^2)` time in the worst case, where `n` is the size of the tree.

### 7.7
> Show that the functions `preord`, `inord` and `postord` all take linear time in the size of the tree.

### 7.8
> Write a function to remove the first element from a functional array. All the other elements are to have their subscripts reduced by one. The cost of this operation should be linear in the size of the array.

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
	| Cons(x, xf) -> 
		
```
### 9.3
> Code a function to make change using lazy lists, delivering the sequence of all possible ways of making change. Using sequences allows us to compute solutions one at a time when there exists an astronomical number. Represent lists of coins using ordinary lists. (Hint: to benefit from laziness you may need to pass around the sequence of alternative solutions as a function of type `unit -> (int list) seq`.)

### 9.4
> A *lazy binary tree* is either empty or is a branch containing a label and two lazy binary trees, possibly to infinite depth. Present an OCaml datatype to represent lazy binary trees, along with a function that accepts a lazy binary tree and produces a lazy list that contains all of the tree’s labels.

---

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