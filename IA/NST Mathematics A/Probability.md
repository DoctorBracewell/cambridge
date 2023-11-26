**Outcome** - The mutually exclusive possible results of an experiment.
**Sample Space** - The set of all possible outcomes of an experiment
**Event** - A subset of the sample set.

![[Pasted image 20231123090151.png]]

**Probability** - How likely a particular event is.
![[Pasted image 20231123090210.png]]
![[Pasted image 20231123090236.png]]

**Conditional Probability** - The probability of one thing given another happens:
![[Pasted image 20231123090307.png]]

**Baye's Theorem** - 
![[Pasted image 20231123090421.png]]
or even
![[Pasted image 20231123090447.png]]

**Permutations** - Consider `n` distinct items, select `r` of them without replacement and put them in order. This is a permutation.
![[Pasted image 20231123090611.png]]
e.g. how many ways to arrange the club from a pack of cards.

If you consider `n` items and select `r` *without* putting them in order you get a combinatoric.
![[Pasted image 20231123090718.png]]
e.g. how many different sets of 5 hands are there in a pack of cards.

**Random Variable** - A variable whose value depends on the outcome of some experiment depending on randomness or chance.
Could take discrete values or a range of continuous values.

For a particular discrete value you can construct a **probability function**, or even a cumulative probability function, defining the probability of each outcome.
![[Pasted image 20231123090951.png]]

**Mean** - The "expected value" of the random variable. If an experiment is repeated many many times, the average value of all the results will approach `E(X)`.
![[Pasted image 20231123091106.png]]

Has the following properties:
![[Pasted image 20231123091145.png]]

**Variance** - A measure of spread around the mean.
![[Pasted image 20231123091216.png]]

**Standard Deviation** - Square root of the variation.
![[Pasted image 20231123091243.png]]

**Binomial Distribution** - Arises when an experiment has two incomes, describes how the result is distributed.
![[Pasted image 20231123091456.png]]

![[Pasted image 20231123091513.png]]

**Mean of Binomial** -
![[Pasted image 20231123091946.png]]
= `np`.

**Variance of Binomial** -
![[Pasted image 20231123092030.png]]
\=
![[Pasted image 20231123092051.png]]

**Poisson Distribution** - A distribution when the number of "successes" is around to be unlimited. This can also be seen as a "rate", successes per some unit of time.

![[Pasted image 20231123093137.png]]

You can also define the poisson distribution using the binomial distribution as so:
![[Pasted image 20231123093359.png]]
(not required to prove).

![[Pasted image 20231123093458.png]]

**Mean of poisson** -
![[Pasted image 20231123094038.png]]
= `λ`

**Variance of poisson** - 
![[Pasted image 20231123094105.png]]
![[Pasted image 20231123094351.png]]
= `λ`

**Continuous Random Variables** - Consider a random variable `X` that can take any value in a continuous range, in general `-infinity < X < infinity`.
We can no longer assign a probability to `X` taking a single value, but instead define the probability that `X` takes a value in an infinitesimally small interval, e.g.

![[Pasted image 20231123094926.png]]
We set this to a **probability density function**. Then we can integrate this to find the value.