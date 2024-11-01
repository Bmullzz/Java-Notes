# Inputs and Outputs

- [Input and Output](#input-and-output)
- [Reading Input](#reading-input)
- [Formatting Output](#formatting-output)
- [File Input and Output]()

## Input and Output

To make our programs more interesting, we want to accept input and properly format the program output. Of course, modern programs use a GUI for collecting user input. However, programming such an interface requires more tools and techniques than we have at our disposal at this time. Our first order of business is to become more familiar with the JAva programming language, so we make do with the humble console for input and output for now. 

## Reading Input

You saw that it is easy to print output to the "standard output stream" (that is, the console window) just by calling `System.out.println`. Reading from the "standard input stream" `System.in` isn't quite as simple. To read the console input, you first construct a `Scanner` that is attached to `System.in`:

```Java
Scanner in = new Scanner(System.in);
```

(We discuss constructors and the new operator in detail later on)

Now you can use the various methods of the `Scanner` class to read input. For example, the `nextLine` method reads a line of input.

```Java
System.out.print("What is your name? ");
String name = in.nextLine();
```

Here, we use the `nextLine` method because the input might contain spaces. To read a single word (delimited by whitespace), call

```Java
String firstName = in.next();
```

To read an integer, use the `nextInt` method.

```Java
System.out.print("How old are you? ");
int age = in.nextInt();
```

Similarly, the `nextDouble` method reads the next floating-point number.

The program in [Listing 3.2](#listing-32) asks for the user's name and age then prints a message like

```console
Hello, Cay. Next year, you'll be 57
```

Finally, note the line

```java
import java.util.*;
```

at the beginning of the program. The `Scanner` class is defined in the `java.util` package. Whenever you use a class that is not defined in the basic `java.lang` package, you need to use an `import` directive. We look at pakcages and `import directives in more detail in the next chapter.

### Listing 3.2
`InputTest/InputTest.java`

```Java
import java.util.*;

/**
 * This program demonstrates console input.
 * @version 1.10 2004-02-10
 * @author Cay Horstman
 **/
public class InputTest {

    public static void main(String[] args) {

        Scanner in = new Scanner(System.in);

        // get first input
        System.out.print("What is your name? ");
        String name = in.nextLine();

        // get second input
        System.out.print("How old are you? ");
        int age = in.nextInt();

        // display output on console
        System.out.println("Hello, " + name + ". Next year, you'll be " + (age + 1));
    }
}
```

NOTE: The `Scanner`class is not suitable for reading a password from a console since the input is plainly visible to anyone. Java SE 6 introduces a `Console` class specifically for the purpose. To read a password, use the following code:

```Java
Console cons = System.console();
String username = cons.readLine("User name: ");
char[] password = cons.readPassword("Password: ");
```

For security reasons, the password is returned in an array of characters rather than a string. After you are done processing the password, you should immediately overwrite the array elements with a filler value. (Array processing is discussed in the Arrays document.)

Input processing with a `Console` object is not as convenient as with a `Scanner`. You must read the input line a line at a time. There are no methods for reading individual words or numbers.

- `java.util.Scanner` - 5.0

    - `Scanner(InputStream in)`

        - constructs a `Scanner` object from the given input stream.

    - `String nextLine()`

        - reads the nest line of input.

    - `String next()`

        - reads the next word of input (delimited by whitespace).

    - `int nextInt()`
    - `double nextDouble()`

        - reads and converts the next character sequence that represents an integer or floating-point number.

    - `boolean hasNext()`

        - tests whether there is another word in the input.

    - `boolean hasNextInt()`
    - `boolean hasNextDouble()`

        - tests whether the next character sequence represents an integer or floating-point number.


- `java.lang.System` - 1.0

    - `static Console console()` - 6

        - returns a `Console` object for interacting with the user through a console window if such an interaction is possible, `null` otherwise. A `Console object is available for any program that is launched in a console window. Otherwise, the availability of the system is dependent.


- `java.io.Console` - 6

    - `static char[] readPassword(String prompt, Object... args)`
    - `static String readLine(String prompt, Object... args)`

        - displays the prompt and reads the user input until the end of the input line. The `args` parameters cna be used to supply formatting arguments, as described in the next section.

## Formatting Output

You can print a number `x` to the console with the statement `System.out.print(x)`. That command will print `x` witht he maximum number of nonzero digits for that type. For example,

```Java
double x = 10000.0 / 3.0;
System.out.print(x);
```

Prints

```Java
3333.3333333333335
```

That is a problem if you want to display, for example, dollars and cents.

In early versions of Java, formatting numbers was a bit of a hassle. Fortunately, Java SE 5.0 brought back the venerable `printf` method from the C library. For example, the call

```Java
System.out.printf("%8.2f", x);
```

prints `x` with a _field width_ of 8 characters and a _precision_ of 2 characters. That is, the printout contains a leading space and the seven characters

```Java
3333.33
```

You can supply multiple parameters to `printf`. For example:

```Java
System.out.printf("Hello, %s. Next year, you'll be %d", name, age);
```

Each of the _format specifiers_ that start with a % character is replaced with the corresponding argument. The _conversion character_ that ends a format specifier indicates the type of value to be formatted: `f` is a floating-point number, `s` is a string, and `d` is a decimal integer. The following table shows all conversion characters.

Conversions for `printf`

| Conversion Character | Type                       | Example   |
| ---                  | ---                        | ---       |
| `d`                  | Decimal Integer            | `159`     |
| `x`                  | Hexadecimal Integer        | `9f`      |
| `o`                  | Octal Integer              | `237`     |
| `f`                  | Fixed-point floating-point | `15.9`    |
| `e`                  | Exponential floating-point | `1.59e+01`|
| `g` | General floating-point (the shorter of `e` and `f`) | `-` |
| `a`                  | Hexadecimal floating-point | `0x1.fccdp3`|
| `s`                  | String                     | `Hello`   |
| `c`                  | Character                  | `H`       |
| `b`                  | `boolean`                  | `true`    |
| `h`                  | Hash code                  | `4262b2`  |
| _tx_ or _Tx_         | Date and time (T forces uppercase) | Obsolete, use the `java.time` classes instead |
| `%`                  | The percent symbol         | `%`       |
| `n`       | The platform-dependent line separator | `-`       |

In addition, you can specify _flags_ that control the appearance of the formatted output. The table below shows all flags. For example, the comma flag adds group separators. That is,

```Java
System.out.printf("%,.2f", 10000.0 / 3.0);
```

prints

```Java
3,333.33
```

You can use multiple flags, for example `"%,(.2f"` to use group separators and enclose negative numbers in parentheses.

- NOTE: You can use the `s` conversion to format arbitrary objects. If an arbitrary object implements the `Formattable` interface, the object's `formatTo` method is invoked. Otherwise, the `toString` method is invoked to turn the object into a string. We discuss the `toString` method and interfaces later sections.

You can use the static `String.format` method to create a formatted string without printing it:

```Java
String message = String.format("Hello, %s. Next year, you'll be %d", name, age);
```

Flags for `printf`

| Flag      | Purpose                                        | Example      |
| ---       | ---                                            | ---          |
| `+`       | Prints sign for positive and negative numbers. | `+3333.33`   |
| `space`   | Adds a space before positive numbers.          |`. 3333.33 .` |
| `0`       | Adds leading zeroes.                           | `003333.33`  |
| `-`       | Left-justifies field.                          | `.3333.33 .` |
| `(`       | Encloses negative numbers in parentheses.      | `(3333.33)`  |
| `,`       | Adds group separators.                         | `3,333.33`   |
|`#` (for `f`format) | Always includes a decimal point.      | `3,333.`     |
|`#` (for `x` or `o`)| Adds `0x` or `0` prefix               | `0xcafe`     |
| `$`| Specifies the index of the argument to be formatted; for example, `%1$d` `%1$x` prints the first argument in decimal and hexadecimal.        | `159` `9F`   |
| `<`| Formats the same value as the previous specification; for example, `%d` `%<x` prints the same number in decimal and hexadecimal.           | `159` `9F`   |

In the interest of completeness, we briefly discuss the date and time formatting options of the `printf` method. For new code, you should use the methods of the `java.time` package. But you may encounter the `Date` class and the associated formatting options in legacy code. The format consists of two letters, starting with `t` and ending in one of the letters in the table below. For example,

```Java
System.out.printf("%tc", newDate());
```

prints the current date and time in the format

```Java
Mon Feb 09 18:05:19 PST 2015
```

As you can see in the table below, some of th eformats yield only a part of a given date -- for example, just he day or just the month. It would be a bit silly if you had to supply the date multiple times to format each part. For that reason, a format string can indicate the _index_ of the argument to be formatted. The index must immediately follow the `%`, and it must be terminated by a `$`. For example,

```Java
System.out.printf("%1$s %2$tB %2$te, %2$tY", "Due date:", new Date());
```
prints

```Java
Due Date: February 9, 2015
```

Alternatively, you can use the `<` flag. It indicates that the same argument as in the preceding format specification should be used again. That is, the statement 

```Java
System.out.printf("%s %tB %<te, %<tY", "Due date:", new Date());
```

yields the same output as the preceding statement.

Date and Time Conversion Characters:

| Conversion Character           | Type        | Example |
| ---                            | ---         | ---     |
| `c` | Complete date and time | `Due Date: February 9, 2015` |
| `F` | ISO 8601 date          | `2015-02-09` |
| `D` | U.S. formatted date (month/day/year) | `02/09/2015` |
| `T` | 24-hour time           | `18:05:19` |
| `r` | 12-hour time   | `06:05:19 pm` |
| `R` | 24-hour time, no seconds | `18:05` |
| `Y` | Four-digit year (with leading zeroes) | `2015` |
| `y` | Last two digits of the year (with leading zeroes) | `15` |
| `C` | First two digits of the year (with leading zeroes) | `20`|
| `B` | Full month name | `February` |
| `b` or `h` | Abbreviated month name | `Feb` |
| `m` | Two-digit month (with leading zeroes) | `02` |
| `d` | Two-digit day (with leading zeroes) | `09` |
| `e` | Two-digit dat (without leading zeroes) | `9` |
| `A` | Full weekday name | `Monday` |
| `a` | Abbreviated weekday name | `mon` |
| `j` | Three-digit day of the year (with leading zeroes), between `001` and `366` | `069` |
| `h` | Two-digit hour (with leading zeroes), between `00` and `23` | `18` |
| `k` | Two-digit hour (without leading zeroes), between `0` and `23` | `18` |
| `I` | Two-digit hour (with leading zeroes), between `01` and `12` | `06` |
| `l` | Two-digit hour (without leading zeroes), between `1` and `12` | `6` |
| `M` | Two-digit minutes (with leading zeroes) | `05` |
| `S` | Two-digit seconds (with leading zeroes) | `19` |
| `L` | Three-digit milliseconds (with leading zeroes) | `047` |
| `N` | Nine-digit nanoseconds (with leading zeroes) | `047000000` |
| `p` | Morning or afternoon marker | `pm` |
| `z` | RFC 822 numeric offset from GMT | `-0800` |
| `Z` | Time zone | `PST` |
| `s` | Seconds since `1970-01-01 00:00:00 GMT` | `107888431` |
| `Q` | Milliseconds since `1970-01-01 00:00:00 GMT` | `1078884319047` |

- **CAUTION**: Argument index values start with `1`, not with `0:` `%1$...` formats the first argument. This avoids confusion with the `0` flag.

