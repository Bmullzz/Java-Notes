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

Programmers, of course, know that one of the most common operations with a numeric variable is to add or subrtract 1. Java, following in the footsteps of C and C++, has both _increment_ and _decrement_ operators: `n++` adds `1` to the current value of the variable `n`, and `n--` subtracts `1` from it. For example, the code

```Java
int n = 12;
n++;
```

changes `n` to `13`. Since these operators change the value of a variable, they cannot be applied to numbers themselves. For example, `4++` is not a legal statement.

There are two forms of these operators; you've just seen the postfix form of the operator that is placed after the operand. There is also a prefix for `n++`. Both change the value of the variable by `1`. The difference between the two appears only when they are used inside expressions. The prefix form does the addition first; the postfix form evaluates to the old value of the variable.

```Java
int m = 7;
int n = 7;

int a = 2 * ++m; // now a is 16, m is 8
int b = 2 * n++; // now b is 14, n is 8
```

It is NOT recommended to use `++` inside expressions because this often leads to confusing code and annoying bugs.

## Relational and boolean Operators

Java has a full complement of relational operators. To test equality, use a double equal sign, `==`. For example, the value of

```Java
3 == 7;
```

is `false`.

Use a `!=` for inequality. For example, the value of 

```Java
3 != 7;
```

is `true`.

Finally, you have the usual `<` (less than), `>` (greater than), `<=` (less than or equal), and `>=` (greater than or equal) operators.

Java, following C++, uses `&&` for the logical "and" operator and `||` for the logical "or" operator. As you can easily remember from the `!=` operator, the exclamation point `!` is the logical negation operator. The `&&` and `||` operators are evaluated in "short circuit" fashion: The second argument is not evaluated if the first argument already determines the value. If you combine two expressions with the `&&` operator, 

```Java
expression1 && expression2
```

and the truth value of the first expression has been determined to be `false`, then it is impossible for the result to be `true`. Thus, the value for the second expression is _not_ calculated. This behavior can be exploited to avoid errors. For example, in the expression

```Java
x != 0 && 1 / x > x + y // no division by 0
```

The second part is never evaluated if `x` equals zero. Thus, `1 / x` is not computed if `x` is zero, and no divide-by-zero error can occur. 

Similarly, the value of `expression1 || expression2` is automatically `true` if the first expression is `true`, without evaluating the second expression.

Finally, Java supports the ternary `?:` operator that is occasionally useful. The expression

```Java
condition ? expression1 : expression2
```

evaluates to the first expression if the condition is `true`, to the second expression otherwise. For example,

```Java
x < y ? x : y
```

gives the smaller of `x` and `y`.

## Bitwise Operators

When working with any of the integer types, you have oeprators that can work directly with the bits that make up integers. This means that you can use masking techniques to get at individual bits in a number. The bitwise operators are

```Java
&("and") | ("or") ^("xor") ~("not")
```

These operators work on bit patterns. For example, if `n` is an integer variable, then 

```Java
int fourthBitFromRight = (n & 0b1000) / 0b1000;
```

gives you a `1` if the fourth bit from the right in the binary representation of `n` is `1`, and `0` otherwise. Using `&` with the appropriate power of `2` lets you mask out all but a single bit.

- NOTE: When applied to `boolean` values, the `&` and `|` operators yield a `boolean` value. These operators are similar to the `&&` and `||` operators, except that the `&` and `|` operators are not evaluated in "short circuit" fashion - that is, both arguments are evaluated before the result is computed.

There are also `>>` and `<<` operators which shift a bit pattern to the right or left. These operators are convenient when you need to build up bit patterns to do bit masking:

```Java
int fourthBitFromRight = (n & (1 << 3)) >> 3;
```

Finally, a `>>>` operator fills the top bits with zero, unlike `>>` which extends the sign bit into the top bits. There is no `<<<` operator.

- **CAUTION**: The right-hand argument of the shift operators is reduced modulo 32 (unless the left-hand argument is a `long`, in which case the right-hand argument is reduced modulo 64). For example, the value of `1 << 35` is the same as `1 << 3` or `8`.

## Parentheses and Operator Hierarchy

The table below shows the precedence of operators. If no parentheses are used, _operations are performed in the hierarchal order_ indicated. Operators in the same level are processed from left to right, except for those that are right-associative, as indicated in the table. For example, `&&` has a higher precedence than `||`, so the expression

```Java
a && b || c
```

means

```Java
(a && b) || c
```

**Operator Precendence**

| Operators                  | Associativity   |
| ---                        | ---             |
| `[] . () (method call)`    | Left to Right   |
| `! ~ ++ -- + (unary) - (unary) () (cast) new`| Right to Left |
| `* / %`                    | Left to Right   |
| `+ -`                      | Left to Right   |
| `<< >> >>>`                | Left to Right   |
| `< <= > >= instanceof`     | Left to Right   |
| `== !=`                    | Left to Right   |
| `&`                        | Left to Right   |
| `^`                        | Left to Right   |
| `|`                        | Left to Right   |
| `&&`                       | Left to Right   |
| `||`                       | Left to Right   |
| `?:`                       | Right to Left   |
| `= += -= *= /= %= &= (verticalBar)= ^= <<= >>= >>>=` | Right to Left  |

Since `+=` associates right to left, the expression

```Java
a += b += c
```

means

```Java
a += (b += c)
```

That is, the value of `b += c` (which is the value of `b` after the addition) is added to `a`.