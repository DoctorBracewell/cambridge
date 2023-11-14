**Taylor Series** - A special type of infinite series where each term is proportional to a power of `x`.
![[Pasted image 20231114112850.png]]

A crude approximation of `f(a + h)` is `f(a) + hf'(a)`
![[Pasted image 20231114113115.png]]

or `f(x) ~= f(a) + (x - a)f'(a)
and so on for all higher order derivatives
`f'(x) ~= f'(a) + (x - a)f''(a)
`f''(x) ~= f''(a) + (x - a)f'''(a)
...

and so
![[Pasted image 20231114113552.png]]
which gives:
![[Pasted image 20231114113803.png]]
This is a BETTER approximation than the linear one above because it is quadratic in `h`.

You can do this as many times as you like as long as `f(x)` is differentiable at `a`.

**Taylor Sum** -
![[Pasted image 20231114114210.png]]
where `Rn+1` is the remainder term
![[Pasted image 20231114114326.png]]
This says that the remainder is proportional by some factor to the `h^n+1` term. 

OR

**Taylor Polynomial** -
![[Pasted image 20231114114525.png]]
This is another form of Taylor's theorem that gives an approximation of the function at a specific point.

If you are using the taylor series to calculate a specific value and you want your answer to be within a specific error interval, you can calculate how high your `n` needs to be to ensure that the remainder term will be within that interval.
![[Pasted image 20231114115339.png]]

**Taylor Series** - An infinite expansion of a Taylor polynomial where, as long as the function is infinitely differentiable.
![[Pasted image 20231114115824.png]]

A taylor series about `a = 0` is a Maclaurin series.
A power series is one where each term is proportional to a power of `x`.

For example:
![[Pasted image 20231114120417.png]]

You can use this to calculate hyperbolic function power series:
![[Pasted image 20231114120528.png]]
![[Pasted image 20231114120535.png]]
![[Pasted image 20231114120708.png]]

To calculate any power series around a value, just differentiate as many times as required and put into the taylor series formula.

Sometimes certain functions have periodic differentiations, especially trigonometric functions around `x = 0`:
![[Pasted image 20231114121115.png]]
so:
![[Pasted image 20231114121129.png]]

![[Pasted image 20231114121311.png]]
so:
![[Pasted image 20231114121324.png]]

You can't necessarily define power series for logarithms because they are undefined at `x = 0`, but you could e.g. define `ln (1 + x)`:
![[Pasted image 20231114122005.png]]

**Binomial Theorem**
Consider the function:
![[Pasted image 20231114122159.png]]

Apply taylor's theorem:
![[Pasted image 20231114122227.png]]
-> power series around `x = 0`
![[Pasted image 20231114122246.png]]

When `α` is a *positive integer* (`N`)the power series stops after a finite number of terms, because the alpha derivative eventually becomes `0`. Any terms containing `α - N` becomes `0`, including the `x^(n+1)` term and all higher powers.
When `α` is between `-1` and `1`,  the power series is valid and will converge.

The binomial coefficient (nCr) is:
![[Pasted image 20231114122820.png]]
and for `α` is a positive integer the sum becomes
![[Pasted image 20231114122940.png]]
as usual.

For an