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
;;

calculate ((1.0 +. sqrt 5.0) /. 2.0) 0 50
```