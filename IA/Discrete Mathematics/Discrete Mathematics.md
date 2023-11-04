### Introduction
**Point**: 
- Learn to read, write and work with mathematical proofs
- Learn basic discrete mathematics
- Learn applications of this to computer science.

### Proofs
**Proofs in practice**:
- E.g., "The product of two odd numbers is odd"
	- Need to understand what a statement is
	- Know what an integer (number) is
	- Understand product, odd, etc. etc. etc.
- Could be written more formally, such as "If `m` and `n` are odd integers, then so is `m * n`".

**Statement** - A sentence that is either true or false but not both. Can be given in natural language or in mathematical formula.

**Predicate** - A statement whose truth depends on one or more variables.
![[Pasted image 20231104162901.png]]

**Theorem** - A very important true statement
**Proposition** - A less important but still interesting true statement.
**Lemma** - A true statement used in proving other statements.
**Corollary** - A true statement that is a simple deduction from a theorem or proposition.

**Conjecture** - A statement that is believed to be true but not yet proved.

**Proof** - A logical explanation of why a statement is true.
**Logic** - The study of methods and principles used to distinguish correct proofs from indirect proofs. There are multiple logics, e.g. classical predicate, hoare, temporal.
**Axiom** - A basic assumption about a mathematical situation. These can be considered facts that do not need to be proved.
**Definition** - An explanation of the mathematical meaning of a word or phrase.

**Simple/Atomic** - A statement that cannot be broken down into other statements.
**Composite** - A statement built using several other statements connected by *logical expression*.

**Assumptions** - Statements that may be used for deduction.
**Goals** - Statements to be established or proved.
### Implication
**If** a collection of *assumptions* holds **then** so does some *conclusion*.
or
a collection of *assumptions* **implies** some *conclusion*.

To prove an implication of the form `if P then Q`, assume that `P` is true and prove `Q`.

**Example 1**:
> If `n` and `m` are odd integers then so is `m * n`.

Assumptions:
1. `m` is an odd integer
2. `n` is an odd integer
Goal (required to prove):
- `m * n` is an odd integer

By *1*, `m` is of the form `m = 2i + 1` for some integer `i`.
By *2*, `n` is of the form `n = 2j + 1` for some integer `j`.

Therefore
`m * n = (2i + 1)*(2j + 1)
`m * n = 4ij + 2i + 2j + 1`
`m * n = 2 (2 (ij + i + j) ) + 1` 
is of the form `2k + 1` where `k` is an integer, hence 

`m * n` is an odd integer.

**Example 2**:
> Let `x` be a positive number. If `sqrt(x)` is rational then so is `x`.

Assumptions:
1. `sqrt(x)` is rational.
Goal:
- `x` is rational

By *1*, `sqrt(x)` is of the form `m / n` for some integers `m` and `n`.

Therefore
`sqrt(x)^2 = x`
`sqrt(x)^2 = (m/n)^2 = m^2 / n^2
`x = m^2 / n^2
is of the form `x / y` where `x` and `y` are integers, hence

`x` is rational.

**Modus Ponens**:
- From the statements `P` and `P implies Q`, the statement `Q` follows.
- Or, if `P` and `P implies Q` hold then so does `Q`.

**Example 3**:
> Let `P1`, `P2`, `P3` be statements. If `P1` implies `P2`, and `P2` implies `P3`, then `P1` implies `P3`.

Assumptions:
1. `P1` implies `P2`.
2. `P2` implies `P3`.
Goal:
- `P1` implies `P3`.

	Assumptions:
	3. `P1`
	Goal:
	- `P3`
	
	By *1* and *3*, we have `P2`.
	By *2*, we have `P3`.

Assuming `P1` gives us `P3`, hence

`P1` implies `P3`.







