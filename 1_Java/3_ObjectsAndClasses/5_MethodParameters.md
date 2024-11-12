# Method Parameters

Let us review the computer science terms that describe how parameters can be passed to a method (or a function) in a programming language. The term _call by value_ means that the method gets the _location_ of the variable that the caller provides. Thus, a method can _modify_ the value stored in a variable passed by reference but not in one passed by value. These "call by ..." terms are standard computer science terminology describing the behavior of method parameters iin various programming languages, not just Java. (There is also a _call by name_ that is mainly of historical interest, being employed in the Algol programming language, one of the oldest high-level languages.)

The Java programming language _always_ uses call by value. That means that the method gets a copy of all parameter values. In particular, the method cannot modify the contents of any parameter variables passed to it.

For example, consider the following call:

```Java
double percent = 10;
harry.raiseSalary(percent);
```

No matter how the method is implemented, we know that after the method call, the value of `percent` is still `10`.

Let us look a little more closely at this situation. Suppose a method tried to triple the value of a method parameter:

```Java 
public static void tripleValue(double x) { // doesn't work
    x = 3 * x;
}
```

Let's call this method:

```Java
double percent = 10;
tripleValue(percent);
```

However, this does not work. After the method call, the value of `percent` is still `10`. Here is what happens:

1. `x` is initialized with a copy of the value of `percent` (that is, `10`).

2. `x` is tripled - it is now `30`. But `percent` is still `10`.

3. the method ends, and the parameter variable `x` is no longer in use.

There are, however, two kinds of method parameters:

- Primitive types (numbers, `boolean` values)

- Object references

![numeric parameter](image.png)

- **Figure 4.6**: Modifying a numeric parameter has no lasting effect.

You have seen that it is impossible for a method to change a primitve for a method to change a primitve type parameter. The situation is different for object parameters. You can easily implment a method that triples the salary of an employee:

```Java
public static void tripleSalary(Employee x) { // works
    x.raiseSalary(200);
}
```

When you call

```Java
harry = new Employee(...);
tripleSalary(harry);
```

then the following happens:

1. `x` is initialized with a copy of the value of `harry`, that is, an object reference.

2. The `raiseSalary` method is applied to that object reference. The `Employee` object to which both `x` and `harry` refer gets its salary by 200 percent.

3. The method ends, and the parameter variable `x` is no longer in use. Of course, the object variable `harry` continues to refer to the object whose salary was tripled (see Figure 4.7).

![modify object parameter](image-1.png)

- **Figure 4.7**: Modifying an object parameter has a lasting effect.

As you have seen, it is easily possible - and in fact very common - to implement methods that change the state of an object parameter. The reason is simple. The method gets a copy of the object reference, and both the original and the copy refer to the same object.

Many programming languages (in particular, C++ and Pascal) have two mechanisms for parameter passing: call by value and call by reference. Some programmers (and unfortunately even some book authors) claim that Java uses call by reference for objects. That is false. As this is such a common misunderstanding, it is worth examining a counterexample in detail.

Let's try to write a method that swaps two employee objects:

```Java
public static void swap(Employee x, Employee y) { // doesn't work
    Employee temp = x;
    x = y;
    y = temp;
}
```

If Java used call by reference for objects, this method would work:

```Java
Employee a = new Employee("Alice", ...);
Employee b = new Employee("Bob", ...);
swap(a, b);
// does a now refer to Bob, b to Alice?
```

However, the method does not actually change the object references that are stored in the variables `a` and `b`. The `x` and `y` parameters of the `swap` method are initialized with _copies_ of these references. The method then proceeds to swap these copies.

```Java
// x refers to Alice, y to Bob
Employee temp = x;
x = y;
y = temp;
// now x refers to Bob, y to Alice
```

But ultimately, this is a wasted effort. Then the method ends, the parameter variables `x` and `y` are abandoned. The original variables `a` and `b` still refer to the same objects as they did before the method call (see Figure 4.8).

![object swapping](image-2.png)

- **Figure 4.8**: Swapping object parameters has no lasting effect.

This demonstrates that the Java programming language does not use call by reference for objects. Instead, _object references are passed by value_.

Here is a summary of what you can and cannot do with method parameters if Java:

- A method cannot modify a parameter of a primitve type (that is, numbers or `boolean` values).

- A method can change the _state_ of an object parameter.

- A method cannot make an object parameter refer to a new object.

The program in [Listing 4.4](#listing-44) demonstrates these facts. The program first tries to triple the value of a number parameter and does not succeed:

```Java
Testing tripleValue:
Before: percent=10.0
End of method: x=30.0
After: percent=10.0
```

It then successfully triples the salary of an employee:

```Java
Testing tripleSalary:
Before: salary=50000.0
End of method: salary=150000.0
After: salary=10.0
```

After the method, the state of the object to which `harry` refers has changed. This is possible because the method modified the state through a copy of the object reference.

Finally, the program demonstrates the failure of the `swap` method:

```Java
Testing swap:
Before: a=Alice
Before: b=Bob
End of method: x=Bob
End of method: y=Alice
After: a=Alice
After: b=Bob
```

As you can see, the parameter variables `x` and `y` are swapped, but the variables `a` and `b` are not affected.

- ***C++ Note**: C++ has both call by value and call by reference. You tag reference parameters with `&`. For example, you can easily implement methods `void tripleValue(double& x)` or `void swap(Employee& x, Employee& y)` that modify their reference parameters.

### Listing 4.4

- `ParamTest/ParamTest.java`

```Java
/**
 * This program demonstrates parameter passing in Java.
 * @version 1.00 2000-01-27
 * @author Cay Horstmann
 **/
public class ParamTest {

    public static void main(String[] args) {

        /*
         * Test 1: Methods can't modify numeric parameters
         */
        System.out.println("Testing tripleValue:");
        double percent = 10;
        System.out.println("Before: percent=" + percent);
        tripleValue(percent);
        System.out.println("After: percent=" + percent);

        /*
         * Test 2: Methods can change the state of object parameters
         */
        System.out.println("\nTesting tripleSalary:");
        Employee harry = new Employee("Harry", 50000);
        System.out.println("Before: salary=" + harry.getSalary());
        tripleSalary(harry);
        System.out.println("After: salary=" + harry.getSalary());

        /*
         * Test 3: Methods can't attach new objects to object parameters
         */
        System.out.println("\nTesting swap:");
        Employee a = new Employee("Alice", 70000);
        Employee b = new Employee("Bob", 60000);
        System.out.println("Before: a=" + a.getName());
        System.out.println("Before: b=" + b.getName());
        swap(a, b);
        System.out.println("After: a=" + a.getName());
        System.out.println("After: b=" + b.getName());
    }

    public static void tripleValue(double x) { // doesn't work
        x = 3 * x;
        System.out.println("End of method: x=" + x);
    }

    public static void tripleSalary(Employee x) { // works
        x.raiseSalary(200);
        System.out.println("End of method: salary=" + x.getSalary());
    }

    public static void swap(Employee x, Employee y) {
        Employee temp = x;
        y = temp;
        System.out.println("End of method: x=" + x.getName());
        System.out.println("End of method: y=" + y.getName());
    }
}

class Employee { // simplified Employee class

    private String name;
    private double salary;

    public Employee(String n, double s) {
        name = n;
        salary = s;
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public void raiseSalary(double byPercent) {
        double raise = salary * byPercent / 100;
        salary += raise;
    }
}
```
