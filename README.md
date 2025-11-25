# Usage

`cd` into the root directory of the project, then enter `cabal install —installdir=<path>`.

The executable will be installed at `path/glang` - add it to your `PATH`, and use the `glang` command to interpret programs. 

# Tutorial

glang is a functional programming language in the vein of the ML family.  It is non-strictly-evaluated, meaning that arguments to functions aren’t evaluated unless they’re returned by the body of the function, or passed to a strict function (i.e. built-in arithmetic operators).  

A program in glang specifies an entrypoint (a main function), as well as a series of definitions binding names to expressions.  Expressions are defined in a manner analogous to the lambda calculus - an expression is either a function, a value, or the application of a function to another expression.  

Syntactically, a function is written as a series of parameters separated by periods, then an expression.  The application of a function is written as a series of arguments separated by single spaces, preceded by the name of the function. 
```
foo = x. y. x + y + 3

result = foo 4 5
```
Here’s an example program, which calculates the ninth term (zero-indexed) in the Fibonacci sequence:
```
fib = n. cond ((n == 0) | (n == 1)) n (fib (n-1) + fib (n-2)) 

main = print (fib 9)
```
Currently, there are only two built-in datatypes — floats and boolean values, which are expressed as True or False. 
```
num = 3.879

bool = False
```
The language contains several built-in operations, which deviate from user-defined functions in that they are evaluated strictly - this means that the arguments passed to them are always fully evaluated.  The infix operations, in order of precedence, are:
```
== (equals)

& (logical AND)

| (logical OR)

* (multiplication), / (division)

+ (addition), - (subtraction)
```
Furthermore, the language includes a `print` operator for I/O.  It returns its argument, meaning that you can think of it as an identity function that has a side-effect of writing its argument to `stdout`.  

For control flow, you can use the `cond` operator. `cond` takes three arguments - a boolean value, an expression that’s returned if the boolean value is true, and an expression that’s returned if the boolean value is false.
```
cflow = x. y. (x == y) (x + 3) (y * 4)

-- prints 8
main = print (cflow 5 5)
```
Note that cond is only strict in the first argument, and is lazy in the others.  Speaking of lazy evaluation, here's an example of how non-strict semantics facilitate infinite functional lists:
```
cons = a. b. f. f a b

head = l. l (x. y. x)

tail = l. l (x. y. y)

inf = x. cons x (inf x)

main = print (head (tail (inf 4)))
```
For further reference, there are a number of examples located in the `examples` folder.  


