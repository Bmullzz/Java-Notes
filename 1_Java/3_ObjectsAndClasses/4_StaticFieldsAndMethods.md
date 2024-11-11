# Static Fields and Methods

- [Static Fields](#static-fields)
- [Static Constants](#static-constants)
- [Static Methods]()
- [Factory Methods]()
- [The `main` Method]()

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