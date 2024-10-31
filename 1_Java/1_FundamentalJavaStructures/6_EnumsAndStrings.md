# Enumerated Types and Strings

- [Enumerated Types](#enumerated-types)
- [Strings](#strings)
- [Substrings](#substrings)
- [Concatenation](#concatenation)
- [Strings are Immutable](#strings-are-immutable)
- [Testing Strings for Equality](#testing-strings-for-equality)
- [Empty and Null Strings](#empty-an-null-strings)
- [Code Points and Code Units](#code-points-and-code-units)
- [The String API](#the-string-api)
    - This section contains a list of the most useful methods in the String API.
- [Building Strings](#building-strings)

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

Java strings are implemented as sequences of `char` values. As we discussed in The `char` Type section, the `char` data type is a code unit for representing Unicode code points in the UTF-16 encodig. The most commonly used Unicode characters can be represented with a single code unit. The supplementary characters requirea pair of code units.

The `length` method yields the number of code units required for a given string in the UTF-16 encoding. For example:

```Java
String greeting = "Hello";
int n = greeting.length(); // is 5.
```

To get the true length -- that is, the number of code points -- call

```Java
int cpCount = greeting.codePointCount(0, greeting.length());
```

The call `s.charAt(n)` returns the code unit at position `n`, where `n` is between `0` and `s.length() - 1`. For example:

```Java
char first = greeting.charAt(0); // first is "H"
char last = greeting.charAt(4); // last is "o"
```

To get at the `i`th code point, use the statements

```Java
int index = greeting.offsetByCodePoints(0, i);
int cp = greeting.codePointAt(index);
```

Why are we making a fuss about code units? 
Consider the sentence

- "&Oopf; is the set of octonians"

The character &Oopf; (`U+1D546) requirestwo code units in the UTF-16 encoding. Calling

```Java
char ch = sentence.charAt(1);
```

doesn't return a space but the second code unit of &Oopf;. To avoid this problem, you should _not_ use the `char` type. It is too low-level.

If your code traverses a string, and you want to look at each code point in turn, you can use these statements:

```Java
int cp = sentence.codePointAt(i);
if (Character.isSupplementaryCodePoint(cp)) i += 2;
else i++;
```

You can move backwards with the following statements:

```Java
i--;
if (Character.isSurrogate(sentence.charAt(i))) i--;
int cp = sentence.codePointAt(i);
```

Obviously, that is quite painful. An easier way is to use the `codePoints` method that yields a "stream" of `int` values, one for each code point. (We will discuss streams later on.) You can just turn it into an array and traverse that.

```Java
int[] codePoints = str.codePoints().toArray();
```

Conversely, to turn an array of code points to a string, use a _constructor_. (We discuss constructors and the `new` operator in detail later on.)

```Java
String str = new String(codePoints, 0, codePoints.length);
```

## The String API

The `String` class in Java contains more than 50 methods. A surprisingly large number of them are suufficiently useful so that we can imagine using them frequently. The following API note summarizes the most useful of them.

- NOTE: These API notes will help you understand the Java Application Programming Interface (API). Each API note starts with the name of a class, such as `java.lang.String` (the significance is explained later on.) The class name is followed by the names, explanations, and paramter descriptions of one or more methods. 

The methods in this list does not contain all of the methods in the `String` class, but contains the most commonly used and describes them in a concise form. 

We also list the version number in which a particular class was introduced. If a method has been added later, it has a separate version number.

- `java.lang.String` - 1.0

    - `char chatAt(int index)`

        - _returns_ the _code unit_ at the specified location. You probably don't want to call this method unless you are interested in low-level code units.

    - `int codePointAt(int index)` - 5.0

        - _returns_ the _code point_ that starts at the specified location.

    - `int offsetByCodePoints(int startIndex, int cpCount)` - 5.0

        - _returns_ the _index of the code point_ that is `cpCount` code points away from the code point `startIndex`.

    - `int compareTo(String other)`

        - _returns_ a _negative value_ if the string comes _before_ `other` in **dictionary order**, a _positive value_ if the string comes _after_ `other` in **dictionary order**, or _`0`_ if the strings are _equal_.

    - `IntStream codePoints()` - 8

        - _returns_ the _code points_ of the this string as a stream. Call `toArray` to put them in an array.

    - `new String(int[] codePoints, int offset, int count)` - 5.0

        - _constructs_ a string with the `count` code points in the array starting at `offset`.

    - `boolean equals(Object other)`

        - _returns_ `true` if the string equals `other`.

    - `boolean equalsIgnoreCase(String other)`

        - _returns_ `true` if the string equals `other`, except for _upper_ / _lowercase_ distinction.

    - `boolean startsWith(String prefix)`

        - _returns_ `true` if the string _starts_ with `prefix`.

    - `boolean endsWith(String suffix)`

        - _returns_ `true` if the string _ends_ with `suffix`.

    - `int indexOf(String str)`
    - `int indexOf(String str, int fromIndex)`
    - `int indexOf(int cp)`
    - `int indexOf(int cp, int fromIndex)`

        - _returns_ the start of the _first_ substring equal to the string `str` or the code point `cp`, starting at index `0` or at `fromIndex`, or `-1` if `str` does not occur in this string.

    - `int lastIndexOf(String str)`
    - `int lastIndexOf(String str, int fromIndex)`
    - `int lastIndexOf(int cp)`
    - `int lastIndexOf(int cp, int fromIndex)`

        - _returns_ the start of the _last_ substring equal to the string `str` or the code point `cp`, starting at the end of the string or at `fromIndex`.

    - `int length()`

        - _returns_ the _number_ of code units of the string.

    - `int codePointCount(int startIndex, int endIndex)` - 5.0

        - _returns_ the _number_ of code points between `startIndex` and `endIndex - 1`.

    - `String replace(CharSequence oldString, CharSequence newString)

        - _returns_ a _new string_ that is obtained by replacing all substrings matching `oldString` in the string with the string `newString`. You can supply `String` or `StringBuilder` objects for the `CharSequence` parameters.

    - `String substring(int beginIndex)`
    - `String substring(int beginIndex, int endIndex)`

        - _returns_ a _new string_ consisting of all code units from `beginIndex` until the end of the string or until `endIndex - 1`.

    - `String toLowerCase()`
    - `String toUpperCase()`

        - _returns_ a _new string_ containing all characters in the original string, with uppercase characters converted to lowercase, or lowercase characters converted to uppercase.

    - `String trim()`

        - _returns_ a _new string_ by eliminating all leading and trailing whitespace in the original string.

    - `String join(CharSequence delimiter, CharSequence... elements)` - 8

        - _returns_ a _new string_ joining all elements with the given delimiter.

    -NOTE: In the above API notes, there are a few parameters of type `CharSequence`. This is an _interface_ type to which all strings belong. We will discuss interface types later on. For now, you just need to know that _you can pass arguments of type `String` whenever you see a `CharSequence` parameter_.

As you just saw, the `String` class has lots of methods. Furthermore, there are thousands of classes in the standard libraries, with many more methods. It is plainly impossible to remember all useful classes and methods. Therefore, it is essential that you become familiar with the online API documentation that lets you look up all classes and methods in the standard library. You can download the API documentation, or you can navigate to https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/lang/package-summary.html

## Building Strings

Occasionally, you need to build up strings from shorter strings, such as keystrokes or words from a file. It would be inefficient to use string concatenation for this purpose. Every time you concatenate strings, a new `String` object is constructed. This is time consuming and wastes memory. Using the `StringBuilder` class avoinds this problem.

Follow these steps if you need to build a string from many small pieces. First, construct an empty string builder:

```Java
StringBuilder builder = new StringBuilder();
```

Each time you need to add another part, call the `append` method.

```Java
builder.append(ch); // appends a single character
builder.append(str); // appends a string
```

When you are done building the string, call the `toString` method. You will get a `String` object with the character sequence contained in the builder.

```Java
String completedString = builder.toString();
```

- NOTE: The `StringBuilder` class was introduced in JDK 5.0. Its predecessor, `StringBuffer`, is slightly less efficient, but it allows multiple threads to add or remove characters. If all string editing happens in a single thread (which is usually the case), you should use `StringBuilder` instead. The APIs of both classes are identical.

The following API notes contain the most important methods for the `StringBuilder` class.

- `java.lang.StringBuilder` - 5.0

    - `StringBuilder()`

        - _constructs_ an empty string builder.

    - `int length()`

        - _returns_ the _number_ of code units of the builder or buffer.

    - `StringBuilder append(String str)`

        - appends a _string_ and _returns_ `this`.

    - `StringBuilder append(char c)`

        - appends a _code unit_ and _returns_ `this`.

    - `StringBuilder appendCodePoint(int cp)`

        - appends a _code point_, converting it into one or two code units, and returns `this`. 

    - `void setCharAt(int i, char c)`

        - set the `i`th code unit to `c`.

    - `StringBuilder insert(int offset, String str)`

        - inserts a _string_ at position `offset` and returns `this`.

    - `StringBuilder insert(int offset, char c)`

        - inserts a _code unit_ at position `offset` and returns `this`

    - `StringBuilder delete(int startIndex, int endIndex)`

        - deletes the code units with offsets `startIndex` to `endIndex - 1` and _returns_ `this`.

    - `String toString()`

        - _returns_ a _string_ with the same data as the builder or buffer contents.

