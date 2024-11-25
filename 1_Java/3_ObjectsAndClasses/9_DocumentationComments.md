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

The `javadoc` utility extracts information for the following items:

- Packages
- Public classes and interfaces
- Public and protected fields
- Public and protected constructors and methods

Protected features are introduced in the next chapter, interfaces the chapter after that.

You can (and should) supply a comment for each of these features. Each comment is placed immediately _above_ the feature it describes. A comment starts with a `/**` and ends with a `*/`.

Each `/**...*/` documentation comment contains _free-form text_ followed by _tags_. A tag starts with an `@`, such as `@author` or `@param`.

The _first sentence_ of the free-form text should be a _summary statement_. The `javadoc` utility automatically generates summary pages that extract these sentences.

new

