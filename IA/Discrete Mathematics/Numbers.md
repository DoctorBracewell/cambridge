**Natural Numbers** - Generated from zero by successive increment.
Basic operations include addition and multiplication.

**Monoid** - An algebraic structure with a neutral element (`e`) and a binary operation (`•`), which satisfies:
- Neutral element laws: `e • x = x = x • e`
- Associativity law: `(x • y) • z = x • (y • z) = x • y • z`ative if
and a monoid is commutative if
- `x • y =  y • x`

**Semiring** - An algebraic structure with a commutative monoid `(0, ⊕)` and a monoid structure `(1, ⊗)` which satisfies distributivity laws:
![[Pasted image 20231113103844.png]]

**Additive Structure** - `(N, 0, +)` monoid for addition of the natural numbers.
**Multiplicative Structure** - `(N, 1, *)` monoid for multiplication of the natural numbers.

**Additive Cancellation** - `k + m = k + n` -> `m = n`
**Multiplicative Cancellation** - when `k != 0` then `k * m = k * n` -> `m = n`

A binary operation `•` allows cancellation by an element `c` if:
- on the left: if `c • x = c • y` implies `x = y`
- on the right: if `x • c = y • c` implies `x = y`

For a monoid with neutral element `e` and binary operation `•`, element `x` is said to admit an:
- inverse on the left if there exists an element `l` such that `l • x = e` (e.g `-8` for `8`)
- inverse on the right if there exists an element `r` such that `x • r = e` (e.g `1/6` for `6`)
- inverse if admits both left and right inverses.

**Group** - A monoid for which every element has an inverse.

If you extend the natural numbers to admit all additive inverses you get the integers `Z`.
If you extend the natural numbers to admit all multiplicative inverses for non-zero numbers you get the rational numbers `Q`.

A **ring** is a semiring in which the commutative monoid `(0, ⊕)` is a group. A ring is commutative if so is the monoid `(1, ⊗)`.

![[Pasted image 20231115100701.png]]

`q = quo(m, n)`
`r = rem(m, n)`

This can be proven by taking away `n` from `m` as many times as possible until you end up with `r < n`. This gives you `q` and `r`.
![[Pasted image 20231115103119.png]]
It can be shown that this program terminates because at each call of `divter` the second argument decreases, and it is impossible to have an ever-decreasing sequence of natural numbers.

**Partial Correctness** - Showing that an algorithm performs correctly.
**Total Correctness** - Showing partial correctness as well as that the program terminates.

**Modular Arithmetic**:
![[Pasted image 20231115104924.png]]
![[Pasted image 20231115104950.png]]

The tables for `Z4` are:
![[Pasted image 20231115105107.png]]

The modular-arithmetic structure
![[Pasted image 20231115105132.png]]
is a **commutative ring**. 

**Example 1**:
>![[Pasted image 20231115105616.png]]

*1*.
![[Pasted image 20231115105632.png]]

*2.*
![[Pasted image 20231115105651.png]]

**Integer Linear Combinations** - An integer `r` is said to be a linear combination of a pair of integers `m` and `n` whenever there are integers `s` and `t` such that `s * m + t * n = r`.
![[Pasted image 20231115105955.png]]

**Set of Divisors** - Given a natural number `n`, the set of its divisors is given:
![[Pasted image 20231120115645.png]]
e.g
![[Pasted image 20231120115704.png]]

You can also consider common divisors of two numbers,
![[Pasted image 20231120115803.png]]

**Euclid's Algorithm** -  Or even use this to find greatest common divisors
![[Pasted image 20231120121446.png]]

![[Pasted image 20231120122113.png]]

