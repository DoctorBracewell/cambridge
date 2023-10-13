**Name:** Brace Godfrey
**CRSId:** dg681
### 1.1
This approach would limit the number of available years to be stored to be 100, between `1950` up to `2049`. Whilst this is the same number of years as the original two-digit implementation representing years between 1900 to 1999, it is more reasonable and useful as more years into the future could be stored.
However, there are multiple disadvantages of the solution. It would be additional cognitive load on the programmer to remember how the digits "wrap around" the turn of the century, perhaps leading to bugs in program or making it less clear to other programmers. Any implementation of the standard would also require additional logic to check whether the date is in the 1900s or the 2000s, as well as providing functions to operate on dates such as adding/subtracting/calculating intervals
In addition, the "Year 2000" problem would just occur again after `2049` because any programs using the implementation would interpret a `50` as `1950`.
### 1.2
a)
```ocaml
let compare_offset_dates a b = 
  if a > b then
    "a is greater"
  else if b > a then
    "b is greater"
  else
    "the dates are the same"

let offset_dates d = 
  if d <= 49 then
    d + 50 
  else 
    d - 50
    
let compare_dates a b = 
    if a >= 0 && a <= 99 && b >= 0 && b <= 99 then
      compare_offset_dates (offset_dates a) (offset_dates b)
    else 
      failwith "invalid dates!"
```

b) 
```ocaml
let add y n = 
  if y < 0 || y > 99 then
    failwith "invalid date!"
  else if n < 0 then
    failwith "invalid sum!"
  else
    if y >= 50 then
      if y + n <= 99 then
        y + n
      else
        y + n - 100
    else
      if y + n <= 49 then
        y + n
      else
        failwith "date overflow!"
```
### 1.3
An `if-else` expression/pattern takes in some other expression that resolves to a boolean. If this boolean is true, the `if` branch will resolve, if it is false then the `else` branch will resolve. In the example, the `if-else` pattern is unnecessary because the expression in the `...` would already result in a boolean value (`true` or `false`). The programmer should simply return this expression.

In the second example the programmer should negate the result of the expression. This will result in `false` if the expression resolves to `true` and vice versa. Most languages offer an operator of some kind to negate or "switch" boolean values.
### 1.4
The type checker knows that `power` returns a float because its base case returns its parameter `x`. Inside the function body, the `power` function is only ever called with a float as that parameter (shown by the use of the `*.` floating-point multiplication operator), such as on line 4 and line 6, where `power (x *. x)` is used.
The type checker might also know from line 6 where `power` is used as a parameter in a float multiplication, `x *. power (x *. x)`, although I am unsure which of the two reasons above would contribute the type signature.
### 1.5
```ocaml
let rec mul x n =
  if n = 0 then
    x
  else 
    x +. mul x n - 1
```
### 1.6
`γ50 ~= -0.618121843485747391`
```ocaml
let rec calculate curr amnt stop =
  if amnt = stop then
    curr
  else
    calculate (1.0 /. (curr -. 1.0)) (amnt + 1) stop

calculate ((1.0 +. sqrt 5.0) /. 2.0) 0 50
```
---
### 2.1
```ocaml
let power_i x n = 
  let rec power acc x n = 
    if n = 1 then
      acc
    else
      if n mod 2 = 0 then
        power (acc * acc) x (n / 2)
      else 
        power (acc * acc * x) x (n / 2)
  in
  power x x n
```
### 2.2
`f(n) = O(g(n))` where `|f(n)| <= c * |g(n)|`
Not sure what a<sub>i</sub> is defined as? I think you would use the formal definition of Big O complexity as above but I don't understand how a<sub>i</sub> relates?
### 2.3
What does closed form mean?
> Given 𝑛 + 1, it performs a constant amount of work (an addition and subtraction) and calls itself recursively with argument 𝑛. We get the recurrence equations 𝑇 (0) = 1 and 𝑇 (𝑛 + 1) = 𝑇 (𝑛) + 1. The closed form is clearly 𝑇 (𝑛) = 𝑛 + 1, as we can easily verify by substitution. The cost is linear.

Answer: `O(log(n + 1))`

---
### 3.1
a)
```ocaml
let rec sum_r list = match list with
  | hd::tl -> hd + sum_r tl
  | [] -> 0
```

b)
```ocaml
let sum_i list = 
  let rec sum acc = function
    | hd::tl -> sum (acc + hd) tl
    | [] -> acc
  in
  sum 0 list
```

The recursive implementation will use more stack space as the calculation cannot begin until tails that are also lists have been passed into the function. Only once the tail is an empty list can the function start to resolve the calculations until it has added all of the items.
In contrast, the iterative implementation uses an accumulator so calculations can be performed in constant space complexity, as the current item is added to the accumulator before being passed into the function so no intermediate values need to be kept in memory.
### 3.2
```ocaml
let rec last_item  = function
    | hd::tl ->
        if tl = [] then
            hd
        else
            last_item tl
    | [] -> failwith "no last item"
```
I believe that the most efficient way to find the last item in a list is to just traverse the list one item after another until you reach a list with a tail that is an empty list. This is the most efficient method because OCaml does not represent lists as items in specific indices but as a "head" (a single item) linked to a "tail", where the tail is another list.
### 3.3
```ocaml
let even_items list =
  let rec skip_items acc skip = function
    | hd::tl -> x
      if skip then
        skip_items acc (not skip) tl
      else
        skip_items (acc @ [hd]) (not skip) tl
    | [] -> acc
  in
  skip_items [] true list
```
### Exam Question
a)
```ocaml
let rec item_in_list item = function
  | hd::tl -> 
    hd = item || item_in_list item tl
  | [] -> false

let rec intersect set_1 set_2 =
  match set_1 with 
    | hd::tl -> 
      if item_in_list hd set_2 then
        hd :: intersection tl set_2
      else
        intersection tl set_2
    | [] -> []
```

b)
```ocaml
let rec subtract set_1 set_2 =
  match set_1 with 
    | hd::tl -> 
      if item_in_list hd set_2 then
        subtract tl set_2
      else
        hd :: subtract tl set_2
    | [] -> []
```

c)
Using append and existing `subtract` function. Unsure how to do it without append operation:
```ocaml
let union set_1 set_2 =
  set_1 @ subtract set_2 set_1
```
Type: `val union : 'a list -> 'a list -> 'a list = <fun>`