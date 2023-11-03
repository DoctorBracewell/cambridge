**First Principles** - 
![[Pasted image 20231031091250.png]]
^ Find the gradient between two points, as the distance between those two points tend to `0`.

Not all functions are differentiable everywhere - *discontinuous functions* are not differentiable at their discontinuities.
For a function to be differentiable at all `x` it must be **continuous** and **smooth**.

**Higher-Order Derivatives** - You can differentiate a function and then differentiate the result, as many times as you want. Each of these new functions represents the *rate of change* of the previous. Some functions (all polynomials with positive integer powers) will eventually differentiate to `0`, and all derivatives after that will also be `0`.

**Elementary Functions** - 
![[Pasted image 20231031092816.png]]
![[Pasted image 20231031092823.png]]
^ Can be proven by differentiating the exponential forms.

**Product Rule** -
When:
![[Pasted image 20231031092932.png]]
then:
![[Pasted image 20231031092939.png]]
^ Can be proven using first principles

**Chain Rule** - 
When:
`y = f( u ( x ) )`
then:
![[Pasted image 20231031093413.png]]

**Quotient Rule** - 
![[Pasted image 20231102090433.png]]

**Implicit Differentiation** - You can find the derivative of `y` with respect to `x` from an equation of the form:
![[Pasted image 20231102090938.png]]

then:
![[Pasted image 20231102091251.png]]and hence:
![[Pasted image 20231102091333.png]]

Another way to perform this is:
`d/dx (y) = d/dy (y) * dy/dx`

**Mean Value Theorem** - 
![[Pasted image 20231102093250.png]]
^ There is at least some point on the curve `f(x)` bounded by `a <= x <= b` such that the gradient at that point is the same as the line joining the endpoints (`a`, `b`).

**Stationary Point** - A turning point of the curve where the gradient is `0`.
- If `d2y/d2x > 0`, the stationary point is a minimum (gradient increases through the turning point)
- If `d2y/d2x < 0`, the stationary point is a maximum (gradient decreases through the turning point)
- If `d2y/d2x = 0` then:
	![[Pasted image 20231102093605.png]]
	![[Pasted image 20231102093614.png]]

**Curve Sketching** - 
![[Pasted image 20231102094658.png]]
