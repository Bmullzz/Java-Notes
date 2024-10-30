# Enumerated Types and Strings

- Enumerated Types
- Strings
- Substrings
- Concatenation
- Strings are Immutable
- Testing Strings for Equality
- Empty and Null Strings
- Code Points and Code Units
- The String API
- Building Strings

## Enumerated Types

Sometimes, a variable should only hold a restricted set of values. For example, you may sell clothes or pizza in four sizes: small, medium, large, and extra large. Of course, you could encode these sizes as integers `1`, `2`, `3`, `4` or characters `S`, `M`, `L`, and `X`. But that is an error-prone setup. It is too easy for a variable to hold a wrong value (such as `0` or `m`).

You can define your own _enumerated type_ whenever such a situation arises. An enumerated type has a finite number of named values. For example:

```Java
enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE };
```

Now you can declare variables of this type:

```Java
Size s = Size.MEDIUM;
```

A variable of type `Size` can hold only one of the values listed in the type declaration, or the special value `null` that indicates that the variable is not set to any value at all.

## Strings

Conceptually, Java strings are sequences of Unicode characters. For example, the string "Java\u2122" consists of the five Unicode characters J, a, v, a, and &trade;. Java does not have a built-in string type. Instead, the standard Java library contains a pre-defined class called, naturally enough, `String`. Each quoted string is an instance of the `String` class:

```Java
String e = ""; // an empty string
String greeting = "Hello";
```

## Substrings

You can extract a substring from a larger string with the `substring` method of the `String` class. For example,

```Java
String greeting = "Hello";
String s = greeting.substring(0, 3);
```

creates a string consisting of the characters `"Hel"`.

The second parameter of `substring` is the first position that you _do not_ want to copy. In our case, we want to copy positions `0`, `1`, and `2` (from position `0` to position `2` inclusive). As `substring` counts it, this means from position `0` inclusive to position `3` _exclusive_.

There is one advantage to the way `substring` works: Computing the length of the substring is easy. The string `s.substring(a, b)` always has length `b - a`. For example, the substring `"Hel"` has a length `3 - 0 = 3`.

## Concatenation

Java, like most programming languages, allows you to use `+` to join (concatenate) two strings.

```Java
String expletive = "Expletive";
String PG13 = "deleted";
String message = expletive + PG13;
```

The preceding code set the variable `message` to the string `"Expletivedeleted"`. (Note the lack of a space between the words: The `+` operator joins two strings in the order received, _exactly_ as they are given.)

When you concatenate a string with a value that is not a string, the latter is converted to a string. (As you will see in Chapter 5, every Java object can be converted to a string.) For example,

```Java
int age = 13;
String rating = "PG" + age;
```

sets the `rating` to the string `"PG13"`.

This feature is commonly used in output statements. For example,

```Java
System.out.println("The answer is " + answer);
```

is perfectly acceptable and prints what you would expect (and with the correct spacing because of the space after the word `is`).

If you need to put multiple strings together, separated by a delimiter, use the static `join` method:

```Java
String all = String.join(" / ", "S", "M", "L", "XL");
    // all is the string "S / M / L / XL"
```

## Strings are Immutable

The `String` class gives no methods that let you _change_ a character in an existing string. If you want to turn `greeting` into `"Help!"`, you cannot directly change the last positions of `greeting` into `p` and `!`. IF you are a C programmer, this will make you feel pretty helpless. How are we going to modify the string? In Java, it is quite easy: Cacatenate the substring that you want to keep with the characters that you want to replace.

```Java
greeting = greeting.substring(0, 3) + "p!";
```

This declaration changes the current value of the `greeting` variable to "Help!".

Since you cannot change the individual characters in a Java string, the documentation refers to the objects of the `String` class as _immutable_. Just as the number `3` is always `3`, the string `"Hello"` will always contain the code-unit sequence for the characters `H`, `e`, `l`, `l`, `o`. You cannot change these values. Yet you can, as you just saw, change the contents of the string _variable_ `greeting` and make it refer to a different string, just as you can make a numeric variable currently holding the value of `3` hold the value `4`.

Isn't that a lot less efficient? IT would seem simpler to change the code units than to build up a whole new string from scratch. Well, yes and no. Indeed, it isn't efficient to generate a new string that holds the concatenation of `"Hel"` and `"p!"`. But immutable strings have one great advantage: The compiler can arrange that strings are _shared_.

To understand how this works, think of the various strings as sitting in a common pool. If you copy a string variable, both the original and copy share the same characters.

Overall, the designers of Java decided that the efficiency of sharing outweighs the inefficiency of string editing by extracting substrings and concatenating. Look at your own programs; we suspect that most of the time, you don't change strings -- you just compare them. (There is one common exception -- assembling strings from individual characters for from shorter strings that come from a keyboard or file. For these situations, Java provides a separate class called `Stringbuilder`.)

## Testing Strings for Equality

To test whether two strings are equal, use the `equals` method. The expression

```Java
s.equals(t)
```

returns `true` if the strings `s` and `t` are equal, `false` otherwise. Note that `s` and `t` can be string _variables_ ot string _literals_. For example, the expression

```Java
"Hello".equals(greeting)
```

is perfectly legal. To test whether two strings are identical except for the upper/lowercase letter distinction, use the `equalsIgnoreCase` method.

```Java
"Hello".equalsIgnoreCase("hello")
```

Do _not_ use the operator `==` to test whether two strings are equal! It only determines whether or not the strings are stored in the same location. Sure, if strings are in the same location, they must be equal. But it is entirely possible to store multiple copies of identical strings in different places.

```Java
String greeting = "Hello"; // initialize greeting to a string
if (greeting == "Hello") ...
    // probably true
if (greeting.substring(0, 3) == "Hel") ...
    // probably false
```

If the virtual machine always arranges for equal strings to be shared, then you could use the `==` operator for testing equality. But only string _literals_ are shared, not strings that are the result of operations like `+` or `substring`. Therefore, _never_ use `==` to compare strings lest you end up with a program with the worst kind of bug -- an intermittent one that seems to occur randomly.

## Empty an Null Strings

The empty string `""` is a string of length `0`, You can test whether a string is empty by calling

```Java
if (str.length() == 0)
```

or 

```Java
if (str.equals(""))
```

An empty string is a Java object which holds the string length (namely `0`) and an empty contents. However, a `String` variable can also hold a special value, called `null`, that indicates that no object is currently associated with the variable. (See chapter 4 for more information about `null`.) To test whether a string is `null`, use the condition

```Java
if (str != null && str.length() != 0)
```

You need to test that `str` is not `null` first. As you will see in chapter 4, it is an error to invoke a method on a `null` value.

## Code Points and Code Units

