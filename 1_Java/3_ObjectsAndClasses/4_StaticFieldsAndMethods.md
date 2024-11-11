# Static Fields and Methods

- [Static Fields](#static-fields)
- [Static Constants](#static-constants)
- [Static Methods](#static-methods)
- [Factory Methods](#factory-methods)
- [The `main` Method](#the-main-method)

In all programs that you have seen, the `main` method is tagged with the `static` modifier. We are now ready to discuss the meaning of this modifier.

## Static Fields

If you define a field as `static`, then there is only one such field per class. In contrast, each object has its own copy of all instance fields. For example, let's suppose we want to assign a unique identification number to each employee. We add an instance field `id` and a static field `nextId` to the `Employee` class:

```Java
class Employee {
    private static int nextId = 1;

    private int id;
    ...
}
```

Every employee object now has its own `id` field, but there is only one such field per class. Let's put it another way. If there are 1000 objects of the `Employee` class, then there are 1000 instance fields `id`, one for each object. But there is a single static field `nextId`. Even if there are no employee objects, the static field `nextId` is present. It belongs to the class, not to any individual object.

- **NOTE**: In some object-oriented programming languages, static fields are called _class fields_. The term "static" is a meaningless holdover from C++.

Let's implement a simple method: 

```Java
public void setId() {
    id = nextId;
    nextId++;
}
```

Suppose you set the employee identification number for `harry`:

```Java
harry.setId();
```

then, the `id` field of `harry` is set to the current value of the static field `nextId`, and the value of the static field is incremented:

```Java
harry.id = Employee.nextId;
Employee.nextId++;
```

## Static Constants

Static variables are quite rare. However, static constants are mor ecommon. For example, the `Math` class defines a static constant:

```Java
public class Math {
    ...
    public static final double PI = 3.14159265358979323846;
    ...
}
```

You can access this constant in your programs as `Math.PI`.

If the keyword `static` had been omitted, then `PI` would have been an instance field of the `Math` class. That is, you would need an object of this class to access `PI`, and every `Math` object would have its own copy of `PI`.

Another static constant that you have used many times is `System.out`. It is declared in the `System` class as follows:

```Java
public class System {
    ...
    public static final PrintStream out = ...;
    ...
}
```

As we mentioned several times, it is never a good idea to have public fields, because everyone can modify them. However, public constants (that is, `final` fields) are fine. Since `out` has been declared as `final`, you cannot reassign another print stream to it:

```Java
System.out = new PrintStream(...); // Error--out is final
```

- **NOTE**: If you look at the `System` class, you will notice a method can change the value of a `final` variable. However, the `setOut` method is a _native_ method, not implemented in the Java programming language. Native methods can bypass the access control mechanisms of the Java language. This is a very unusual workaround that you should not emulate in your programs.

## Static Methods

Static methods are methods that do not operate on objects. For example, the `pow` method of the `Math` class is a static method. The expression

```Java
Math.pow(x, a);
```

computes the power $x^{a}$. It does not use any `Math` object to carry out its task. In other words, it has no implicit parameter.

You can think of static methods as methods that don't have a `this` parameter. (In a nonstatic method, the `this` parameter refers to the implicit parameter of the method - see the section on "Implicit and Explicit Parameters,".)

A static method of the `Employee` class cannot access the `id` instance field because it does not operate on an object. However, a static method can access a static field. Here is an example of such a static method:

```Java
public static int getNextId() {
    return nextId; // returns static field
}
```

To call this method, you supply the name of the class:

```Java
int n = Employee.getNextId();
```

Could you have omitted the keyword `static` for this method? Yes, but then you would need to have an object reference of type `Employee` to invoke the method.

- **NOTE**: It is legal to use an object to call a static method. For example, if `harry` is an `Employee` object, then you can call `harry.getNextId()` instead of `Employee.getNextId()`. However, we find that notation confusing. The `getNextId` method doesn't look at `harry` at all to compute the result. We recommend that you use class names, not objects, to invoke static methods.

Use static methods in two situations:

- When a method doesn't need to access the object state because all needed parameters are supplied as explicit parameters (example: `Math.pow`).

- When a method only needs to access static fields of the class (example: `Employee.getNextId`).

---

- **C++ Note**: Static fields and methods have the same functionality in Java and C++. However, the syntax is slightly different. In C++, you use the `::` operator to access a static field or method outside its scope, such as `Math::PI`.

    The term "static" has a curious history. At first, the keyword `static` was introduced in C to denote local variable that don't go away when a block is exited. In that context, the term "static" makes sense: The variable stays around and is still there when the block is entered again. Then `static` got a second meaning in C, to denote global variables and functions that cannot be accessed from other files. The keyword `static` was simply reused, to avoid introducing a new keyword. Finally, C++ reused the keyword for a third, unrelated, interpretation - to denote variables and functions that belong to a class but not to any particular object of the class. That is the same meaning the keyword has in Java.

## Factory Methods

Here is another common use for static methods. Classes such as `LocalDate` and `NumberFormat` use static _factory methods_ that constructs objects. You have already seen the factory methods `LocalDate.now` and `LocalDate.of`. Here is how the `NumberFormat` class yields formatter objects for various styles:

```Java
NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();
NumberFormat percentFormatter = NumberFormat.getPercentInstance();
double x = 0.1;
System.out.println(currencyFormatter.format(x)); // prints $0.10
System.out.println(percentFormatter.format(x)); // prints 10%
```

Why doesn't the `NumberFormat` class use a constructor instead? There are two reasons:

- You can't give names to the constructors. The constructor name is always the same as the class name. But we want two different names to get the currency instance and the percent instance.

- When you use a constructor, you can't vary the type of the constructed object. But the factory methods actually return objects of the class `DecimalFormat`, a sub-class that inherits from `NumberFormat`. (See the chapter on inheritance to learn more.)

## The `main` Method

Note that you can call static methods without having any objects. For example, you never construct any objects of the `Math` class to call `Math.pow`.

For the same reason, the `main` method is a static method.

```Java
public class Application {

    public static void main(String[] args) {

        //construct objects here
        ...
    }
}
```

The `main` method does not operate on any objects. In fact, when a program starts, there aren't any objects yet. The static `main` method executes, and constructs the objects that the program needs.

- **Tip**: Every class can have a `main` method. That is a handy trick for unit testing of classes. For example, you can add a `main` method to the `Employee` class:

    ```Java
    class Employee {

        public Employee(String n, double s, int year, int month, int day) {
            name = n;
            salary = s;
            LocalDate hireDay = LocalDate.now(year, month, day);
        }
        ...
        public static void main(String[] args) { // unit test
            Employee e = new Employee("Romeo", 50000, 2003, 3, 31);
            e.raiseSalary(10);
            System.out.println(e.getName() + " " + e.getSalary());
        }
        ...
    }
    ```

    If you want to test the `Employee` class in isolation, simple execute

    ```terminal
    java Employee
    ```

    If the `Employee` class is a part of a larger application, you start the application with 

    ```terminal
    java Application
    ```

    and the `main` method of the `Employee` class is never executed.

The program in [Listing 4.3](#listing-43) contains a simple version of the `Employee` class with a static field `nextId` and a static method `getNextId`. We fill an array with three `Employee` objects and then print the employee information. Finally, we print the next available identification number, to demonstrate the static method. 

Note that the `Employee` class also has a static `main` method for unit testing. Try running both

```terminal
java Employee
```

and

```terminal
java StaticTest
```

to execute both `main` methods.

### Listing 4.3

- `StaticTest/StaticTest.java`

```Java
/**
* This program demonstrates static methods
* @version 1.01 2004-02-19
* @author Cay Horstmann
**/
public class StaticTest {

    public static void main(String[] args) {

        // fill the staff array with three Employee objects
        Employee[] staff = new Employee[3];

        staff[0] = new Employee("Tom", 40000);
        staff[1] = new Employee("Dick", 60000);
        staff[2] = new Employee("Harry", 65000);

        // print out information about all Employee objects
        for (Employee e : staff) {
            e.setId();
            System.out.println("name=" + e.getName() + ",id=" + e.getId() + ",salary=" + e.getSalary());
        }

        int n = Employee.getNextId(); // calls static method
        System.out.println("Next available id=" + n);
    }
}

class Employee {

    private static int nextId = 1;

    private String name;
    private double salary;
    private int id;

    public Employee(String n, double s) {
        name = n;
        salary = s;
        id = 0;
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

    public void setId() {
        id = nextId; // set id to next available id
        nextId++;
    }

    public static int getNextId() {
        return nextId; // returns static field
    }

    public static void main(String[] args) {
        Employee e = new Employee("Harry", 50000);
        System.out.println(e.getName() + " " + e.getSalary());
    }
}
```