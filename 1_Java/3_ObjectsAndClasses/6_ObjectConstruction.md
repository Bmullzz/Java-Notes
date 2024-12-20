# Object Construction

- [Overloading](#overloading)
- [Default Field Initialization](#default-field-initialization)
- [The Constructor With No Arguments](#the-constructor-with-no-arguments)
- [Explicit Field Initialization](#explicit-field-initialization)
- [Parameter Names](#parameter-names)
- [Calling Another Constructor](#calling-another-constructor)
- [Initialization Blocks](#initialization-blocks)
- [Object Destruction and the `finalize` Method](#object-destruction-and-the-finalize-method)

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

If you write a class with no constructors whatsoever, then a no-argument constructor is provided for you. This constructor sets _all_ the instance fields to their default values. So, all numeric data contained in the instance fields would be `0`, all `boolean` values would be `false`, and all object variables would be set to `null`.

If a class supplies at least one constructor but does not supply a no-argument constructor, it is illegal to construct objects without supplying arguments. For example, our original `Employee` class in Listing 4.2 provided a single constructor:

```Java
Employee(String name, double salary, int y, int m, int d)
```

With that class, it was not legal to contruct default employees. That is, the call

```Java
e = new Employee();
```

would have been an error.

- **CAUTION**: Please keep in mind that you get a free no-argument constructor _only_ when your class has no other constructors. If you write your class with even a single constructor of your own and you want the users of your class to have the ability to create an instance by a call to 

    ```Java
    new ClassName() {

    }
    ```

    then you must provide a no-argument constructor. Of course, if you are happy with the default values for all fields, you can simply supply

    ```Java
    public ClassName() {

    }
    ```

## Explicit Field Initialization

By overloading the constructor methods in a class, you can build many ways to set the initial state of the instance fields of your classes. It is always a good idea to make sure that, regardless of the constructor call, every instance field is set to something meaningful.

You can simply assign a value to any field in the class definition. For example: 

```Java
class Employee {
    private String name = "";
    ...
}
```

This assignment is carried out before the constructor executes. This syntax is particularly useful if all constructors of a class need to set a particular instance field to the same value.

The initialization value doesn't have to be a constant value. Here is an example in which a field is initialized with a method call. Consider an `Employee` class where each employee has an `id` field. You can initialize it as follows:

```Java
class Employee {
    private static int nextId;
    private int id = assignId();
    ...
    private static int assignId() {
        int r = nextId;
        nextId++;
        return r;
    }
    ...
}
```

- **C++ Note**: In C++, you cannot directly initialize instance fields of a class. All fields must be set in a constructor. However, C++ has a special initializer list syntax, such as

    ```C++
    Employee::Employee(String n, double s, int y, int m, int d) // C++
    : name(n),
      salary(s),
      hireDay(y, m, d)
    {
    }
    ```

    C++ uses this special syntax to call field constructors. In Java, there is no need for that because objects have no subobjects, only pointers to other objects.

## Parameter Names

When you write very trivial constructors (and you'll write a lot of them), it can be somewhat frustrating to come up with parameter names.

We have generally opted for single-letter parameter names:

```Java
public Employee(String n, double s) {
    name = n;
    salary = s;
}
```

However, the drawback is that you need to read the code to tell what the `n` and `s` parameters mean.

Some programmers prefix each parameter with an "a":

```Java
public Employee(String aName, double aSalary) {
    name = aName;
    salary = aSalary;
}
```

That is quite neat. Any reader can immediately figure out the meaning of the parameters.

Another commonly used trick relies on the fact that parameter variables _shadow_ instance fields with the same name. For example, if you call a parameter `salary`, then `salary` refers to the parameter, not the instance field. But you can still access the instance field as `this.salary`. Recall that `this` denotes the implicit parameter, that is, the object being constructed. Here is an example:

```Java
public Employee(String name, double salary) {
    this.name = name;
    this.salary = salary;
}
```

- **C++ Note**: In C++, it is common to prefix instance fields with an underscore or a fixed letter. (The letters `m` and `x` are common choices.) For example, the salary field might be called `_salary`, `mSalary`, or `xSalary`. Java programmers don't ususally do that.

## Calling Another Constructor

The keyword `this` refers to the implicit parameter of a method. However, this keyword has a second meaning.

If _the first statement of a constructor_ has the form `this(...)`, then the constructor calls another constructor of the same class. Here is a typical example:

```Java
public Employee(double s) {
    // calls Employee(String, double)
    this("Employee #" + nextId, s);
    nextId++;
}
```

When you call `new Employee(60000)`, the `Employee(double)` constructor calls the `Employee(String, double)` constructor.

Using the `this` keyword in this manner is useful - you only need to write common construction code once.

- **C++ Note**: The `this` reference in Java is identical to the `this` pointer in C++. However, in C++ it is not possible for one constructor to call another. If you want to factor out common initialization code in C++, you must write a separate method.

## Initialization Blocks

You have already seen two ways to initialize a data field:

- By setting a value in a constructor

- By assigning a value in the declaration

There is a third mechanism in Java, called an _initialization block_. Class declarations can contain arbitrary blocks of code. These blocks are executed whenever an object of that class is constructed. For example:

```Java
class Employee {

    private static int nextId;

    private int id;
    private String name;
    private double salary;

    // object initialization block
    {
        id = nextId;
        nextId++;
    }

    public Employee(String n, double s) {
        name = n;
        salary = s;
    }

    public Employee() {
        name = "";
        salary = 0;
    }

    ...
}
```

In this example, the `id` field is initialized in the object initialization block, no matter which constructor is used to construct an object. The initialization block runs first, and then the body of the constructor is executed.

This mechanism is never necessary and is not common. It is usually more straightforward to place the initialization code inside a constructor.

- **NOTE**: It is illegal to set fields in initialization blocks even if they are only defined later in the class. However, to avoid circular definitions, it is not legal to read from fields that are only initialized later. The exact rules are spelled out in the Java Language Specification section (http://docs.oracle.com/javase/specs). The rules are complex enough to baffle the compiler implementors - early versions of Java implemented them with subtle errors. Therefore, we suggest that you always place initialization blocks agter the field definitions.

With so many ways of initializing data fields, it can be quite confusing to give all possible pathways for the construction process. Here is what happens in detail when a constructor is called:

1. All data fields are initialized it their default values(`0`, `false`, or `null`).

2. All field initializers and initialization blocks are executed, in the order in which they occur in the class declaration.

3. If the first line of the constructor calls a second constructor, then the body of the second constructor is executed.

4. The body of the constructor is executed.

Naturally, it is always a good idea to organize your initialization code so that another programmer could easily understand it without having to be a language lawyer. For example, it would be quite strange and somewhat error-prone to have a class whose constructors depend on the order in which the data fields are declared.

To initialize a static field, either supply an intial value or use a static initialization block. You have already seen the first mechanism:

```Java
private static int nextId = 1;
```

If the static fields of your class require complex initialization code, use a static initialization block.

Place the code inside a block and tag it with the keyword `static`. Here is an example. We want the employee ID numbers to start at a random integer less than 10,000.

```Java
// static initialization
static {
    Random generator = new Random();
    nextId = generator.nextInt(10000);
}
```

Static initialization occurs when the class is first loaded. Like instance fields, static fields are `0`, `false`, or `null` unless you explicitly set them to another value.

All static field initializers and static initialization blocks are executed in the order in which they occur in the class declaration.

- **NOTE**: Amazingly enough, up to JDK 6, it was possible to write a "Hello, World" program in Java without ever writing a `main` method.

    ```Java
    public class Hello {

        static {
            System.out.println("Hello, World");
        }
    }
    ```

    When you invoked the class with `java Hello`, the class was loaded, the static initialization block printed "Hello, World", and only then was a message displayed that `main` is not defined. Since Java SE 7, the `java` program first checks that there is a `main` method.

The program in [Listing 4.5]() shows many of the features that we discussed in this section:

- Overloaded constructors
- A call to another constructor with `this(...)`
- A no-argument constructor
- An object initialization block
- A static intialization block
- An instance field intialization

### Listing 4.5

- `ConstructorTest/ConstructorTest.java`

```Java
import java.util.*;

/**
 * This program demonstrates object construction.
 * @version 1.01 2004-02-19
 * @author Cay Horstmann
 **/
public class ConstructorTest {

    public static void main(String[] args) {
        // fill the staff with three Employee objects
        Employee[] staff = new Employee[3];

        staff[0] = new Employee("Harry", 40000);
        staff[1] = new Employee(60000);
        staff[2] = new Employee();

        // print out information about all Employee objects
        for (Employee e : staff) {
            System.out.println("name=" + e.getName() + ",id=" + e.getId() + ",salary=" + e.getSalary());
        }
    }
}

class Employee {

    private static int nextId;

    private int id;
    private String name = ""; // instance field initialization
    private double salary;

    // static initialization block
    static {
        Random generator = new Random();
        // set nextId to a random number between 0 and 9999
        nextId = generator.nextInt(10000);
    }

    // object initialization block
    {
        id = nextId;
        nextId++;
    }

    // three overloaded constructors
    public Employee(String n, double s) {
        name = n;
        salary = s;
    }

    public Employee(double s) {
        // calls the Employee(String, double) constructor
        this("Employee #" + nextId, s);
    }

    // the default constructor
    public Employee() {
        // name initialized to ""--see above
        // salary not explicitly set--initialized to 0
        // id initialized in initialization block
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public int getId() {
        return id;
    }
}
```

- `java.util.Random` 1.0

    - `Random()`
        - constructs a new random number generator

    - `int nextId(int n)` 1.2
        - returns a random number between `0` and `n-1`.

## Object Destruction and the `finalize` Method

Some object-oriented programming languages, notably C++, have explicit destructor for methods for any cleanup code that may be needed when an object is no longer used. The most common activity in a destructor is reclaiming the memory set aside for objects. Since Java does automatic garbage collection, manual memory reclamation is not needed, so Java does not support destructors.

Of course, some objects utilize a resource other than memory, such as a file or a handle to another object that uses system resources. In this case, it is important that the resource be reclaimed and recycled when it is no longer needed.

You can add a `finalize` method to any class. The `finalize` method will be called before the garbage collector sweeps away the object. In practice, _do not rely on the_ `finalize` _method_ for recycling any resources that are in short supply - you simply cannot know when this method will be called.

- **NOTE**: The method call `System.runFinalizersOnExit(true)` guarantees that finalizer methods are called before Java shuts down. However, this method is inherently unsafe and has been deprecated. An alternative is to add "shutdown hooks" with the method `Runtime.addShutdownHook - see the API documentation for details.

If a resource needs to be closed as soon as you have finished using it, you need to manage it manually. Supply a `close` method that does the necessary cleanup, and call it when you are done with the object. In the section "the Try-with-Resources Statement," you will see how you can ensure that this method is called automatically.
