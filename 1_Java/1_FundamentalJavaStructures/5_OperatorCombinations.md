# Operators Continued

- Combining Assignment with Operators
- Increment and Decrement Operators
- Relational and Boolean Operators
- Bitwise Operators
- Parentheses and Operator Hierarchy

## Combining Assignment with Operators

There is a convenient shortcut for using binary operators in an assignment. For example,

```Java
x += 4;
```

is equivalent to 

```Java
x = x + 4;
```

(In general, place the operator to the left of the `=` sign, such as `*=` or `%=`.)

- NOTE: If the operator yields a value whose type is diffent than that of the left hand sidfe, then it is coerced to fit. For Example, if `x` is an `int`, then the statement

```Java
x += 3.5;
```

- is valid, setting `x` to `(int)(x + 3.5)`.

## Increment and Decrement Operators

Programmers, of course, know that one of the most common operations with a numeric variable is to add or subrtract 1. Java, follwoing in the footsteps 