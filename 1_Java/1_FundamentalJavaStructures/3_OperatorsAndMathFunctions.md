# Operators, Mathematical Functions, and Constants

# Operators

Java uses the the usual operators for addition, subtraction, multiplication, division, and modulus.

- `+`: addition

- `-`: subtraction

- `*`: multiplication

- `/`: division
    - The `/` operator denotes _integer_ division if **both** arguments are integers, and _floating-point_ division otherwise.

- `%`: modulus (Integer remainder)
    - Integer remainder (sometimes called _modulus_) is denoted by `%`.
    - For example, `15 / 2` is `7`, `15 % 2` is `1`, and `15.0 / 2` is `7.5`.

Note that _integer division_ by `0` raises an **exception**, whereas _floating-point division_ by `0` yields an **infinite** or **NaN** result.

- NOTE: One of the stated goals of the java programming language is _portability_. A computation should yield the same results no matter which virtual machine executes it. For arithmetic computations with floating-point numbers, it is surprisingly difficult to achieve this portability. The `double` type uses 64 bits to store a numeric value, but some processors use 80-bit floating-point registers. These registers yield added precision in intermediate steps of a computation. For example, consider the following computation:

```Java
double w = x * y / z;
```

- Many Intel processors compute `x * y`, leave the result in an 80-bit register, then divide by `z`, and finally truncate the results back to 64 bits. That can yield a more accurate result, and it can avoid exponent overflow. But the result may be _different_ from a computation that uses 64 bits throughout. For that reason, the initial specification of the **Java Virtual Machine** mandated that all intermediate computations must be truncated. The numeric community hated it. Not only can the truncated computations cause overflow, they are actually _slower_ than the more precise computations because the truncation operations take time. For that reason, the Java programming language was updated to recognize the conflicting demands for optimum performance and perfect reproducibility. By default, virtual machine designers are now permitted to use extended precision for intermediate computations. However, methods tagged with the `strictfp` keyword must use strict floating-point operations that yield reprodicible results. For example, you can tag `main` as

```Java
public static strictfp void main(String[] args)
```

- Then all instructions inside the `main` method will use strict floating-point computations.

- Theses _gory_ details are very much tied to intel processors. In default mode, intermediate results are allowed to use an extended exponent, not not an extende mantissa. (The Intel chips support truncation of the mantissa without loss of performance.) therefore, the only difference between the default and strict modes is that strict computations may overflow when default computations don't.

- Don't worry though. Floating-point overflow isn't a problem that one ecounters for most common programs. It would be _very rare_ for you to need to use the `strictfp` keyword.

## Mathematical Functions and Constants

The `Math` class contains an assortment of mathematical functions that you may occasionally need, depending on the kind of programming that you do.

To take the square root of a number, use the `sqrt` method:

```Java
double x = 4;
double y = Math.sqrt(x);
System.out.println(y); // prints 2.0
```

- NOTE: There is a subtle difference between the `println` method and the `sqrt` method. The `println` method operates in the `System.out` object. But the `sqrt` method in the `Math` class does not operate on any object. Such a method is called a _static_ method. 

The Java programming language has no operator for raising a quantity to a power: You must use the `pow` method in the `Math` class. The statement

```Java
double y = Math.pow(x, a);
```

sets `y` to be `x` raised to the power `a(x^a)`. The `pow` method's parameters are both of a type `double`, and it returns a `double` as well.

The `floorMod` method aims to solve a long-standing problem with integer remainders. Consider the expression `n % 2`. Everyone knows that this is `0` if `n` is even and `1` if `n` is odd. Except, of course, when `n` is negative. Then it is `-1`. Why? When the first computers were built, someone had to make rules for how integer division and remainder should work for negative operands. Mathematicians had known the optimal (or "Euclidean") rule for a few hundred years: always leave the remainder `>= 0`. But, rather than open a math textbook, those pioneers came up with rules that seemed reasonable but are actually inconvenient.

Consider this problem. You compute the position of the hour hand of a clock. An adjustment is applied, and you want to normalize to a number between `0` and `11`. That is easy: `(position + adjustment) % 12`. But what if the adjustment is negative? Then you might get a negative number. So you have to introduce a branch, or use `((position + adjustment) % 12 +12) % 12. Either way, it is a hassle.

The `floorMod` method makes it easier: `floorMod(position + adjustment, 12)` always yields a value between `0` and `11`. (Unfortunately, `floorMod` gives negative results for negative divisors, but the situation doesn't often occur in practice.)

The `Math` class supplies the usual trigonometric functions:

- `Math.sin`
- `Math.cos`
- `Math.tan`
- `Math.atan`
- `Math.atan2`

and the exponential function with its inverse, the natural logarithm, as well as the decimal logarithm:

- `Math.exp`
- `Math.log`
- `Math.log10`

Finally, two constants denote the closest possible approximations to the mathematical constants &pi; and `e`:

- `Math.PI`
- `Math.E`

TIP: You can avoid the `Math` prefix for mathematical methods and constants by adding the following line at the top of your file:

```Java
import static java.lang.Math.*;
```

For example:

```Java
System.out.println("The square root of \u03C0 is " + sprt(PI));
```

- NOTE: The methods in the `Math` class use the routines in the computer's floating-point unit for fastest performance. If completely predictable results are more important than performance, use the `StrictMath` class instead. It implements the algorithms from the "Freely Distributable Math Library" `fdlibm`, guaranteeing identical results on all platforms. See www.netlib.org/fdlibm for the source of these algorithms. 