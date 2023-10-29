**i** - The square root of `-1`
**Real Number** - One with no imaginary parts
**Imaginary Number** - A number containing `i`
**Complex Number** - A number with real and imaginary parts, often expressed as `z = x + iy`

Complex numbers can be added, multiplied (thanks to the identity `i*i = -1`), subtracted and divided.
![[Pasted image 20231021125926.png]]

Complex numbers inherit commutation and associativity from real numbers

**Complex Conjugate** - A complex number with a negative imaginary part, `z* = x - iy`
`zz* = x^2 + y^2`, `|z| = sqrt(zz*) = sqrt(x^2 + y^2)`

**Division**:
![[Pasted image 20231024090532.png]]

**Complex Plane/Argand Diagram** - The real axis on the `x` axis and imaginary axis on the `y` axis.
![[Pasted image 20231024091026.png]]

Complex numbers can also be represented in a polar form, using `r` and `θ`, where:
![[Pasted image 20231024092033.png]]
![[Pasted image 20231024092040.png]]
hence:
![[Pasted image 20231024092112.png]]
![[Pasted image 20231024092304.png]]

**Loci** - Equations such as `|z| = |z - i|` can represent shapes on the Argand Diagram. These are often solvable by setting `z = x + iy`, squaring both sides and inspecting for a cartesian equation.

**Exponential Form** - Complex numbers can be written in the form `z = re^iθ`, because:
![[Pasted image 20231024093621.png]]
and hence:
![[Pasted image 20231024093647.png]]
![[Pasted image 20231024093701.png]]

A geometrical interpretation of complex number multiplication is a *rotation of `z1` by the argument of `z2` and scaling the modulus of `z1` by the modulus of `z2`* (shown by the exponential form manipulation above.

**Exponential Imaginary forms of cos and sin**:
1. ![[Pasted image 20231024093621.png]]
2. ![[Pasted image 20231024094330.png]]
hence:
![[Pasted image 20231024094339.png]]
![[Pasted image 20231024094346.png]]

**Fundamental Theorem of Algebra**:
![[Pasted image 20231027101837.png]]

**De Moivre's Theorem**:
![[Pasted image 20231027102205.png]]
This can be used to find trigonometric identies, e.g. `cos^5(θ)` in terms of `cos(5θ)`, or sums of trig functions by taking the real/imaginary part.

**Roots of Unity**:
`z^n = 1` has `n` roots, solvable by writing e.g. both in modulus-argument and using De Moivre's theorem.

This roots are distributed equally around the unit circle
![[Pasted image 20231027102116.png]]

**Complex logarithm**:
![[Pasted image 20231027102457.png]]
`ln z` is a multi-valued function because `θ` can take multiple values. The principal value is often the one between `-π` & `π`.

![[Pasted image 20231027103348.png]]

![[Pasted image 20231027103617.png]]
^ From rules of exponents
![[Pasted image 20231027103924.png]]
^ doesn't use a `ln` because `ln i` simplifies to `iπ/2`.

**Oscillation Problems**:
Complex numbers can be used to solve these, such as the the swinging of a pendulum.
Consider a pendulum swinging with angular momentum, therefore angular displacement is given by (parametric equation of a circle):
![[Pasted image 20231027104256.png]]
![[Pasted image 20231027104332.png]]
![[Pasted image 20231027104341.png]]

Therefore you can differentiate to find velocity of acceleration of the pendulum at any point:
![[Pasted image 20231027104413.png]]

**Hyperbolic Functions**:
![[Pasted image 20231028093403.png]]
`cos(ix) = cosh(x)`
`sin(ix) = isinh(x)`
`tan(ix) = itanh(x)`

**Identities**:
![[Pasted image 20231028093605.png]]
![[Pasted image 20231028093619.png]]
![[Pasted image 20231028093650.png]]

**Inverse Hyperbolic Functions**:
`sinh(y) = x`
`1/2 (e^y - e^-y) = x`
`e^2y - 1 = 2xe^y`
`e^2y - 2xe^y - 1 = 0`
`e^y = x +/- sqrt(x^2 + 1)`
`y = ln ( x + sqrt(x^2 + 1) )` (negative root is discarded)
^ and similar for cosh
`