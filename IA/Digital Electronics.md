# 1
---
### Aims
- Combinational logic circuits
- Sequential logic circuits
- How digital logic gates are built with transistors
- Simple processor architectures
- Design and build digital logic systems.

- Be able to design and construct simple DE systems
- Apply boolean logic and algebra
- Design and build state machines

### Managing Complexity
**Microprocessors** - Made from millions of transistors, impossible for a human to
**Abstraction** - Hiding unnecessary detail, e.g in DE don't have to consider the actual movements of electrons. It is good to know levels around the one you are working at.
Primarily the course will be around devices, digital circuit and logic elements.
![[Pasted image 20231006112839.png]]

### Microprocessor
**Architecture** - The instruction set and registers available
**Microarchitecture** - The specific arrangement of registers, ALUs, controllers, memories and other logic blocks needed to implement the architecture.
A particular architecture can be implemented by many different microarchitectures. Trade-offs between performance, complexity and cost.

### Logic Gates
**Binary/Boolean/Logic Variables** - Can only take on 2 values, e.g. true/false, 0/1, on/off
Particularly useful in electronics because high/low voltages can be used. Higher immunity to electrical noise because variations are less likely to cause mistakes/invalid values.
**Logic Gate** - A circuit with one or more input and one output

**NOT Gate** - `y` is true if `a` is false. Also known as an inverter.
![[Pasted image 20231006113758.png]]

**AND Gate** - `y` is only true if `a` is true and `b` is true
![[Pasted image 20231006113908.png]]

**OR Gate** - `y` is only true if `a` is true or `b` is true
![[Pasted image 20231006114000.png]]

**XOR Gate** - `y` is true if `a` is true or `b` is true but not both
![[Pasted image 20231006114127.png]]

**NAND Gate** - `y` is true is `a` is false or `b` is false or both
![[Pasted image 20231006114231.png]]

**NOR Gate** - `y` is true only if `a` is false and `b` is false
![[Pasted image 20231006114428.png]]

### Combinational Logic
**Combinational Logic Circuits** - Ones that do not have an internal stored state, so the output is solely a function of the current inputs.
### Boolean Algebra Simplification
**Boolean Algebra Examples**
`a + 0 = a`
`a + a = a`
`a + 1 = 1`
`a + !a = 1`

`a . 0 = 0`
`a . a = 0`
`a . 1 = a`
`a . !a = 0`

AND takes precedence over OR.

**Commutation** - `a + b = b + a`
**Association** - `(a + b) + c = a + (b + c)`
**Distribution** - `a.(b + c) = (a.b) + (a.c)`
**Absorption*** - `a + (a.c) = a` | `a.(a + c) = a`

**Consensus Theorem** - Conjunction of unique literals of all the terms
Another technique is to expand terms until they all include one instance of each variable (or its compliment). 

**DeMorgan's Theorem** - Complement all terms, switch all ANDs to ORs or vice versa then complement the entire expression. `a + b + c = !(!a . !b . !c)`
Can be proved with a truth table or extended to more variables by induction. 

DeMorgan's can also be applied to gates, especially if you want to turn ANDs and ORs into NANDs or NORs as these are more efficient.
![[Pasted image 20231006120536.png]]

# 2
---
### Boolean Algebra Simplification

Any boolean function can be implemented directly using combinational logic (gates).
Often boolean functions can be simplified to reduce the number of gates required.

**Truth Table** - Mapping all possible inputs to outputs (or intermediate outputs)
![[Pasted image 20231009110827.png]]
### Boolean Function Forms
**Minterm** - Contains all variables either complemented or uncomplemented and ANDed together
**Disjunctive Normal Form** - ORing all of the minterms of a function
**Sum of Products Form** - A function expressed as the ORing og ANDed variables.

**Maxterm** - Contains all the variables either complemented or uncomplemented and ORed together
De Morgan's law tells us that the maxterms of `f` are the complemented minterms of f complemented.
**Conjunctive Normal Form** - ANDing all of the maxterms of a function
**Products of Sums Form** - A function expressed as the ANDing of ORed variables.

*SoP* and *PoS* forms are individually useful when implementing a boolean function using logic gates, especially since combinations of AND and OR gates can be replaced with NAND gates (when in *SoP* form) or NOR gates (when in *PoS* form).

### Karnaugh Maps
**Karnaugh Map** - A visual tool for carrying out simplification and manipulation of logical expressions up to 5 variables.
![[Pasted image 20231009111452.png]]
- Group size must be a power of 2
- Large groups are best as they contain fewer variables
- Gropus can wrap around edges and corners
- Join groups together with OR

### Karnaugh Maps into PoS Form
Karnaugh maps will always result in *Sum of Products* form as individual terms are formed of ANDed variables and combined into one function with ORs.

To generate a *Product of Sums* form:
- Collect `0`s instead of `1`s
- Which will give the function complement in *SoP* form
- Apply De Morgan's law which will give the function complement in *PoS* form (with an additional complement)
- Complement to generate the normal function.

### PoS Form into Karnaugh
- If an expression is already in *PoS* form, you can just use a truth table to fill it in
- However there is a faster method:
	- Apply De Morgan's and take complement (giving function complement in *SoP* form)
	- Plot function complement (plot `0`s)
	- Fill remaining cells with `1`s
	- Simplify usually.

### Don't Care Conditions
**Don't Care Condition** - Certain input combinations that can never happen.
In any simplification, these can be treated as a `0` *or* `1`, depending on which gives the simplest result (or ignored entirely).

### Definitions
**Cover** - A term covers a minterm if that minterm is part of that term.
**Prime Implicant** - A term that cannot be further combined (e.g. groups in a Karnaugh Map)
**Essential Prime Implicant** - A PI that coves a minterm that no other PI covers.
**Covering Set** - A minimum set of PIs which includes all essential terms plus any other prime implicants required to cover all minterms.

The result of an efficiently-simplified karnaugh map is the same as the minimum covering set. The minimum covering set *must* include all essential prime implicants and a minimum amount of others.
![[Pasted image 20231009114220.png]]

### Tabular Simplification
Karnaugh Maps are not practical beyond 5-6 variables.
**Quine-McCluskey Method** - A systematic method that finds the minimised representation of any boolean function
- First use the QM Implication Table to find all prime implicants
- Find the minimum cover set using the prime implicant chart

### Example
- Write out all of the minterms and don't-cares in their binary format, group them by the number of `1`s in their representation
- Then apply the *uniting theorem*:
	- Compare elements in the 1st group with all elements in the second. If they differ by a single bit, they are adjacent terms.
	- Place these elements in the 2nd column with that single bit replaced by a dash.
	- Terms in the 1st column that contribute a term in the second are ticked (they are not prime implicants)
	- Do the same for all of the groups in the 1st column
	- Then repeat for the 2nd column into the 3rd. They must differ by a single bit but also have a dash in the *same place*.
	- Groups in the 2nd column that do not contribute to the 3rd column are marked with an asterix (these are prime implicants)








