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
or
![[Pasted image 20231106101855.png]]

To prove an implication of the form `if P then Q`, assume that `P` is true and prove `Q`.
![[Pasted image 20231106104539.png]]

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
### Bi-Implication
`P` is equivalent to `Q`
or
`P` implies `Q` and vice versa,
`Q` implies `P` and vice versa
or
`P` if and only if, `Q`.
or
![[Pasted image 20231106101833.png]]

To prove, prove that `P` implies `Q` AND `Q` implies `P`.
![[Pasted image 20231106104558.png]]

**Example 1**:
> For every integer `n`,
> 1. `n` is even if, and only if `n = 0 (mod 2)`.

##### 1 -> 2
Assumptions:
1. `n` is even
Goal:
- `n - 0` is a multiple of `2`.

By *1*, `n = 2i`, hence

`n - 0` is a multiple of `2`.
##### 2 -> 1
Assumptions:
1. `n = 0 (mod 2)
Goal:
- `n` is even

By *1*, `n - 0` is a multiple of `2`
`n - 0 = 2i` for some integer `i`
`n = 2i`, hence

`n` is even.
### Universal Quantifications
**for all** individuals `x` in the universe of discourse, the property `P(x)` holds.
or
no matter what individual `x` in the universe of discourse one considers, the property `P(x)` for it holds.
or
![[Pasted image 20231106101910.png]]

To prove, let `x` stand for an arbitrary individual (ensure an unused variable!) and prove `P(x)`.
![[Pasted image 20231106104621.png]]

To use, plug in any value `y` and conclude that `P(y)` is true and further assume it. This is called **universal instantiation**.

**Example 1**:
> Fix a positive integer `m`. For integers `a` and `b`, we have that `a = b (mod m)` if and only if for all positive integers `n`, we have that `n * a = n * b (mod n * m)
##### 1 -> 2
Assumptions:
1. `a = b (mod m)
Goals:
- for all positive integers `n`, `n * a = n * b (mod n * m)`

Let `x` be an arbitrary positive integer.
Assumptions:
2. 
Goals:
- `x * a = x * b (mod x * m)
### Equalities in Proofs
if `a = b` and `b = c` then `a = c` 
if `a = b` and `x = y` then `a + x = b + x = b + y`

Side note - axioms for equality:
- for every individual `x`, `x = x`
- for any pair of equal individuals, if a property holds for one then it holds for the other.
### Conjunctions
`P` and `Q`
or
both `P` and also `Q` hold
or
![[Pasted image 20231106104434.png]]

To prove, first prove `P` and then prove `Q` or vice versa.
![[Pasted image 20231106104648.png]]

**Example 1**: 
> For every integer `n`, we have that `6 | n` iff `2 | n` and `3 | n`

Assumptions:
1. `n` is an arbitrary integer.
RTP:
-  `6 | n` implies (`2 | n` and `3 | n`)
- (`2 | n` and `3 | n`) implies `6 | n`
##### 1 -> 2
Assumptions:
1. `6 | n`
RTP:
- `2 | n`
- `3 | n`
##### 1
RTP:
- `2 | n`

By *1*, `n = 6k` for an integer `k`.
`n = 2 (3k)` 
`n = 2i` for an integer `i`, hence

`2 | n`
##### 2
RTP:
- `3 | n`

