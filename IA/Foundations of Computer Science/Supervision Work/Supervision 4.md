**Name:** Brace Godfrey
**CRSId:** dg681

### 10.1
> Suppose that we have an implementation of queues, based on binary trees, such that each operation takes logarithmic time in the worst case. Outline the advantages and drawbacks of such an implementation compared with one presented above.

### 10.2
> The traditional way to implement queues uses a fixed-length array. Two indices into the array indicate the start and end of the queue, which wraps around from the end of the array to the start. How appropriate is such a data structure for implementing breadth-first search?

### 10.3
> Write a version of the function `breadth` using a nested `let` construction rather than `match`.

### 10.4
> Iterative deepening is inappropriate if 𝑏 ≈ 1, where 𝑏 is the branching factor. What search strategy is appropriate in this case?

### 10.5
> If we regard this function as representing a tree, where the subtrees are computed from the current label, what tree does next 1 represent?
> ![[Pasted image 20231102091510.png]]

---

### 11.1
> Comment, with examples, on the differences between an `int ref list` and an `int list ref`.

### 11.2
> Write a version of function `power` using `while` instead of recursion.

### 11.4
> What is the effect of `while C1; B do C2 done`?

---

### 2003 Paper 1 Question 6
> a) Explain in detail how a binary search tree represents a dictionary. If reference types are not used, what is the average-case cost of an update operation?



> b) A *mutable* binary tree is either a leaf or a branch node containing a label and references to two other mutable binary trees. Present the ML datatype declaration for mutable binary trees.



> c) Describe how to use this datatype to implement (mutable) binary search trees. Give ML code for the lookup and update operations. The update operation must never copy existing tree nodes.

---

### 2007 Paper 1 Question 6
> a) Write brief notes on reference types in ML and on control structures for imperative programming.



> b) Write a function that is equivalent to snacker below but makes no use of references. Briefly explain why the two functions are equivalent. ![[Pasted image 20231102091857.png]] ![[Pasted image 20231102091906.png]]



> c) Write a function `gluttony` such that `gluttony m1 m2` makes a copy of `m1`, replacing every `Snack` node with `m2`.



> d) Write a function `glut` such that `glut k m1 m2` makes a copy of `m1`, replacing the kth `Snack` node with `m2`. Nodes are counted from left to right, with the leftmost node being number one. 



---

# Notes
### Chapter 12

### Chapter 13

### Chapter 14

### Pages
