# Using Predefined Classes

- [Objects and Object Variables](#objects-and-object-variables)
- [The `LocalDate` Class of the Java Library](#the-localdate-class-of-the-java-library)
- [Mutator and Accessor Methods](#mutator-and-accessor-methods)

You can't do anything in Java without classes, and you have already seen several classes at work. However, not all of these show off the typical features of object orientation. Take, for example, the `Math` class. You have seen that you can use methods of the `Math` class, such as `Math.random`, without needing to know how they are implemented - all you need to know is the name and parameters (if any).

That's the point of encapsulation, and it will certainly be true of all classes. But the `Math` class _only_ encapsulates functionality; it neither needs nor hides data. Since there is no data, you do not need to worry about making objects and initializing their instance fields - there aren't any!

In the next section, we will look at a more typical class, the `Date` class. You will see how to construct objects and call methods of this class.

## Objects and Object Variables

To work with objects, you first construct them and specify their initial state. Then you apply methods to the objects.

In the Java programming language, you use _constructors_ to construct new instances. A constructor is a special method whose purpose is to construct and initialize objects. Let us look at an example. The standard Java library contains a `Date` class. Its objects describe points in time, such as `"December 31, 1999, 23:59:59 GMT"`.

- **Note**: You may be wondering: Why use a class to represent dates rather than (as in some other languages) a built-in type? For example, Visual Basic has a built-in date type, and programmers can specify dates in the format `#6/1/1995#`. On the surface, this sounds convenient - programmers can simply use the built-in date type without worrying about classes. But actually, how suitable is the Visual Basic design? In some locales, dates are specified as month/day/year, in others as day/month/year. Are the language designers really equipped to foresee these kinds of issues? If they do a poor job, the language becomes an unpleasant muddle, but unhappy programmers are powerless to do anything about it. With classes, the design task is offloaded to a library designer. If the class is not perfect, other programmers can easily write their own classes to enhance or replace the system classes. (To prove a point: The Java date library started out a bit muddled, and it has been redesigned twice.)

Constructors always have the smae name as the class name. Thus, the constructor for the `Date` class is called `Date`. To construct a `Date` object, combine the constructor with the `new` operator, as follows:

```Java
new Date()
```

This expression constructs a new object. The object is initialized to the current date and time.

If you like, you can pass the object to a method:

```Java
System.out.println(new Date());
```

Alternatively, you can apply a method to the object that you just constructed. One of the methods of the `Date` class is the `toString` method. That method yields a string representation of the date. Here is how you would apply the `toString` method to a newly constructed `Date` object:

```Java
String s = new Date().toString();
```

In these two examples, the constructed object is used only once. Usually, you will want to hang on to the objects that you construct so that you can keep using them. Simply store the object in a variable:

```Java
Date birthday = new Date();
```

The object variable `birthday` refers to the newly constructed object.

There is an important difference between objects and object variables. For example, the statement

```Java
Date deadline; // deadline doesn't refer to any object
```

defines an object variable, `deadline`, that can refer to objects of type `Date`. It is important to realize that the variable `deadline` _is not an object_ and, in fact, does not even refer to an object yet. You cannot use any `Date` methods on this variable at this time. The statement

```Java
s = deadline.toString(); // not yet
```

would cause a compile-time error.

You must first initalize the `deadline` variable. You have two choices. Of course, you can initialize the variable with a with a newly constructed object:

```Java
deadline = new Date();
```

Or you can set the variable to refer to an exisiting object:

```Java
deadline = birthday;
```

Now both variables refer to the _same_ object.

It is important to realize that an object variable doesn't actually contain an object. It only _refers_ to an object.

In Java, the value of any object variable is a reference to an object that is stored elsewhere. The return value of the `new` operator is also a reference. A statement such as

```Java
Date deadline = new Date();
```

has two parts. The expression `new Date()` makes an object of type `Date`, and its value is a reference to that newly created object. That reference is then stored in the `deadline` variable.

You can explicitly set an object variable to `null` to indicate that it currently refers to no object.

```Java
deadline = null;
...
if (deadline != null) {
    System.out.println(deadline);
}
```

If you apply a method to a variable that holds `null`, a runtime error occurs.

```Java
birthday = null;
String s = birthday.toString(); // runtime error!
```

Local variables are not automatically initialized to `null`. You must initialize them, either by calling `new` or by setting them to `null`.

## The `LocalDate` Class of the Java Library

In the preceding examples, we used the `Date` class that is a part of the standard Java library. An instance of the `Date` class has a state, namely _a particular point in time_.

Although you don't need to know this when you use the `Date` class, the time is represented by the number of milliseconds (positive or negative) from a fixed point, the so-called _epoch_, which is `00:00:00 UTC, January 1, 1970`. `UTC` is the Coordinated Universal Time, the scientific time standard which is, for practical purposes, the same as the more familiar `GMT`, or Greenwich Mean Time.

But as it turns out, the `Date` class is not very useful for manipulating the kind of calendar information that humans use for dates, such as "December 31, 1999". This particular description of a day follows the Gregorian calendar, which is the calendar used in most countries around the world. The same point in time would be described quite differently inn the Chinese or Hebrew lunar calendars, not to mention the calendar used by your customers from Mars.

- **Notes**: Throughout human history, civilizations grappled with the design of calendars to attach names to dates and bring order to the solar and lunar cycles. For a fascinating explanation of calendars around the world, from the French Revolutionary calendar to the Mayan long count, see _Calendrical Calculations_ by Nachum Dershowitz and Edward M. Reingold (Cambridge University Press, 3rd ed., 2007).

The library designers decided to separate the concerns of keeping time and attaching names to points in time. Therefore, the standard Java library contains two separate classes: the `Date` class, which represents a point in time, and the `LocalDate` class, which expresses days in the familiar calendar notation. Java SE 8 introduced quite a few other classes for manipulating various aspects of date and time.

Separating time measurement from calendars is good object-oriented design. In general, it is a good idea to use separate classes to express different concepts.

You do not use a constructor to construct objects of the `LocalDate` class. Instead, use static _factory methods_ that call constructors on your behalf. The expression

```Java
LocalDate.now()
```

constructs a new object that represents the date at which the object was constructed.

You can construct an object for a specific date by supplying year, month, and day:

```Java
LocalDate.of(1999, 12, 31)
```

Of course, you will usually want to store the constructed object in an object variable:

```Java
LocalDate newYearsEve = LocalDate.of(1999, 12, 31);
```

Once you have a `LocalDate` object, you can find out the year, month, and day with the methods `getYear`, `getMonthValue`, and `getDayOfMonth`:

```Java
int year = newYearsEve.getYear(); // 1999
int month = newYearsEve.getMonthValue(); // 12
int day = newYearsEve.getDayOfMonth(); // 31
```

This may seem pointless because they are the very same values that you just used to construct the object. But sometimes, you have a date that has been computed, and then you will want to invoke those methods to find out more about it. For example, the `plusDays` method yields a new `LocalDate` that is a given number of days away from the object to which you apply it:

```Java
LocalDate aThousandDaysLater = newYearsEve.plusDays(1000);
year = aThousandDaysLater.getYear(); // 2002
month = aThousandDaysLater. getMonthValue() // 09
day = aThousandDaysLater.getDayOfMonth(); // 26
```

The `LocalDate` class has encapsulated instance fields to maintain the date to which it is set. Without looking at the source code, it is impossible to know the representation that the class uses internally. But, of course, the point of encapsulation is that this doesn't matter. What matters are the methods that a class exposes.

- **Note**: Actually, the `Date` class also has methods to get the day, month, and year, called `getDay`, `getMonth`, and `getYear`, but these methods are _deprecated_. A method is deprecated when a library designer realizes that the method should have never been introduced in the first place.

These methods were a part of the `Date` class before the library designers realized that it makes more sense to supply separate classes to deal with calendars. When an earlier set of calendar classes was introduced in Java 1.1, the `Date` methods were tagged as separate deprecated. You can still use them in your programs, but you will get unsightly compliler warnings if you do. It is a good idea to stay away from using deprecated methods because they may be removed in a future version of the library.

## Mutator and Accessor Methods

Have another look at the `plusDays` method call that you saw in the preceding section:

```Java
LocalDate aThousandDaysLater = newYearsEve.plusDays(1000);
```

What happens to `newYearsEve` after the call? Has it been changed to be a thousand days later? As it turns out, it has not. The `plusDays` method yields a new `LocalDate` object, which is then assigned to the `aThousandDaysLAter` variable. The original object remains unchanged. We say that the `plusDays` method does not _mutate_ the object on which it is invoked. (This is similar to the `toUpperCase` method of the `String` class that you saw in Chapter 3. When you call `toUpperCase` on a string, that string stays the same, and a new string with uppercase characters is returned.)

An earlier version of the Java library had a different class for dealing with calendars, called `GregorianCalendar`. Here is how you add a thousand days to a date represented by that class:

```Java
GregorianCalendar someDay = new GregorianCalendar(1999, 11, 31);
    // Odd feature of that class: month numbers go from 0 to 11
someDay.add(Calendar.DAY_OF_MONTH) + 1; // 09
```

Unlike the `LocalDate.plusDays` method, the `GregorianCalendar.add` method is a _mutator method_. After invoking it, the state of the `someDay` object has changed. Here is how you can find out the new state: 

```Java
year = someDay.get(Calendar.YEAR); // 2002
month = someDay.get(Calendar.MONTH) + 1; // 09
day = someDay.get(Calendar.); // 26
```

That's why we called the variable `someDay` and not `newsYearsEve` - it no longer is new year's eve after calling the mutator method.

In contrast, methods that only access objects without modifying them are sometimes called _accessor methods_. For example, `LocalDate.getYear` and `GregorianCalendar.get` are accessor methods.

- **C++ Note**: In C++, the `const` suffix denotes accessor methods. A method that is not declared as `const` is assumed to be a mutator. However, in that Java programming language, no special syntax distinguishes accessors from mutators.

We finish this section with a program that puts the `LocalDate` class to work. The program displays a calendar for the current month, like this:

| Mon | Tue | Wed | Thu | Fri | Sat | Sun |
| --- | --- | --- | --- | --- | --- | --- |
|     |     |     |     |     |     |  1  |
| 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| 9   | 10  | 11  | 12  | 13  | 14  | 15  |
| 16  | 17  | 18  | 19  | 20  | 21  | 22  |
| 23  | 24  | 25  | 26* | 27  | 28  | 29  |
| 30  |     |     |     |     |     |     |

The current day is marked with an asterisk (*). As you can see, the program needs to know how to compute the length of a month and the weekday of a given day.

Let us go through the key steps of the program. First, we construct an object that is initialized with the current date.

```Java
LocalDate date = LocalDate.now();
```

We capture the current month and day.

```Java
int month = date.getMonthValue();
int today = date.getDayOfMonth();
```

Then we set `date` to the first of the month and get the weekday of that date.

```Java
date = date.minusDays(today - 1); // Set to start of month
DayOfWeek weekday = date.getDayOfWeek();
int value = week.getValue(); // 1 = Monday, ... 7 = Sunday
```

The variable `weekday` is set to an object of type `DayOfWeek`. We call the `getValue` method of that object to get a numerical value for the weekday. This yields an integer that follows the international convention where the weekend comes at the end of the week, returning `1` for Monday, `2` for Tuesday, and so on. Sunday has value `7`.

Note that the first line of the calendar is indented, so that the first day of the month falls on the appropriate weekeday. Here is the code to print the header and the indentation for the first line:

```Java
System.out.println("Mon Tue Wed Thu Fri Sat Sun");
for (int i = 1; i < value; i++) {
    System.out.print("   ");
}
```

Now, we are ready to print the date value. If `date` is today, the date is marked with an `*`. Then, we advance `date` to the next day. If we reach the beginning of each new week, we print a new line:

```Java
while (date.getMonthValue() == month) {
    System.out.printf("%3d", date.getDayOfMonth());
    if (date.getDayOfMonth() == today) {
        System.out.print("*");
    } else {
        System.out.print("  ");
    }
    date = date.plusDays(1);
    if (date.getDayOfWeek().getValue() == 1) System.out.println();
}
```

When do we stop? We don't know whether the month has 31, 30, 29, or 28 days. Instead, we keep iterating while `date` is still in the current month.

[Listing 4.1](#listing-41) shows the complete program.

As you can see, the `LocalDate` class makes it possible to write a calendar program that takes care of complexities such as weekdays and the varying month lengths. You don't need to know _how_ the `LocalDate` class computes months and weekdays. You just use the _interface_ of the class - the methods such as `plusDays` and `getDayOfWeek`.

The point of this example program is to show you how you can use the interface of a class to carry out fairly sophisticated tasks without having to know the implementation details.

### Listing 4.1

- `CalendarTest/CalendarTest.java`

```Java
import java.time.*;

/**
* @version 1.5 2015-05-08
* @author Cay Horstmann
**/

public class CalendarTest {

    public static void main(String[] args) {

        LocalDate date = LocalDate.now();
        int month = date.getMonthValue();
        int today = date.getDayOfMonth();

        date = date.minusDays(today - 1); // Set to start of month
        DayOfWeek weekday = date.getDayOfWeek();
        int value = weekday.getValue(); // 1 = Monday, ... 7 = Sunday

        System.out.println("Mon Tue Wed Thu Fri Sat Sun");
        for (int i = 1; i < value; i++) {
            System.out.print("   ");
        }

        while (date.getMonthValue() == month) {
            System.out.printf("%3d", date.getDayOfMonth());
            if (date.getDayOfMonth() == today) {
                 System.out.print("*");
            } else {
                System.out.print("  ");
            }
            date = date.plusDays(1);
            if (date.getDayOfWeek().getValue() == 1) System.out.println();
        }
        
        if (date.getDayOfWeek().getValue() != 1) System.out.println();
    }
}
```

- `java.time.LocalDate` - Java 8

    - `static LocalTime now()`

        - constructs an object that represents the current date.

    - `static LocalTime of(int year, int month, int day)`

        - constructs an object that represents the given date.

    - `int getYear()`

    - `int getMonthValue()`

    - `int getDayOfMonth()`

        - get the year, month, and day of this date

    - `DayOfWeek getDayOfWeek`

        - Gets the weekday of this date as an instance of the `DayOfWeek` class. Call `getValue` to get a weekday between `1` (Monday) and `7` (Sunday).

    - `LocalDate plusDays(int n)`

    - `LocalDate minusDays(int n)`

        - Yields the date that is `n` days after or before this date.