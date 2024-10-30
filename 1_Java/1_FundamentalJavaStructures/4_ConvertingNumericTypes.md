# Conversions between Numeric Types

It is often necessary to convert from one numeric type to another. The follwing graph show legal type conversions: 

![type conversions](image.png)

The following conversions happen without information loss:

- `byte -> short -> int -> long`
- `byte -> short -> int -> double`
- `char -> int -> long`
- `char -> int -> double`
- `float -> double`

Conversions that may lose precision:

- `int -> float`
- `long -> float`
- `long -> double`

For example, a large integer such as `123456789` has more digits than the `float` type can represent. When the integer is converted to a `float`, the resulting value has the correct magnitude but loses precision.

```Java
int n = 123456789;
float f = n; // f is 1.23456792E8
```

When two values are combined with a binary operator (such as `n + f` where `n` is an integer and `f` is a floating-point value), both operands are converted to a common type before the operation is carried out.

- If either of the operands is of type `double`, the other one will be converted to a `double`.

- Otherwise, if either of the operands is of type `float`, the other one will be converted to a `float`.

- Otherwise, if either of the operands is of type `long`, the other one will be converted to a `long`.

- Otherwise, both operands will be converted to an `int`.

## Casts

In the previous section, you saw that `int` values are automatically converted to `double` when necessary. On the other hand, there are obviously times when you want to consider a `double` as an integer. Numeric conversions are possible in Java, but of course information may be lost. Conversions in which loss of information is possible are done by means of _casts_. The syntax for casting is to give the target type in parentheses, followed by the variable name. For example:

```Java
double x = 9.997;
int nx = (int) x;
```

Now, the variable `nx` has a value of `9` because casting a floating-point value to an integer discards the fractional part.

If you want to `round` a floating-point number to the _nearest_ integer (which in most cases is a more useful operation), use the `Math.round` method:

```Java
double x = 9.997;
int nx = (int) Math.round(x);
```

Now the variable `nx` has the value `10`. You still need to use the cast `(int)` when you call `round`. The reason is that the return value of the `round` method is a `long`, and a `long` can only be assigned to an `int` with an explicit cast because there is the possibility of information loss.

- **CAUTION**: If you try to cast a number of one type to another that is out of range for the target type, the result will be a truncated number that has a different value. For example, `(byte) 300` is actually `44`.

