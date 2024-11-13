# Object Construction

- [Overloading](#overloading)
- [Default Field Initialization](#default-field-initialization)
- [The Constructor With No Arguments]()
- [Explicit Field Initialization]()
- [Parameter Names]()
- [Calling Another Constructor]()
- [Initialization Blocks]()
- [Object Destruction and the `finalize` Method]()

You have seen how to write simple constructors that define the initial state of your objects. However, since object construction is so important, Java offers quite a variety of mechanisms for writing constructors. We go over these mechanisms in the sections that follow.

## Overloading

Some classes have more than one constructor. For example, you can construct an empty `StringBuilder` object as 

```Java
StringBuilder messages = new StringBuilder();
```

Alternatively, you can specify an initial string:

```Java
StringBuilder toDoList = new StringBuilder("To do:\n");
```

This capability is called _overloading_. Overloading occurs if several methods have the same name (in this case, the `StringBuilder` constructor method) but different parameters. The compiler must sort out which method to call. It picks the correct method by matching the parameter types in the headers of the various methods with the types of values used in the specific method call. A compile-time error occurs if the compiler cannot match the parameters, either because there is no match at all or because there is not one that is better than all others. (The process of finding a match is called _overloading resolution_.)

- **Note**: Java allows you to overload any method - not just the constructor methods. Thus, to completely describe a method, you need to specify its name together with its parameter types. This is called the _signature_ of the method. For example, the `String` class has four public methods called `indexOf`. They have signatures 

    ```Java
    indexOf(int)
    indexOf(int, int)
    indexOf(String)
    indexOf(String, int)
    ```

    The return type is not part of the method signature. That is, you cannot have two methods with the same names and parameter types but different return types.

## Default Field Initialization

If you don't set a field explicitly in a constructor, it is automatically set to a default value: numbers to `0`, `boolean` values to `false`, and object references to `null`. Some people consider it poor programming practice to rely on the defaults. Certainly, it makes it harder for someone to understand your code if fields are being initialized invisibly.

- **Note**: This is an important difference between fields and local variables. You must always explicitly initialize local variables in a method. But in a class, if you don't initialize a field, it is automatically initialized to a default (`0`, `false`, or `null`).

For example, consider the `Employee` class. Suppose you don't specify how to initialize some of the fields in a constructor. By default, the `salary` field would be initialized with `0` and the `name` and `hireDay` fields would be initialized with `null`.

However, that would not be a good idea. If anyone called the `getname` or `getHireDay` method, they would get a `null` reference that they probably don't expect:

```Java
LocalDate h = harry.getHireDay();
int year = h.getYear(); // throws exception if h is null
```

## The Constructor with No Arguments

Many classes contain a constructor with no arguments that creates an object whose state is set to an appropriate default. For example, here is a constructor with no arguments for the `Employee` class:

```Java
pubic Employee() {
    name = "";
    salary = 0;
    hireDay = LocalDate.now();
}
```

If you 