# Packages

- [Class Importation](#class-importation)
- [Static Imports](#static-imports)
- [Addition of a Class into a Package]()
- [Package Scope]()

Java allows you to group classes in a collection called a _package_. Packages are convenient for organizing your work and for separating your work from code libraries provided by others.

The standard Java library is distributed over a number of packages, including `java.lang`, `java.util`, `java.net`, and so on. The standard Java packages are examples of hierarchal packages by using levels of nesting. All standard Java packages are inside the `java` and `javax` package hierarchies.

The main reason for using packages is to guarantee the uniqueness of class names. Suppose two programmers come up with the bright idea of supplying an `Employee` class. As long as both of them place their class into different packages, there is no conflict. In fact, to absolutely a unique package name, use an Internet domain name (which is known to be unique) written in reverse. You then use subpackages for different projects. For example, consider the domain `horstmann.com`. When written in reverse order, it turns into the package `com.horstmann`. That package can then be further subdivided into subpackages such as `com.horstmann.corejava`.

From the point of view of the compiler, there is absolutely no relationship between nested packages. For example, the packages `java.util` and `java.util.jar` have nothing to do with each other. Each is its own independent collection of classes.

## Class Importation

A class can use all classes from its own package and all _public_ classes from other packages.

You can access the public classes in another package in two ways. The first is simply to add the full package name in front of _every_ class name. For example:

```Java
java.time.LocalDate today = java.time.LocalDate.now();
```

That is obviously tedious. A simpler, and more common, approach is to use the `import` statement. The point of the `import` statement is to give you a shorthand to refer to the classes in the package. Once you use `import`, you no longer have to give the classes their full names.

You can import a specific class or the whole package. You place `import` statements at the top of your source files (but below and `package` statements). For example, you can import all classes in the `java.util` package with the statement

```Java
import java.util.*;
```

Then you use

```Java
LocalDate today = LocalDate.now();
```

The `java.time.*` syntax is less tedious. It has no negative effect on code size. However, if you import classes explicitly, the reader of your code knows exactly which classes you use.

- **Tip**: In Eclipse, you can select the menu option Source -> Organize Imprts. Package statements such as `import java.util.*;` are automatically expanded into a list of specific imports such as 

    ```Java
    import java.util.ArrayList;
    import java.util.Date;
    ```

    This is an extremely convenient feature.

However, not that you can only use the `*` notation to import a single package. You cannot use `import java.*` or `import java .*.*` to import all packages with the `java` prefix.

Most of the time, you just import the packages that you need, without worrying too much about them. The only time that you need to pay attention to packages is when you have a name conflict. For example, both the `java.util` and `java.sql` packages have a `Date` class. Suppose you write a program that imports both packages.

```Java
import java.util.*;
import java.sql.*;
```

If you want to use the `Date` class, you get a compile-time error:

```Java
Date today; // Error--java.util.Date or java.sql.Date?
```

The compiler cannot figure out which `Date` class you want. You can solve this problem by adding a specific `import` statement:

```Java
import java.util.*;
import java.sql.*;
import java.util.Date;
```

What if you really need both `Date` classes? Then you need to use the full package with every class name.

```Java
java.util.Date deadline = new java.util.Date();
java.sql.Date today = new java.sql.Date(...);
```

Locating classes in packages is an activity of the _compiler_. The bytecodes in class files always use full package names to refer to other classes.

## Static Imports

