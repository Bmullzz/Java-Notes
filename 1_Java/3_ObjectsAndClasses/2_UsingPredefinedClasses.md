# Using Predefined Classes

- [Objects and Object Variables](#objects-and-object-variables)
- [The `LocalDate` Class of the Java Library]()
- [Mutator and Accessor Methods]()

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

- **C++ Note**: 