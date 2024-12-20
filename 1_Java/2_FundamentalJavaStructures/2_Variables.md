# Java Variables

- [Initializing Variables](#initializing-variables)
- [Constants](#constants)

In Java, every variable has a _type_. You declare a variable by placing the type first, followed by the name of the variable. Here are some examples:

```java
double salary;
int vacationDays;
long earthPopulation;
boolean done;
```

Notice the semicolon at the end of each declaration. The semicolon is necessary because a declaration is a complete Java statement.

A variable name must begin with a letter and must be a sequence of letters or digits.

- Note: If you are really curious as to what Unicode characters are "letters" as far as Java is concerned, you can use the `isJavaIdentifierStart` and `isJavaIdentifierPart` methods in the `Character` class to check.

- Note: Even though `$` is a valid Java letter, you should not use it in you own code. It is intended for names that are generated by the Java compiler and other tools.

You cannot use a Java [reserved word](https://en.wikipedia.org/wiki/List_of_Java_keywords) as a variable name.

You can declare multiple variables on a single line:

```java
int i, j; // both are integers
```

However,  this style is not recommended.  If you declare each variable separately, your programs are easier to read.

- NOTE: Names are **case sensitive**, for example, `hireday` and `hireDay` are two separate names. In general, you should not have two names that only differ in their letter case. However, sometimes it is difficult to come up with a good name for a variable. Many programmers then give the variable the same name as the type, for example:

```Java
Box box; // "Box" is the type and "box" is the variable name
```

Other programmers prefer to use an "a" prefix for the variable:

```java
Box aBox;
```

## Initializing Variables

After you declare a variable, you must expicitly initialize it by means by means of an assignment statement - you can **never** use the value of an uninitialized variable. For example, the Java compiler flags the following sequence of statements as an error:

```java
int vacationDays;
System.out.println(vacationDays); // ERROR--variable not initialized
```

You assign to a previously declared variable by using the variable name on the left, an equal sign (=), and then some Java expression with an appropiate value on the right.

```java
int vacationDays;
vacationDays = 12;
```

Finally, in Java you can put declarations anywhere in your code. For example, the following is valid code in Java:

```java
double salary = 65000.0;
System.out.println(salary);
int vactionDays = 12; // OK to declare a variable here
```

In Java, it is consider good style to declare variables as closely as possible to the point where they are used (not sure if this is still true in 2024).

## Constants

In Java, use the keyword `final` to denote a constant. For example, 

```java
public class Constants {
    public static void main(String[] args) {
        final double CM_PER_INCH = 2.54;
        double paperWidth = 8.5;
        double paperHeight = 11;
        System.out.println("Paper size in centimeters: " + paperWidth * CM_PER_INCH + " by " + paperheight * CM_PER_INCH);
    }
}
```

The keyword `final` indicates that you can assign to the variable once, and then its value is set once and for all. It is customary to name constants in all uppercase.

It is probably more common in Java to create a constant so it's available to multiple methods inside a single class. These are usually called _class constants_. Set up a class constant with the keywords `static final`. Here is an example of using a class constant:

```java
public class Constants2 {
    
    public static final double CM_PER_INCH = 2.54;

    public static void main(String[] args) {

        double paperWidth = 8.5;
        double paperHeight = 11;
        System.out.println("Paper size in centimeters: " + paperWidth * CM_PER_INCH + " by " + paperheight * CM_PER_INCH);
    }
}
```

- Note that the definition of class constant appears _outside_ the `main` method. Thus, the constant can also be used in other methods of the same class. Furthermore, if the constant is declared, as in our example, `public`, methods of other classes can also use it - in our example, as `Constants2.CM_PER_INCH`.

