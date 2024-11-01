# Java Data Types

- [Integer Types](#integer-types)
- [Floating-Point Types](#floating-point-types)
- [The `char` Type](#the-char-type)
- [Unicode and the `char` Type](#unicode-and-the-char-type)
- [The `boolean` type](#the-boolean-type)

Java is a _strongly typed language_.  This means that every variable **must have a declared type**.  There are **eight** _primitive types_ in Java.  Four are integer types, 2 are floating-point number types, one is the character type `char` (used for code units in the Unicode encoding scheme), and one is a `boolean` type for truth values.

- **8 data types** in _total_:
    - 4 integer types:
        - `int`
        - `short`
        - `long`
        - `byte`

    - 2 floating-point types:
        - `float`
        - `double`

    - 1 `char` type:
        - `char`

    - 1 `boolean` type:
        - `boolean`

## Integer Types

The integer types are numbers without fractional parts. Negative values are allowed.  Java provides 4 integer types.

| Type   | Storage Requirement | Range (Inclusive)           |
| ---    | ---                 | ---                         |
| `int`  | 4 bytes             | `-2,147,483,648` to `2,147,483,647`
| `short`| 2 bytes             | `-32,768` to `32,767`       |
| `long` | 8 bytes             | `-9,223,372,036,854,775,808` to `9,223,372,036,854,775,807` |
| `byte` | 1 byte              | `-128` to `127`             |

- `int`
    - For most situations, `int` is the most practical type to use, unless you want represent the population of Earth.

- `long`
    - For dealing with large numbers > 2.147 billion, you will need to use `long`.

- `byte` and `short`
    - These two data types are mainly intended for specialized applications, such as low-level file handling, or for large arrays when storage is at a premium.

In Java, the ranges of integer types do not depend on the machine or operating system on which you are running your code. In contrast, C and C++ use the most efficient integer type for each processor. As a result, a C program that runs on 32-bit processor may exhibit integer overflow on a 16-bit system.  Since Java programs must run with the same results on all machines, the ranges for various types are fixed.

`Long` integer numbers have a suffix `L` or `1` (for example, `400000000L`). Hexadecimal numbers have a prefix `0x` or `0X` (for example `0xCAFE`). Octal numbers have a prefix `0` (for example, `010` is `8'). It is recommend to NOT use octal constants.

- NOTE: Java does not havea any `unsigned` versions of `int`, `long`, `short`, or `byte`

## Floating-Point Types

The floating-point types denote numbers with fractional parts.

| Type     | Storage Requirement | Range    |
| ---      | ---                 | ---      |
| `float`  | 4 bytes             | Approximately &pm;3.40282347E (6-7 significant decimal digits)  |
| `double` | 8 bytes             | Approximately &pm;1.79769313486231570E+308 (15 significant decimal digits) |

- `float`
    - The precision of `float` is NOT sufficient for many situations.
    - Use `float` values only when you work with a library that requires them, or when you need to store a very large number of them.
    - Number types of `float` have a suffix `F` or `f` (for example, `3.14F`).
    - Floating-point numbers _without_ an `F` suffix (such as `3.14`) are _always_ considered to be of type `double`.
    - You can optionally supply the `D` or `d` suffix (for example, `3.14D`).

- `double`
    - The name `double` refers to the fact that these numbers have twice the precision of the `float` type or _double-precision_.

All floating point computations follow the [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) specification. In particular, there are three special floating-point values to denote _overflows_ and _errors_:

- Positive infinity
- Negative infinity
- NaN (not a number)

For example, the result of dividing a positive number by `0` is positive infinity.  Computing `0/0` or the square root of a negative number yields **NaN**.

- NOTE: The constants `Double.POSITIVE_INFINITY`, `Double.NEGATIVE_INFINITY`, and `Double.Nan` (as well as corresponding `Float` constants) represent these special values, but they are rarely used in practice.  In particular, you cannot test

```java
if (x == Double.Nan) // is never true
```

- to check whether a particular result equals `Double,NaN`.  All "not a number" values are considered distinct.  However, you can use the `Double.isNaN` method:

```java
if (Double.isNaN(x)) check whether x is "not a number"
```

- **CAUTION**: Floating-point numbers are **NOT** suitable for _financial calculations_ in which roundoff errors cannot be tolerated. For example, the command `System.out.println(2.0 - 1.1)` prints `0.8999999999999999`, not `0.9` as you would expect. Such roundoff errors are caused by the fact that floating point numbers are represented in the binary number system.  There is no precise binary representation of the fraction `1/10`, just as there is no accurate representation of the fraction `1/3` in the decimal system.  If you need precise numerical computations without roundoff errors, use the `BigDecimal` class.

## The `char` Type

The `char` type was origionally intended to describe individual characters.  However, this is no longer the case. Nowadays, some Unicode characters can be described with _one_ `char` value, and other Unicode characters require _two_ `char` values.

Literal values of type `char` are enclosed in single quotes. For example, `'A'` is a character constant with value 65.  It is **different** from `"A"`, a string containing a single character. Values of type `char`can be expressed as hexadecimal values that run from `\u0000` to `\uFFFF`. For example, `\u2122` is the trademark symbol (&trade;) and `u\03C0` is the Greek letter pi (&pi;).

Besides the \u escape sequences, there are several escape sequences for special characters, as shown below.  You can use these escape sequences inside quoted character literals and strings, such as `\u2122` or `"Hello\n"`. The `\u` escape sequence (but none of the other escape sequences) can be used _outside_ quoted character constants and strings. For example,

```java
public static void main(String\u005B\u005D args)
```

is perfectly legal - `\u005B` and `\u005D` are the encodings for `[` and `]`.

Escape sequences for Special Characters:

| Escape Sequence | Name            | Unicode Value |
| ---             | ---             | ---           |
| `\b`            | Backpace        | `\u0008`      |
| `\t`            | Tab             | `\u0009`      |
| `\n`            | Linefeed        | `\u000a`      |
| `\r`            | Carriage return | `\u000d`      |
| `\"`            | Double quote    | `\u0022`      |
| `\'`            | Single quote    | `\u0027`      |
| `\\`            | Backslash       | `\u005c`      |

- **CAUTION**: Unicode escape sequences are processed before the code is parsed. For example, "\u0022+\u0022" is _not_ a string consisting of a plus sign surrounded by quotation marks (U+0022). Instead, the `\u0022` are converted into `"` before parsing, yielding `""+""`, or an empty string.

    - Even more insidiously, you must beware of `\u` inside comments. The comment,

    ```java
    // \u000A is a newline
    ```

    - yields a syntax error since `\u000A` is replaced with a newline when the program is read. Similarly, a comment,

    ```java
    // Look inside c:\users
    ```

    -yields a syntax error because `\u` is not followed by four hex digits.

## Unicode and the `char` Type

To fully understand the `char` type, you have to know about the Unicode encoding scheme. Unicode was invented to overcome the limitations of traditional character encoding schemes. Before Unicode, there were many different standards: ASCII in the United States, ISO 8859-1 for Western European languages, KOI-8 for Russian, GB18030 and BIG-5 for Chinese, and so on. This caused two problems. Aparticular code value corresponds to different letters in the different encoding schemes. Moreover, the encodings for languages with large character sets have variable length: Some common characters are encoded as single bytes, others require two or more bytes.

Unicode was designed to solve these problems. When the unification effort started in the 1980's, a fixed 2-byte code was more than sufficient to encode all characters used in all languages around the world, with room to spare for future expansion -- or so everyone thought at the time. In 1991, Unicode 1.0 was released, using slightly less than half of the available 65,536 code values. Java was designed from the ground up to use 16-bit Unicode characters, which was a major advance over other programming languages that used 8-bit characters.

Unfortunately, over time, the inevitable happened. Unicode grew beyond 65,536 characters, primarily due to the addition of a very large set of ideographs used for Chinese, Japanese, and Korean. Now, the 16-bit `char` type is insufficient to describe all Unicode characters.

We need a bit of terminology to explain how this problem is resolved in Java, beginning with Java SE 5.0. A _code point_ is a code value that is associated with a character in an encoding scheme. In the Unicode standard, code points are written in hexadecimal and prefixed with `U+`, such as `U+0041` for the code point of the Latin letter `A`. Unicode has code points that are grouped into 17 _code planes_. The first code plane, called the _basic multilingual plane_, consists of the "classic" Unicode characters with code points `U+0000` to `U+FFFF`. Sixteen additional planes with code points `U+10000` to `U+10FFFF`, hold the _supplementary characters_.

The UTF-16 encoding represents all Unicode points in a variable-lengthcode. The characters in the basic multilingual plane are represented as 16-bit values, called _code units_. The supplementary characters are encoded as consecutive pairs of code units. Each of the values in such an encoding pair falls into a range of 2048 unused values of the basic multilingual plane, called the _surrogates area_ (`U+D800` to `U+DBFF` for the first code unit, `U+DC00` to `U+DFFF` for the second code unit). This is rather clever, because you can immediately tell whether a code unit encodes a single character or it is the first or second part of a supplementary character. For example, &Oopf; (the mathematical symbol for the set of octonians) has the code point `U+1D546` and is encoded by the two code units `U+D845` and `U+DD46`. (See https://en.wikipedia.org/wiki/UTF-16 for a description of the encoding algorithm.)

In Java, the `char` type describes a _code unit_ in the UTF-16 encoding.

It is _strongly_ reccomended to not use the `char` type in your programs unless you are actually manipulating UTF-16 code units. You are almost _always_ better off treating strings as _abstract data types_.

## The `boolean` type

The `boolean` type has two values, `false` and `true`. It is used for evaluating logical conditions. You **cannot** convert between integers and `boolean` values.

- NOTE: In **C++**, numbers and even pointers can be used in place of `boolean` values. The value `0` is equivalent to the `bool` value `false`, and a nonzero value is equivalent to `true`. This is _not_ the case in Java. Thus, Java programmers are shielded from accidents such as

```C++
if (x = 0) //oops... meant x == 0
```

- In C++, this test compiles and runs, always evaluating to `false`. In Java, the test does not compile because the integer expression `x= 0` cannot be converted to a `boolean` value.

