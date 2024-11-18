# Documentation Comments

- [Comment Insertion]()
- [Class Comments]()
- [Method Comments]()
- [Field Comments]()
- [General Comments]()
- [Package and Overview Comments]()
- [Comment Extraction]()

The JDK contains a very useful tool, called `javadoc`, that generates HTML documentation from your source files. In fact, the online API documentation that we described in Chapter 3 is simply the result of running `javadoc` on the source code of the standard Java library.

If you add comments that start with the special delimiter `/**` to your source code, you too can easily produce professional-looking documentation. This is a very nice approach because it lets you keep your code and documentation in one place. If you put your documentation into a separate file, then, as you probably know, the code and comments tend to diverge over time. When documentation comments are in the same file as the source code, it is an easy matter to update both and run `javadoc` again.

## Comment Insertion
