# Enumerated Types and Strings

- Enumerated Types
- Strings
- Substrings
- Concatenation
- Strings are Immutable
- Testing Strings for Equality
- Empty and Null Strings
- Code Points and Code Units
- The String API

## Enumerated Types

Sometimes, a variable should only hold a restricted set of values. For example, you may sell clothes or pizza in four sizes: small, medium, large, and extra large. Of course, you could encode these sizes as integers `1`, `2`, `3`, `4` or characters `S`, `M`, `L`, and `X`. But that is an error-prone setup. It is too easy for a variable to hold a wrong value (such as `0` or `m`).

You can define your own _enumerated type_ whenever such a situation arises. An enumerated type has a finite number of named values. For example:

```Java
enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE };
```

Now you can declare variables of this type:

```Java
Size s = Size.MEDIUM;
```

A variable of type `Size` can hold only one of the values listed in the type declaration, or the special value `null` that indicates that the variable is not set to any value at all.