# Arrays

- [The "for each" Loop](#the-for-each-loop)
- [Array Initalizers and Anonymous Arrays](#array-initializers-and-anonymous-arrays)
- [Array Copying](#array-copying)
- [Command-Line Parameters]()
- [Array Sorting]()
- [Multidimensional Arrays]()
- [Ragged Arrays]()

An array is a data structure that stores a collection of values of the same type. You access each individual value through integer _index_. For example, if `a` is an array of integers, then `a[i]` is the `i`th integer in the array.

Declare an array variable by specifying the array type - which is the element type followed by `[]` - and the array variable name. For example, here is the declaration of an array `a` of integers:

```Java
int[] a;
```

However, this statement only declares the cariable `a`. It does not yet initialize `a` with an actual array. Use the `new` operator to create the array.

```Java
int[] a = new int[100];
```

This statement declares and initializes an array of `100` integers. 

The array length need not be a constant: `new int[n]` creates an array of length `n`.

- **NOTE**: You can define an array variable either as

    ```Java
    int[] a;
    ```

- or as

    ```Java
    int a[];
    ```

- Most Java programmers prefer the former style because it neatly separates the type `int[]` (integer array) from the variable name.

The array elements are numbers are _numbered from `0` to `99`_ (and not `1` to `100`). Once the array is created, you can fill the elements in an array, for example, by using a loop:

```Java
int[] a = new int[100];
for (int i = 0; i < 100; i++) {
    a[i] = i; // fills the array with numbers 0 to 99
}
```

When you create an array of numbers, all elements are initialized with zero. Arrays of `boolean` are initialized with `false`. Arrays of objects are initialized with the special value `null`, which indicates that they do not (yet) hold any objects. This can be surprising for beginners. For example,

```Java
String[] names = new String[10];
```

creates an array of ten strings, all of which are `null`. If you want the array to hold empty strings, you must supply them:

```Java
for (int i = 0; i < 10; i++) names[i] = "";
```

- **CAUTION**: If you construct an array with 100 elements and then try to access the element `a[100]` (or any other index outside the reange from `0` to `99`), your program will terminate with an "aray index out of bounds" exception.

To find the number of elements of an array, use `array.length`. For example:

```Java
for (int i = 0; i < a.length; i++) {
    System.out.println(a[i]);
}
```

Once you create an array, you cannot change its size (although you can, of course, change an individual array element). If you frequently need to expand the size of an array while your program is running, you should use a different data structure called an _array list_.

## The "for each" Loop

Java has a powerful looping construct that allows you to loop through each element in an array (or any other collection of elements) without having to fus with index values.

The _enhanced_ `for` loop

```Java
for (variable : collection) statement
```

sets the given variable to each element of the collection and then executes the statement (which of course, may be a block). The _collection_ expression must be an array or an object of a class that implements the `Iterable` interface, such as `ArrayList`. We discuss array lists in Chapter 5 and the `Iterable` interface in Chapter 9.

For example,

```Java
for (int element : a)
    System.out.println(element);
```

prints each element of the array `a` on a separate line.

You should read this loop as "for each `element` in `a`". The designers of the Java language considered using keywords, such as `foreach` and `in`. But this loop was a late addition to the Java language, and in th end nobody wanted to break the old code that already contained methods or variables with these names (such as `System.in`).

Of course, you could achieve the same effect with a traditional `for` loop:

```Java
for (int i = 0; i < a.length; i++) {
    System.out.println(a[i]);
}
```

However, the "for each" loop is more concise and less error-prone, as you don't have to worry about those pesky start and end values.

- **NOTE**: the loop variable of the "for each" loop traverses the _elements_ of the array, not the index values.

The "for each" loop is pleasant improvement over the traditional loop if you need to process all elements in a collection. However, there are still plenty of opportunities to use the traditional `for` loop. For example, you might not want to traverse the entire collection, or you may need the index value inside the loop. 

- **TIP**: There is an even easier way to print all values of an array, using the `toString` method of the `Arrays` class. The call `Arrays.toString(a)` returns a string containing the array elements, enclosed in brackets and separated by commas, such as `"[2, 3, 5, 7, 11, 13]"`. To print the array, simply call

```Java
System.out.println(Arrays.toString(a));
```

## Array Initializers and Anonymous Arrays

Java has a shortcut for creating an array object and supplying initial values at the same time. Here's an example of the syntax at work:

```Java
int[] smallPrimes = { 2, 3, 5, 7, 11, 13 };
```

Notice that you do not call `new` when you use this syntax.

You can even initialize an _anonymous array_:

```Java
new int[] { 17, 19, 23, 29, 31, 37 };
```

This expression allocates a new array and fills it with the values inside the braces. It counts the number of initial values and sets the array size accordingly. You can use this syntax to reinitialize an array without creating a new variable. For example,

```Java
smallPrimes = new int[] { 17, 19, 23, 29, 31, 37 };
```

is shorthand for 

```Java
int[] anonymous = { 17, 19, 23, 29, 31, 37 };
smallPrimes = anonymous;
```

- **Note**: It is legal to have arrays of length `0`. Such an array can be useful if you write a method that computes an array result and the result happens to be empty. Construct an array of length `0` as 

```Java
new elementType[0]
```

Note that an array of length `0` is not the same as `null`.

## Array Copying

You can copy one array variable into another, but then _both variables refer to the same array_:

```Java
int[] luckyNumbers = smallPrimes;
luckyNumbers[5] = 12; // now smallPrimes[5] is also 12
```

If you want to copy all values of one array into a new array, you use the `copyOf` method in the `Arrays` class:

```Java
int[] copiedLuckyNumbers = Arrays.copyOf(luckyNumbers, luckyNumbers.length);
```

The second parameter is the length if the new array. A common use of this method is to increase the size of an array:

```Java
luckyNumbers = Arrays.copyOf(luckyNumbers, 2 * luckyNumbers.length);
```

The additional elements are filled with `0` if the array contains numbers, `false` if the array contains `boolean` values. Conversly, if the length is less than the length of the original array, only the initial values are copied.

- **C++ Note**: A Java array is quite different from a C++ array on the stack. It is, however, essentially the same as a pointer to an array allocated on the _heap_. That is,

    ```Java
    int[] a = new int[100]; // java
    ```

    is not the same as 

    ```C++
    int[100]; // C++
    ```

    but rather

    ```C++
    int* a = new int[100]; // C++
    ```

    In Java, the `[]` operator is predefined to perform _bounds checking_. Furthermore, there is no pointer arithmetic - you can't increment `a` to point to the next element in the array. 

## Command-Line Parameters

You have already seen one example of a Java array repeated quite a few times. Every Java program has a `main` method with a `String[] args` parameter. This parameter indicates that the `main` method receives an array of strings - namely, the arguments specified on the command line.

For example, consider this program:

```Java
public class Message {

    public static void main(String[] args) {

        if (args.length == 0 || args[0].equals("-h")) {
            System.out.print("Hello");
        } else if {
            System.out.print("Goodbye,");
        }
        // print the other command-line arguments
        for (int i = 1; i < args.length; i++) {
            System.out.print(" " + args[i]);
        }
        System.out.println("!");
    }
}
```

If the program is called as

```terminal
java Message -g cruel world
```

then the args array has the following contents:

```Java
args[0]: "-g"
args[1]: "cruel"
args[2]: "world"
```

The program prints the message

```Terminal
Goodbye, cruel world!
```

- **C++ Note**: In the `main` method of a Java program, the name of the program is not stored in the `args` array. For example, when you start up a program as 

    ```Terminal
    java Message -h world
    ```
    from the command line, then `args[0]` will be `"-h"` and not `"Message"` or `"java"`.

## Array Sorting

