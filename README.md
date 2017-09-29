# equa.lua
equa.lua (standing for equation) is a library for the programming language Lua.

This library focuses on dynamically producing functions from simple logical expresisons. For example:
```
    require("equa")
    hyp = sqrt(x^2 + y^2)
    print(hyp(3, 4))
```
This full program prints `5.0`, which is the hypotanuse of a right angle triangle of sides 3 and 4 respectively.

## Protoexpressions and Protovariables

All global lua variables now default to a state referred to as a **protovariable**.

When a protovariable is operated upon, it becomes a **protoexpression**. For example, caluclating `x+3` where `x` is a protovariable will create the protoexpression `x+3`. You can see this by printing it, with `print(x+3)`.

Most lua functions and operators are wrapped so that they build onto a protoexpression, and many libraries are also placed into the main namespace. Thus, `sqrt(x^2 + y^2)` creates a protoexpression which contains 2 protovariables, `x` and `y`.

## Using Protoexpressions

When a protoexpression is called, it will begin evaluation depending on a few situations.

### Supplying regular values

When you supply one or more values to a protoexpression, it will begin to evaluate, substituting each protovaraible with the next value in alphabetical order.

As an example, calling `(x+y+x)(1,2)` first substitutes `x` with `1`, then `y` with `2`, which produces `4` after evaluation.

If there aren't enough values provided, it will partially evaluate. If the same example was only provided the value `1`, then it would return the new protoexpression `1+y+1`.

Because this is still a protoexpression, protoexpressions are defaultly curryable, so one could just as easily call `(x+y+x)(1)(2)` to achieve the first result.

Protoexpressions may also be passed into protoexpressions. For example, `(x+1)(n^2)` subtitutes `x` with the protoexpression `n^2`, which produces `n^2+1`.

### Supplying a table

By calling `expression({x = 3})` or `expression{x = 3}`, you perform a named evaluation. This will evaluate the protoexpression in which all instances of the protovariable `x` will be substituted for the value `y`. Like the other method, a protoexpression can be supplied still, and partial evaluation can occur.

## Additional functions

Alongside the protoexpressions, equa.lua provides a few functions in the global namespace to make working with them easier.

* `toTeX(exp)`: Creates some LaTeX based off the input expression.
* `call(exp, ...)`: Call takes a function or a protoexpression, and once all it's arguments are evaluated, calls it on the arguments.
* `#exp`: Calling `#` on a protoexpression will wrap it in a function, preventing it's values from being substitued in evaluation unless explicitely done so.
* `isEq(val)`: Returns true if the provided value is a protoexpression or a protovariable, false otherwise.
* `wrap(func, name)`: returns a function which can accept protoexpressions and remember them. `name` is for string conversion, and is optional.
* `brak(val)`: returns a table containing the value `val`.
* `len(val)`: returns the length of val.
* `index(table, ind)`: Returns the value of `table[ind]`.
* `lt, gt, le, ge, eq, ne, orr, andd`: A bunch of functions for comparing variables.
* `branch(condition, truthy, falsey, ...)`: When `condition` is evaluated, executes `truthy` if `condition` is truthy, and `falsey` otherwise, passing to them the contents of `...` as arguments.
* `floop(first, condition, whilst, body, ...)`: When `first` is evaluated, it stores it's value. it then checks if condition, when passed the stored value and the contents of `...` is truthy. If it is, then `body` is evaluated with the same, then so is `whilst` and stores the return value of that instead, then loops back to checking cond. Once finished, it returns the last returned value of `whilst`
* `wloop(cond, body, initial, ...)`: Stores `initial`, then checks if the value of `cond` when passed `initial` and `...` is truthy, if it is, then `body` is called with the same, and `initial` is replaced with it's return value, it then loops back to checking cond. `initial` is then returned.