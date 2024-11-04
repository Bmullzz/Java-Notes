# Arrays

- [The "for each" Loop](#the-for-each-loop)
- [Array Initalizers and Anonymous Arrays]()
- [Array Copying]()
- [Command-Line Parameters]()
- [Array Sorting]()
- [Multidimensional Arrays]()
- [Ragged Arrays]()

An array is a data structure that stores a collection of values of the same type. You access each individual value through integer _index_. For example, if `a` is an array of integers, then `a[i]` is the `i`th integer in the array.

Declare an array variable by specifying the array type - which is the element type followed by `[]` - and the array variable name. For example, here is the declaration of an array `a` of integers:

```Java
int[] a;
```

However, this statement only declares the cariable `a`. It does not yet initialize `a` with an actual array. Use the `new` operator to create the array.

```Java
int[] a = new int[100];
```

This statement declares and initializes an array of `100` integers. 

The array length need not be a constant: `new int[n]` creates an array of length `n`.

- **NOTE**: You can define an array variable either as

    ```Java
    int[] a;
    ```

- or as

    ```Java
    int a[];
    ```

- Most Java programmers prefer the former style because it neatly separates the type `int[]` (integer array) from the variable name.

The array elements are numbers are _numbered from `0` to `99`_ (and not `1` to `100`). Once the array is created, you can fill the elements in an array, for example, by using a loop:

```Java
int[] a = new int[100];
for (int i = 0; i < 100; i++) {
    a[i] = i; // fills the array with numbers 0 to 99
}
```

When you create an array of numbers, all elements are initialized with zero. Arrays of `boolean` are initialized with `false`. Arrays of objects are initialized with the special value `null`, which indicates that they do not (yet) hold any objects. This can be surprising for beginners. For example,

```Java
String[] names = new String[10];
```

creates an array of ten strings, all of which are `null`. If you want the array to hold empty strings, you must supply them:

```Java
for (int i = 0; i < 10; i++) names[i] = "";
```

- **CAUTION**: If you construct an array with 100 elements and then try to access the element `a[100]` (or any other index outside the reange from `0` to `99`), your program will terminate with an "aray index out of bounds" exception.

To find the number of elements of an array, use `array.length`. For example:

```Java
for (int i = 0; i < a.length; i++) {
    System.out.println(a[i]);
}
```

Once you create an array, you cannot change its size (although you can, of course, change an individual array element). If you frequently need to expand the size of an array while your program is running, you should use a different data structure called an _array list_.

## The "for each" Loop

