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

