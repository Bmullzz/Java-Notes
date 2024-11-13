# The Class Path

- [Setting the Class Path]()

As you have seen, classes are stored in subdirectories of the file system. The path to the class must match the package name.

Class files can also be stored in a JAR (Java archive) file. A JAR file contains multiple class files and subdirectories in a compressed format, saving space and improving performance. When you use a third-party library in your programs, you will usually be given one or more JAR files to include. The JDK also supplies a number of JAR files, such as the file `jre/lib.rt.jar` that contains thousands of library classes. You will see in Chapter 9 how to create your own JAR files.

- **Tip**: Jar files use the ZIP format to organize files and subdirectories. You can use any ZIP utility to peek inside `rt.jar` and other JAR files.

To share classes among programs, you need to do the following:

1. Place your class files inside a directory, for example, `/home/user/classdir`. Note that this directory is the _base_ directory for the package tree. If you add the class `com.horstmann.corejava.Employee`, then the `Employee.class` file must be located in the subdirectory `/home/user/classdir/com/horstmann/corejava`.

2. Place any JAR files inside a directory, for example, `/home/user/archives`.

3. Set the _class path_. The class path is the collection of all locations that can contain class files.

In UNIX, the elements on the class path are separated by colons:

```Unix
/home/user/classdir:.:/home/user/archives/archive.jar
```

In Windows, they are separated by semicolons:

```
c:\classdir;.;c:\archives\archive.jar
```

In both cases, the period denotes the current directory.

This class path contains

- The base directory `/home/user/classdir` or `c:\classdir;`
- The current directory (.); and
- The JAR file `/home/user/archives/archive.jar` or `c:\archives\archive.jar`.

Staritng with Java SE 6, you can specify a wildcard for a JAR file directory, like this class path.

The runtime library files (`rt.jar` and the other JAR files in the `jre/lib` and `jre/lib/ext` directories) are always searched for classes; don't include them explicitly in the class path.

- **CAUTION**: The `javac` compiler always lookes for files in the current directory, but the `java` virtual machine launcher only looks into the current directly if the "." directory is on the class path. If you have no class path set, this is not a problem - the default class path consists of the "." directory. But if you have set the class path and forgot to include the "." directory, your programs will compile without error, but they won't run.