By *1*, `n = 6k` for an integer `k`.
`n = 3 (2k)
`n = 3i` for an integer `i`, hence

`3 | n`.

hence
`6 | n` implies `2 | n` and `3 | n`
##### 2 -> 1
Assumptions:
1. `2 | n`
2. `3 | n`
RTP:
- `6 | n`

By *1*, `n = 2i` for an integer `i`.
By *2*, `n = 3j` for an integer `j`.
`3n = 6i`
`2n = 6j`
`3n - 2n = n = 6i - 6j = 6(i - j)`
`n = 6(i - j)`, hence

`6 | n`.

hence
`2 | n` and `3 | n` implies `6 | n`.

hence
`6 | n` iff `2 | n` and `3 | n`.
### Existential Quantifications.
**there exists** an individual `x` in the universe of discourse for which the property `P(x)` holds
or
**for some** individual `x`, the property `P(x)` holds.
or
![[Pasted image 20231108100758.png]]

**Example Theorem**:
Let `f` be a real-valued continuous function on an interval `[a, b]`. For every `y` in between `f(a)` and `f(b)`, there exists a value `v` between `a` and `b` such that `f(v) = y`.

To prove, find a witness `w`, (value of `x` you think it will be true for), and show that `P(w)` is true for that value.
![[Pasted image 20231108101914.png]]

To use, introduce a new variable `x0` into the proof to stand for some individual for which the property `P(x)` holds. You can then assume `P(x0)` true.

**Example 1**:
> For every positive integer `k`, there exist natural numbers `i` and `j` such that `4k = i^2 - j^2`.

Let `k` be an arbitrary positive integer.
(scratch work to find `i` and `j` that work)
Let `i0 = k + 1` (witness value)
Let `j0 = k - 1` (witness value)

`i0^2 - j0^2 = (k^2 + 2k + 1) - (k^2 -2k + 1)
`= 2k - -2k`
`= 4k`

**Example 2**:
> For all integers `l`, `m`, `n`, if `l | m` and `m | n` then `l | n`.

Let `l`, `m`, `n` be arbitrary integers.
`m = i * l`
`n = j * m`

**Example 2**:
> For every positive integer `n`, there exists a natural number `l` such that `2^l < n < 2^(l+1)`.

Let `n` be an arbitrary positive integer.
`m
### Unique Existence
the **unique existence** of an `x` for which the property `P(x)` holds.
or
![[Pasted image 20231108102538.png]]

To prove, prove existence AND that if two numbers are true then they are the same.![[Pasted image 20231108102650.png]]
### Disjunctions
**P** or **Q**
or
![[Pasted image 20231108104004.png]]

To prove, either prove O or prove Q or prove that either P or Q depending on the case.
![[Pasted image 20231108104113.png]]

To use, assume `P1` true and prove goal `Q`, then assume `P2` true and prove goal `Q`.

**Example 1**: 
> For all positive integers `p` and natural numbers `n`, if `m = 0` or `m = p` then `(p, m) congr 1 (mod p)`.

Let `p` be an arbitrary positive integer and `m` an arbitrary natural number.
Assume `m = 0` or `m = p`.
RTP: `p!/(m!(p - m)!) = 1 (mod p)

\1. `m = 0`
`(p, 0) = p!/(0!(p - 0)!) = p!/p! = 1 -> 1 congr 1 (mod p)

\2. `m = p`
`(p, 0) = p!(p!(p! - p!) = p!/p! = 1 -> 1 congr 1 (mod p)`
### Binomial Formula
![[Pasted image 20231110101827.png]]

**Example 1**:
> For all natural numbers `m`, `n` and primes `p`, `(m + n)^p congr m^p + n^p (mod p)`

Let `m` and `n` be arbitrary natural numbers and `p` be an arbitrary prime.

![[Pasted image 20231110102614.png]]

![[Pasted image 20231110103807.png]]
^ This tells you that for every number that is not congruent to `0 (mod p)` has a **reciprocal**.
![[Pasted image 20231110104209.png]]
### Negation
**not** `P`
or
`P` is not the case
or
![[Pasted image 20231110104350.png]]

To prove, first try writing the negation in an equivalent form.
![[Pasted image 20231110104406.png]]

**Example 1 (contrapositive)**:
> For all statements `P` and `Q`, if `P` implies `Q` then `not Q` implies `not P`.

Let `P` and `Q` be statements.

2. Assume that `P` implies `Q`.
3. Assume `not Q`, `Q implies false`
RTP: `not P`, `P implies false`

1. Assume `P`
RTP: `false`

By *1* and *2*, we have `Q`. 
By *3*, we have `false`.
### Proof by Contradiction
![[Pasted image 20231110105032.png]]
is classically accepted. 
Hence to prove `P`, one can prove `not P implies false`, or that assuming `not P` leads to contradiction.

**Example 2**:
> For all statements `P` and `Q`, if `not Q` implies `not P` then `P` implies `Q`.

Assume `not Q` implies `not P`
Assume `P`
RTP: `Q`.

Proceed by contradiction:
Assume `not Q` (negation of goal)
RTP: `false`.

`not Q` gives `not P`
which contradicts `P`

hence `Q`.