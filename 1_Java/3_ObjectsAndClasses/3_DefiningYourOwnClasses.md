# Defining Your Own Classes

- [An `Employee` Class](#an-employee-class)
- [Use of Multiple Source Files](#use-of-multiple-source-files)
- [Dissecting the `Employee` Class]()
- [First Steps with Constructors]()
- [Implicit and Explicit Parameters]()
- [Benefits of Encapsulation]()
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