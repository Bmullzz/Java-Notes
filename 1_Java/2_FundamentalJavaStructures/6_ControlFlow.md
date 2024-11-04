# Control Flow

- [Block Scope](#block-scope)
- [Conditional Statements](#conditional-statements)
- [Loops](#loops)
- [Determinate Loops](#determinate-loops)
- [Multiple Selections - The `switch` Statement]()
- [Statements That Break Control Flow]()

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

