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

**Consensus Theorem** - Combination of unique literals of all the terms
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
	- Continue until finsihed

This method works because each column identifies groups of powers of two. If an element in the previous column does not contribute to another one then it has no adjacent `1`s and covers an essential term.

The second step is to find the lowest number of identified prime implicants that will cover the entire function. This can be done with a prime implicant table like so:
- Write all minterms as the columns (ignore don't-cares)
- Write all prime implicants as the rows, and include all the minterms covered by that implicant (dashes can mean 0 or 1)
- Match the covered minterms in the rows and columns
![[Pasted image 20231011111643.png]]
- Then look for columns that have only a single X  - that minterm is covered by only one prime implicant. Those implicants must be included in the result
- Then cross out the minterms that are covered by the essential minterms to find any remaining uncovered minterms
![[Pasted image 20231011112008.png]]
- Finally find the fewest number of prime implicants to cover the remaining minterms.
![[Pasted image 20231011112111.png]]
# 3
---
### Binary Adders
**Half Adder** - A logic circuit that takes in two binary inputs and returns a sum and a carry output.
![[Pasted image 20231011112321.png]]
By inspection: `c = a . b`, `sum = a xor b`

**Full Adder** - A logic circuit that takes in three binary inputs and returns a sum and a carry output
![[Pasted image 20231011112419.png]]
The functions for `c` and `sum` can be simplified through boolean expression manipulation.

**Ripple Carry Adder** - Multiple full adders connected together to sum two n-bit binary numbers.
![[Pasted image 20231011112716.png]]
However this initial design can become very slow because of the sequential additions that rely on previous carries.

![[Pasted image 20231011114027.png]]
Another option is to generate the carries separately to the addition and much faster, so that the full adders can run parallel.

![[Pasted image 20231011114140.png]]
Generate a truth table for the carry result for a full adder. This lets you find expressions for carries at any index and then *express them using carries of lower indices* all the way down to the original carry in. This means you can generate an expression for a carry of any index solely using `an`s, `bn`s and `c0`.

![[Pasted image 20231011114836.png]]

However this can get very complicated very quickly, especially with larger indexes and n-bit numbers. To reduce complexity, you might use fast carry generation in e.g blocks of 4-bit adders

![[Pasted image 20231011114947.png]]
- Conventional ripple carry within 4-bit blocks
- Fast carry generation between 4-bit blocks
- Trade off between complexity and speed.

### Multilevel Logic
**Multilevel Logic** - Using more functions/logic circuits (e.g. not just SoP or PoS) such as a ripple carry adder. Used because:
- Commercially available logic gates usually only available with a restricted number of inputs, typically, 2 or 3.
- System composition from sub-systems reduces design complexity, e.g., a ripple adder made from full adders 
- Allows Boolean optimisation across multiple outputs, e.g., common sub-expression elimination. A SoP expression might only need two levels of gates, but could have many more gates and literals than if it was improved/factorised to have more levels of gates.
# 4
---
### Hazards
**Gate Propagation Delay** - Gate delay can lead to "hazards" which are brief unwanted logic changes at gate level.
**Static Hazard** - The output undergoes a momentary transition when one input changes when it is supposed to remain unchanged.
**Dynamic Hazard** - The output changes more than once when it was supposed to change just once.

Hazards can be represented with *timing diagrams*, however they do not show "wobble" of signals or the small amount of time they take to changes states.
![[Pasted image 20231013110529.png]]

**Static Hazard**
![[Pasted image 20231013110629.png]]

**Dynamic Hazard**
![[Pasted image 20231013110644.png]]

**Fixing Static Hazards**
![[Pasted image 20231013110823.png]]

The above circuit has a static hazard because when `x = z = 1` and `y` changes from `1` to `0`:
- `u` will change to `0`
- `t` will change to `1`
- `w` will change to `0`
- `v` will change to `1`
- `w` will change to `1`

Because of the different delays between `u` and `t -> v`, `w` may end up changing to `0` for a small amount of time.

To fix a static hazard, you can draw a Karnaugh Map of the output concerned. Then add an extra term which overlaps the essential terms. A static hazard will occur when there are adjacent `1`s or `0`s that are not in the same group.

![[Pasted image 20231013111540.png]]

### Multiplexers
**Multiplexer** - Chooses 1 of many inputs to steer to a single output.

![[Pasted image 20231013111753.png]]

You can have an `n:1` mux, as long as you have enough inputs and enough *control inputs* (acting as bits) to create enough unique mappings for each input. For example, and `8:1` mux would need 3 control inputs.

A multiplexer can also be used to to implement combination logic functions. The inputs can select which minterms appear at output (essentially this is using the mux in the opposite way, instead of `xyz` mapping one `I` to the output, its using `I` to map different combinations of `xyz` to output).

![[Pasted image 20231013111950.png]]

Alternatively you can use a smaller multiplexer for the same result by using one of the inputs as a variable. This can be demonstrated with a truth table. Essentially you are trying to determine if the output `f` can be determined by any "simpler" function of the inputs. In the example below, when `xy` is `00` or `01`, the output `f` is always just the complement of `z`, so `!z` can be used as an input for those two combinations of `xy`.

![[Pasted image 20231013112821.png]]
![[Pasted image 20231013112511.png]]
### Demultiplexers
**Demultiplexer** - The opposite of a mux, a single input is directed to exactly one output.
![[Pasted image 20231013113102.png]]

**Decoder** - Where the input `g` is always `1`, so the selector only determines which output will "receive" that `1`.

Decoders or demultiplexers may be used to "enable" different systems.
![[Pasted image 20231013113356.png]]

A logical expression having DNF form can be implemented by ORing together the required minterms at the decoder output.
### ROM
**ROM** - Read Only Memory, usually written into once but readable any time. It uses `n` bits to store memory locations/addresses, and `m` bits to store data values. Total number of bits stored is `m * 2^n`

ROM can implement combinational logic just by programming the output for each minterm combination into the correct memory location. If 3 bits are used to store the minterm in the address then only one bit is needed in the data to store the output of that minterm.
Then the minterms can be looked up and the results immediately accessed (and then combined however necessary).
Since only one data bit is needed for the result, multiple logic functions could be stored in one ROM.
![[Pasted image 20231013114330.png]]
### Programmable Logic Array
![[Pasted image 20231013114546.png]]

Essentially it is two planes that allow you to input any combination of minterms and will automatically perform the `and` and `or` operations on them to create the function result. This is useful because it allows you to create any DNF function instead of creating each individual logic operation for one specific function.
### Programmable Array Logic
![[Pasted image 20231013114810.png]]

This is a simpler structure where the `OR` plane is not as programmable, but it is less efficient.
### Memory Application
**Non-Volatile** - Memory that remains intact when it loses power.
The CPU uses busses to access external memory devices.

**Address Bus** - Used to specify the memory location that is being read or written
**Data Bus** - Used to carry the data to and from that location.
Often more than one memory devices will be connected to the same data bus.

**Bus Contention** - Where different memory devices want to use the data bus at the same time.
**Tri-State Buffers** - A solution to bus contention, where a buffer can be `1`, `0` or "disconnected". This means that particular memory locations can retain their state but not send it along the data bus until it is free. This is usually controlled by "output-enable", which connects/disconnects the cell.

Other control inputs are also provided for any particular memory cell.
**Open Enable** - Whether or not the data is being sent along the data bus
**Write Enable** - Determines whether the data is being writable or readable
**Chip Select** - Determines if the chip is activated.
Often these control signals are "active low", where they are enabled with a low voltage or a "0".
