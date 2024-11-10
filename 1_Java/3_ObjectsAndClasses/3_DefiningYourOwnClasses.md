# Defining Your Own Classes

- [An `Employee` Class](#an-employee-class)
- [Use of Multiple Source Files](#use-of-multiple-source-files)
- [Dissecting the `Employee` Class](#dissecting-the-employee-class)
- [First Steps with Constructors](#first-steps-with-constructors)
- [Implicit and Explicit Parameters](#implicit-and-explicit-parameters)
- [Benefits of Encapsulation](#benefits-of-encapsulation)
- [Class-Based Access Privileges]()
- [Private Methods]()
- [Final Instance Fields]()

In the previous chapter, you started writing simple classes. However, all those classes had just a single `main` method. Now the time has come to show you how to write the kind of "workhorse classes" that are needed for more sophisticated applications. These classes typically do not have a `main` method. Instead, they have their own instance fields and methods. To build a complete program, you combine several classes, one of which has a `main` method.

## An `Employee` Class

The simplest form for a class definition in Java is 

```Java
class ClassName {
    field1
    field2
    ...
    constructor1
    constructor2
    ...
    method1
    method2
    ...
}
```

Consider the following, very simplified, version of an `Employee` class that might be used by a business in writing a payroll system.

```Java
class Employee {

    // instance fields
    private String name;
    private double salary;
    private LocalDate hireDay;

    // constructor
    public Employee(String n, double s, int year, int month, int day) {
        name = n;
        salary = s;
        hireDay = LocalDate.of(year, month, day);
    }

    // a method
    public String getName() {
        return name;
    }

    // more methods
    ...
}
```

We break down the implementation of this class, in some detail, in the sections that follow. First, though, [Listing 4.2](#listing-42) is a program that shows the `Employee` class in action.

In the program, we construct an `Employee` array and fill it with three employee objects:

```Java
Employee[] staff = new Employee[3];

staff[0] = new Employee("Carl Cracker", ...);
staff[1] = new Employee("Hary Hacker", ...);
staff[2] = new Employee("Tony Tester", ...);
```

Next, we use the `raiseSalary` method od the `Employee` class to raise each employee's salary by 5%:

```Java
for (Employee e : staff) {
    e.raiseSalary(5);
}
```

Finally, we print out information about each employee, by calling the `getName`, `getSalary`, and `getHireDay` methods:

```Java
for (Employee e : staff) {
    System.out.println("name=" + e.getName() + ",salary=" + e.getSalary() + ",hireDay=" + e.getHireDay());
}
```

Note that the example program consists of _two_ classes: the `Employee` class and a class `EmployeeTest` with the `public` access specifier. The `main` method with the instructions that we just described is contained in the `EmployeeTest` class.

The name of the source file is `EmployeeTest.java` because the name of the file must match the name of the `public` class. You can only have one public class in a source file, but you can have any number of nonpublic classes.

Next, when you compile this source code, the compiler creates two class files in the directory: `EmployeeTest.class` and `Employee.class`.

You then start the program by giving the bytecode interpretor the name of the class that contains the `main` method of your program:

```terminal
java EmployeeTest
```

The bytecode interpretor starts running the code in the `main` method in the `EmployeeTest` class. this code in turn constructs three new `Employee` objects and shows you their state.

### Listing 4.2

- `EmployeeTest/EmployeeTest.java`

```Java
import java.time.*;

/**
* This program tests the Employee Class
* @version 1.12 2015-05-08
* @author Cay Horstmann
**/
public class EmployeeTest {
    
    public static void main(String[] args) {

        // fill the staff array with three Employee objects
        Employee[] staff = new Employee[3];

        staff[0] = new Employee("Carl Cracker", 75000, 1987, 12, 15);
        staff[1] = new Employee("Hary Hacker", 50000, 1989, 10, 1);
        staff[2] = new Employee("Tony Tester", 40000, 1990, 3, 15);

        // raise everyone's salary by 5%
        for (Employee e : staff) {
            e.raiseSalary(5);
        }

        // print out information about all Employee objects
        for (Employee e : staff) {
            System.out.println("name=" + e.getName() + ",salary=" + e.getSalary() + ",hireDay=" + e.getHireDay());
        }
    }
}

class Employee {

    private String name;
    private double salary;
    private LocalDate hireDay;

    public Employee(String n, double s, int year, int month, int day) {
        name = n;
        salary = s;
        hireDay = LocalDate.of(year, month, day);
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public LocalDate getHireDay() {
        return hireDay;
    }

    public void raiseSalary(double byPercent) {
        double raise = salary * byPercent / 100;
        salary += raise;
    }
}
```

## Use of Multiple Source Files

The program in [Listing 4.2](#listing-42) has two classes in a single source file. Many programmers prefer to put each class into its own source file. For example, you can place the `Employee` class into a file `Employee.java` and the `EmployeeTest` class into `EmployeeTest.java`.

If you like this arrangement, you have two choices for compiling the program. You can invoke the Java compiler with a wildcard:

```terminal
javac Employee*.java
```

Then, all source files matching the wildcard will be compiled into class files. Or, you can simply type

```terminal
javac EmployeeTest.java
```

You may find it surprising that the second choice works even though the `Employee.java` file is never explicity compiled. However, when the Java compiler sees the `Employee` class being used inside `EmployeeTest.java`, it will look for a file named `Employee.class`. If it does not find that file, it automatically searches for `Employee.java` and compiles it. Moreover, if the timestamp of the version of `Employee.java` that it finds is newer than that of the existing `Employee.class` file, the Java compiler will _automatically_ recompile the file.

- **Note**: If you are familiar with the `make` facility of UNIX (or one of its Windows cousins, such as `nmake`), then you can think of the Java compiler as having the `make` functionality already built in.

## Dissecting the `Employee` Class

In the sections that follow, we will dissect the `Employee` class. Let's start with the methods in this class. As you can see by examining the source code, this class has one constructor and four methods:

```Java
public Employee(String n, double s, int year, int month, int day)
public String getName()
public double getSalary()
public LocalDate getHireDay()
public void raiseSalary(double byPercent)
```

All methods of this class are tagged as `public`. The keyword `public` means that any method in any class can call the method. (The four possible access levels are covered in this and the next chapter.)

Next, notice the three instance fields that will hold the data manipulated inside an instance of the `Employee` class.

```Java
private String name;
private double salary;
private LocalDate hireDay;
```

The `private` keyword makes sure that the _only_ methods that can access these instance fields are the methods of the `Employee` class itself. No outside method can read or write to these fields.

- **Notes**: You could use the `public` keyword with your instance fields, but it would be a very bad idea. Having `public` data fields would allow any part of the program to read and modify the instance fields, completely ruining encapsulation. Any method of any class can modify public fields - and, in our experience, some code _will_ take advantage of that access privilage when you least expect it. We strongly recommend to make all your instnace fields `private`.

Finally, notice that two of the instance fields are themselves objects: The `name` and `hireDay` fields are references to `String` and `LocalDate` objects. This is quite usual: Classes will often contain instance fields of class type.

## First Steps with Constructors

Let's look at the constructor listed in our `Employee` class.

```Java
public Employee(String n, double s, int year, int month, int day) {

    name = n;
    salary = s;
    hireDay = LocalDate.of(year, month, day);
}
```

As you can see, the name of the constructor is the same as the name of the class. This constructor runs when you construct objects of the `Employee` class - giving the instance fields the initial state you want them to have.

For example, when you create an instance of the `Employee` class with the code like this:

```Java
new Employee("James Bond", 100000, 1950, 1, 1)
```

you have set the instance fields as follows:

```Java
name = "James Bond";
salary = 100000;
hireDay = LocalDate.of(1950, 1, 1); // January 1, 1950
```

There is an important difference between constructors and other methods. A constructor can only be called in conjunction with the `new` operator. You can't apply a constructor to an existing object to reset the instance fields. For example, 

```Java
james.Employee("James Bond", 250000, 1950, 1, 1) // ERROR
```

is a compile-time error.

We will have more to say about constructors later in this chapter. For now, keep the following in mind:

- A constructor has the same name as the class.
- A class can have more than one constructor.
- A constructor can take zero, one, or more parameters.
- A constructor has no return value.
- A constructor is always called with the `new` operator.

=====

- **C++ Note**: Constructors work the same way in Java as they do in C++. Keep in mind, however, that all Java objects are constructed on the heap and that a constructor must be combined with `new`. It is a common error of C++ programmers to forget the `new` operator:

    ```C++
    Employee number007("James Bond", 100000, 1950, 1, 1); // C++, not Java
    ```

    That works in C++ but not in Java.

- **CAUTION**: Be careful not to introduce local variables with the same names as the instance fields. For example, the following constructor will not set the salary:

    ```Java
    public Employee(Stirng n, double s, ...) {
        String name = n; // Error
        double salary = s; // Error
    }
    ```

    The constructor declares _local_ variables `name` and `salary`. These variables are only accessible inside the constructor. They _shadow_ the instance fields with the same name. Some programmers accidentally write this kind of code when they type faster than they think, because their fingers are used to adding the data type. This is a nasty error that can be hard to track down. You just have to be careful in all of your methods to not use variable names that equal the names of instance fields.

## Implicit and Explicit Parameters

Methods operate on objects and access their instance fields. For example, the method

```Java
public void raiseSalary(double byPercent) {
    double raise = salary * byPercent / 100;
    salary += raise;
}
```

sets a new value for the `salary` instance field in the object on which this method is invoked. Consider the call

```Java
number007.raiseSalary(5);
```

The effect is to increase the value of the `number007.salary` field by 5%. More specifically, the call executes the following instructions:

```Java
double raise = number007.salary * 5 / 100;
number007.salary += raise;
```

The `raiseSalary` method has two parameters. The first parameter, called the _implicit_ parameter, is the object of type `Employee` that appears before the method name. The second parameter, the number inside the parentheses after the method name, is an _explicit_ parameter. (Some people call the implicit parameter the _target_ or _receiver_ of the method call.)

As you can see, the explicit parameters are explicitly listed in the method declaration, for example, `double byPercent`. The implicit parameter does not appear in the method declaration. 

In every method, the keyword `this` refers to the implicit parameter. If you like, you can write the `raiseSalary` method as follows:

```Java
public void raiseSalary(double byPercent) {
    double raise = this.salary * byPercent / 100;
    this.salary += raise;
}
```

Some programmers prefer that style because it clearly distinguishes between instance fields and local variables.

- **C++ Note**: In C++, you generally define methods outside the class:

    ```C++ 
    void Employee::raiseSalary(double byPercent){ // C++, not Java
        ...
    }
    ```

    If you define a method inside a class, then it is, automatically, an inline method.

    ```C++
    class Employee {
        ...
        int getName() { return name; } // inline in C++
    }
    ```

    In Java, all methods are defined inside the class itself. This does not make them inline. Finding opportunites for inline replacement is the job of the Java virtual machine. The just-in-time compiler watches for calls to methods that are short, commonly called, and not overridden, and optimizes them away.

## Benefits of Encapsulation

Finally, let's look more closely at the rather simple `getName`, `getSalary`, and `getHireDay` methods.

```Java
public String getName() {
    return name;
}

public double getSalary() {
    return salary;
}

public LocalDate getHireDay() {
    return hireDay;
}
```

These are obviously examples of accessor methods. As they simply return the values of instance fields, they are sometimes called _field accessors_.

Wouldn't it be easier to make the `name`, `salary`, and `hireDay` fields public, instead of having separate accessor methods?

However, the `name` field is a read-only field. Once you set it in the constructor, there is no method to change it. Thus, we have a guaruntee that the `name` field will never be corrupted.

The `salary` field is not read-only, but it can only be changed by the `raiseSalary` method. In particular, should the value ever turn out wrong, only that method needs to be debugged. Had the `salary` field been public, the culprit for messing up the value could have been anywhere.

Sometimes, it happens that you want to get and set the value of an instance field. Then you need to supply _three_ items:

- A private data field;
- A public field accessor method; and
- A public field mutator method.

This is a lot more tedious than supplying a single public data field, but there are considerable benefits.

First, you can change the internal implementation without affecting any code other than the methods of the class. For example, if the storage of the name is changed to

```Java
String firstName;
String lastName;
```

then the `getName` method can be changed to return

```Java
firstName + " " + lastName
```

This change is completely invisible to the remainder of the program.

Of course, the accessor and mutator methods may need to do a lot of work and convert between the old and new data representation. That leads us to our second benefit: Mutator methods can perform error checking, whereas code that simply assigns to a field may not go into the trouble. For example, a `setSalary` method might check that the salary is never less than `0`.