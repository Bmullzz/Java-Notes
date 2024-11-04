# Control Flow

- [Block Scope](#block-scope)
- [Conditional Statements](#conditional-statements)
- [Loops](#loops)
- [Determinate Loops](#determinate-loops)
- [Multiple Selections - The `switch` Statement](#multiple-selections---the-switch-statement)
- [Statements That Break Control Flow](#statements-that-break-control-flow)

Java, just like any programming language, supports both conditional statements and loops to determine control flow. We will start with the conditional statements, then move on to loops, to end with the somewhat cumbersome `switch` statement that you can use to test for many values of a single expression.

## Block Scope

Before learning about control structures, you need to know more about _blocks_.

A block or compound statement consists of a number of Java statements, surrounded by a pair of braces. Blocks define the scope of your variables. A block can be _nested_ inside another block. Here is a block that is nested inside the block of the `main` method:

```Java
public static void main(String[] args) {
    int n;
    function {
        int k;
        innerFunction {

        } // k is only defined up to here
    }
}
```

You may not declare identically named variables in two nested blocks. For example, the following is an error and will not compile:

```Java
public static void main(String[] args) {
    int n;
    function {
        int k;
        int n; // Error - can't redefine n in inner block
    }
}
```

- **NOTE**: In some languages, it is possible to redefine a variable inside a nested block. The inner definition _shadows_ the outer one. This can be a source of programming _errors_; hence, Java does not allow it.

## Conditional Statements

The conditional statement in JAva has the form

```Java
if (condition) statement
```

The condition must be surrounded by parentheses.

In JAva, as in most programming languages, you will often want to execute multiple statements when a single condition is true. In this case, use a _block statement_ that takes the form 

```Java
{
    statement1
    statement2
    ...
}
```

For example:

```Java
if (yourSales >= targewt) {
    performance = "Satisfactory";
    bonus = 100;
}
```

In this code, all the statements surrounded by the braces will be executed when `yourSales` is greater than or equal to `target`;

- **NOTE**: A block (sometimes called a _compound statement_) enables you to have more than one (simple) statement in any Java programming structure that otherwise allows for a single (simple) statement.

The more general conditional in Java looks like this:

```Java
if (condition) statement1 else statement2
```

For example:

```Java
if (yourSales >= target) {
    performance = "Satisfactory";
    bonus = 100 + 0.01 * (yourSales - target);
} else {
    performance = "Unsatisfactory";
    bonus = 0;
}
```

The `else` part is always optional. An `else` groups with the closest `if`. Thus, in the statement

```Java
if (x <= 0) if (x == 0) sign = 0; else sign = -1;
```

the `else` belings to the second `if`. Of course, it is a good idea to use braces to clarify this code:

```Java
if (x = 0) { if (x == 0) sign = 0; else sign = -1; }
```

Repeated `if... else if ...` alternatives are common. For example:

```Java
if (yourSales >= 2 * target) {
    performance = "Excellent";
    bonus = 1000;

} else if (yourSales >= 1.5 * target) {
    performance = "Fine";
    bonus = 500;

} else if (yourSales >= target) {
    performance = "Satisfactory";
    bonus = 100;

} else {
    System.out.println("You're fired");
}
```

## Loops

The `while` loop executes a statement (which may be a block statement) while a condition is `true`. The general form is 

```Java
while (condition) statement
```

The `while` loop will never execute if the condition is `false` at the outset. The program in [Listing 3.3](#listing-33), below, determines how long it will take to save a specific amount of money for your well-earned retirement, assuming you deposit the same amount of money per year and the money earns a specified interest rate.

In the examples, we are the body of the loop until the total exceeds the targeted amount. 

```Java
while (balance < goal) {
    balance += payment;
    double interest = balance * interestRate / 100;
    balance += interest;
    years++;
}
System.out.println(years + " years.");
```

(Don't rely on this program to plan for your retirement. We left out a few niceties such as inflation and your life expectancy.)

A `while` loop tests at the top. Therefore, the code in the block might never be executed. If you want to make sure a block is executed at least once, you need to move the test to the bottom, using the `do`/`while` loop. Its syntax looks like this:

```Java
do statement while (condition);
```

This loop executes the statement (which is typically a block) and only then tests the condition. If it's `true`, it repeats the statement and retests the condition, and so on. The code in [Listing 3.4](#listing-34) computes the new balance in your retirement account and then asks if you are ready to retire:

```Java
do {
    balance += payment;
    double interest = balance * interestRate / 100;
    balance += interest;
    year++;
    // print current balance
    ...
    // ask if ready to retire and get input
    ...
}
while (input.equals("N"));
```

As long as the user answers "N", the loop is repeated. This program is a good example of a loop that needs to be entered at least once, because the user needs to see the balance before deciding whether it is sufficient for retirement.

### Listing 3.3

- `Retirement/Retirement.java`

```Java
import java.util.*;

/**
 *  This program demonstrates a while loop.
 * @version 1.20 2004-02-10
 * @author Cay Horseman
 **/
 public class Retirement {
    
    public static void main(String[] args) {

        //read inputs
        Scanner in = new Scanner(System.in);

        System.out.print("How much money do you need to retire? ");
        double goal = in.nextDouble();

        System.out.print("How much money will you contribute every year? ");
        double payment = in.nextDouble();

        System.out.print("Interest rate in %: ");
        double interestRate = in.nextDouble();

        double balance = 0;
        int years = 0;

        // update account balance while goal isn't reached
        while (balance < goal) {
            // add this year's payment and interest
            balance += payment;
            double interest = balance * interestRate / 100;
            balance += interest;
            years++;
        }

        System.out.println("You can retire in " + years + " years.");
    }
 }
```

### Listing 3.4

- `Retirement2/Retirement2.java`

```Java
import java.util.*;

/**
 *  This program demonstrates a do/while loop.
 * @version 1.20 2004-02-10
 * @author Cay Horseman
 **/
 public class Retirement2 {
    
    public static void main(String[] args) {

        //read inputs
        Scanner in = new Scanner(System.in);

        System.out.print("How much money will you contribute every year? ");
        double payment = in.nextDouble();

        System.out.print("Interest rate in %: ");
        double interestRate = in.nextDouble();

        double balance = 0;
        int years = 0;

        String input;
        
        // update account balance while user isn't ready to retire

        do {

            // add this year's payment and interest
            balance += payment;
            double interest = balance * interestRate / 100;
            balance += interest;

            year++;

            // print current balance
            System.out.printf("After year %d, your balance is %,.2f%n", year, balance);

            // ask if ready to retire and get input
            System.out.print("Ready to retire? (Y/N) ");
            input = in.next();
        }
        while (input.equls("N"));
    }
 }
```

## Determinate Loops

The `for` loop is a general construct to support iteration controlled by a counter or similar variable that is updated after every iteration. The following loop prints the numbers from `1` to `10` on the screen.

```Java
for (int i = 1; i <= 10; i++)
    system.out.println(i);
```

The first slot of the `for` statement usually holds the counter initialization. The second slot gives the condition that will be tested before each new pass through the loop, and the third slot specifies how to update the counter.

Although Java, like C++, allows almost any expression in the various slots of a `for` loop, it is an unwritten rule of good taste that the three slots should only initialize, test, and update the same counter variable. One can write very obscure loops by disregarding this rule.

Even within the bounds of good taste, much is possible. For example, you can have loops that count down:

```Java
for (int i = 10; i > 0; i--) 
    System.out.println("Counting down ... " + i);
System.out.println("Blastoff");
```

- **CAUTION**: Be careful about testing for equality of floating-point numbers in loops. A `for` loop like this one

```Java
for (double x = 0; x != 10; x += 0.1) ...
```

- might never end. Because of the roundoff errors, the final value might not be reached exactly. In this example, `x` jumps from `9.99999999999998` to `10.09999999999998` because there is no exact binary representation for `0.1`.

When you declare a variable in the first slot of the `for` statement, the scope of that variable extends until the end of the body of the `for` loop.

```Java
for (int i = 1; i <= 10; i++) {
    ...
}
// i no longer defined here
```

In particular, if you define a variable inside a `for` statement, you cannot use its value outside the loop. Therefore, if you wish to use the final value of a loop counter outside the `for` loop, be sure to declare it outside the loop header.

```Java
int i;
for (i = 1; i <= 10; i++) {
    ...
}
// i is still defined here
```

On the other hand, you can define variables with the same name in separate `for` loops:

```Java
for (int i = 1; i <= 10; i++) {
    ...
}

for (int i = 11; i <= 20; i++) { // OK  to define another variable named i
    ...
}
```

A `for` loop is merely a convenient shortcut for a `while` loop. For example,

```Java
for (int i = 10; i > 0; i--)
    System.out.println(""Counting down... " + i);
```

can be rewritten as

```Java
int i = 10;
while (i > 0) {
    System.out.println("Counting down... " + i);
    i--;
}
```

[Lisitng 3.5](#listing-35) shows a typical example of a `for` loop.

The program computes the odds of winning a lottery. For example, if you must pick six numbers from the numbers `1` to `50` to win, then there are $(50\times49\times48\times47\times46\times45)/(1\times2 \times3\times4\times5\times6)$ possible outcomes, so your chance is `1` in `15,890,700`. Good luck!

In general, if you pick `k` numbers out of `n`, there are 

$$\frac{n\times(n-1)\times(n-2)\times...\times(n-k+1)}{1\times2\times3\times4\times...\times k}$$

possible outcomes. The following `for` loop computes this value:

```Java
int lotteryOdds = 1;
for (int i = 1; i <= k; i++)
    lotteryOdds = lotteryOdds * (n - i + 1) / i;
```

### Listing 3.5

- `LotteryOdds/LotteryOdds.java`

```Java
import java.util.*;

/**
 * This program demonstrates a for loop.
 * @version 1.20 2004-02-10
 * @author Cay Horstman
 * */
public class LotteryOdds {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        System.out.print("How many numbers do you need to draw? ");
        int k = in.nextInt();

        System.out.print("What is the highest number you can draw? ");
        int n = in.nextInt();

        /**
         * compute binomial coefficient n*(n-1)*(n-2)*...*(n-k+1)/(1*2*3*...*k)
         **/

        int lotteryOdds = 1;
        for (int i = 1; i <= k; i++) {
            lotteryOdds = lotteryOdds * (n - i + 1) / i;
        }

        System.out.println("Your odds are 1 in " + lotteryOdds + ". Good luck!");
    }
}
```

## Multiple Selections - The `switch` Statement

The `if`/`else` construct can be cumbersome when you have to deal with multiple selections with many alternatives. Java has a `switch` statement that is exactly like the `switch` statement in C and C++, warts and all.

For example, if you set up a menu system with four alternatives like that in a previous section, you could use code that looks like this:

```Java
Scanner in = new Scanner(System.in);
System.out.print("Select an option (1, 2, 3, 4) ");
int choice = in.nextInt();
switch (choice) {
    
    case 1:
        ...
        break;
    case 2:
        ...
        break;
    case 3:
        ...
        break;
    case 4:
        ...
        break;
    default:
        // bad input
        ...
        break;
}
```

Execution starts at the `case` label that matches the value on which the selection is performed and continues until the next `break` or the end of the switch. If none of the case labels match, then the `default` clause is executed, if it is present.

- **CAUTION**: It is possible for multiple alternatives to be triggered. If you forget to add a `break` at the end of an alternative, execution falls through to the next alternative! This behavior is plainly dangerous and a common casue for errors. For that reason, we never use the `switch` statement in our programs.

- If you like the `switch` statement better than we do, consider compiling your code with the `-Xlint:fallthrough Test.java` option, like this:

```terminal
javac -Xlint:fallthrough Test.java
```
- Then the compiler will issue a warning whenever an alternative does not end with a `break` statement.

- If you actually want to use the fallthrough behavior, tag the surrounding method with the annotation `@SuppressWarnings("fallthrough")`. Then no warnings will be generated for that method. (An annotation is a mechanism for supplying information to the compiler or a tool that processes Java source or class files.)

A `case` label can be 

- A constant expression of type `char`, `byte`, `short`, or `int`
- An enumerated constant
- Starting ith Java SE 7, a string literal

For example,

```Java
String input = ...;
switch (input.toLowerCase()) {
    case "yes": // OK since Java SE 7\
        ...
        break;
    ...
}
```

When you use the `switch` statement with enumerated constants, you need not supply the name of the enumeration in each label - it is deduced from the `switch` value. For example,

```Java
Size sz = ...;
switch (sz) {
    case SMALL: // no need to use Size.SMALL
        ...
        break;
    ...
}
```

## Statements That Break Control Flow

Although the designers of Java kept `goto` as a reserved word, they decided not to include it in the language. In general, `goto` statements are considered poor style. Some programmers feel the anti-`goto` forces have gone too far (see, for example, the famous article of Donald Knuth called "Structured Programming with `goto` statements"). They argue that unrestricted use of `goto` is error-prone but that an occasional jump _out of loop_ is beneficial. The Java designers agreed and even added a new statement, the labeled break, to support this programming style.

Let us first look at the unlabeled `break` statement. The same `break` statement that you use to exit a `switch` can also be used to break out of a loop. For example:

```Java
while (years <= 100) {
    balance += payment;
    double interest = balance * interestRate / 100;
    balance += interest;
    if (balance >= goal) break;
    years++;
}
```

Now the loop is exited if either `years > 100` occurs at the top of the loop or `balance >= goal` occurs in the middle of the loop. Of course, you could have computed the same value for `years` without a `break`, like this:

```Java
while (years <= 100 && balance < goal) {
    balance += payment;
    double interest = balance * interestRate / 100;
    balance += interest;
    if (balance < goal)
        years++;
}
```

But note that the test `balance < goal` is repeated twice in this version. To avoid this repeated test, some programmers prefer the `break` statement.

Unlike C++, Java also offers a _labeled break_ statement that lets you break out of multiple nested loops. Occasionally something weird happens inside a deeply nested loop. In that case, you may want to break completely out of all the nested loops. It is inconvenient to program that simply by adding extra conditions to the various loop tests.

Here's an example that shows the break statement at work. Notice that the label must procede the outermost loop out of which you want to break. It also must be followed by a colon.

```Java
Scanner in = new Scanner(System.in);
int n;
read_data:
while (...) { // this loop statement is tagged with the label 
    ...
    for (...) { // this inner loop is not labeled
        System.out.print("Enter a number >= 0: ");
        n = in.nextInt();
        if (n < 0) // should never happen-can't go on
            break read_data;
            // break out of read_data loop
        ...
    }
}
// this statement is executed immediately after the labeled break
if (n < 0) { // check for bad situation
    // deal with bad precision
} else {
    // carry out normal processing
}
```

IF there is a bad input, the labeled break moves past the end of the labeled block. As with any use of the `break` statement, you then need to test whether the loop exited normally or as a result of a break.

- **NOTE**: Curiously, you can apply a label to any statement, even an `if` statement or a block statement, like this:

```Java
label: {
    ...
    if (condition) break label; // exits block
    ...
}
// jumps here when the break statement executes
```

- Thus, if you are lusting after a `goto` and if you can place a block that ends just before the place to which you want to jump, you can use a `break` statement! Naturally, we don't recommend this approach. Note, however, that you can only jump _out of_ a block, never _into_ a block.

Finally, there is a `continue` statement that, like the `break` statement, breaks the regular flow of control. The `continue` statement transfers control to the header of the innermost enclosing loop. Here is an example:

```Java
Scanner in = new Scanner(System.in);
while (sum < goal) {
    System.out.print("Enter a number: ");
    n = in.nextInt();
    if (n < 0) continue;
    sum += n; // not executed if n < 0
}
```

If `n < 0`, then the `continue` statement jumps immediately to the loop header, skipping the remainder of the current iteration.

If the `continue` statement is used in a `for` loop, it jumps to the "update" part of the `for` loop. For example:

```Java
for (count = 1; count <= 100; count++) {
    System.out.print("Enter a number, -1 to quit: ");
    n = in.nextInt();
    if (n < 0) continue;
    sum += n; // not executed if n < 0
}
```

If `n < 0`, then the `continue` statement jumps to the `count++` statement.

There is also a labeled form of the `continue` statement that jumps to the header of the loop with the matching label.

- **Tip**: Many programmers find the `break` and `continue` statements confusing. These statements are entirely optional - you can always express the same logic without them. In this book, we never use `break` or `continue`.

