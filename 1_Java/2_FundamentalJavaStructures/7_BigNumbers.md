# Big Numbers

If the precision of the basic integer and floating-point types is not sufficient, you can turn to a couple of handy classes in the `java.math` package: `BigInteger` and `BigDecimal`. These are classes for manipulating numbers with an arbitrarily long sequence of digits. The `BigInteger` class implements arbitrary-precision integer arithmetic, and `BigDecimal` does the same for floating-point numbers.

Use the static `valueOf` method to turn an ordinary number into a big number:

```Java
BigInteger a = BigInteger.valueOf(100);
```

Unfortunately, you cannot use the familar mathematical operators such as `+` and `*` to combine big numbers. Instead, you must use methods such as `add` and `multiply` in the big number classes.

```Java
BigInteger c = a.add(b); // c = a + b
BigInteger d = c.multiply(b.add(BigInteger.valueOf(2))); // d = c * (b + 2)
```

- **NOTE**: Unlike C++, Java has no programmabale operator overloading. There was no way for the programmers of the `BigInteger` class to redefine the `+` and `*` operators to give the `add` and `multiply` operations of the `BigInteger` classes. The language designers did overload the `+` operator to denote concatenation of strings. They chose not to overload other operators, and they did not give Java programmers the opportunity to overload operators in their own classes.

[Lisitng 3.6](#listing-36) shows a modification of the lottery odds program of Listing 3.5, updated to work with big numbers. For example, if you are invited to participate ina lottery in which you need to pick `60` numbers out of a possible `490` numbers, you can use this program to tell you your odds of winning. They are `1` in `716395843461995557415116222540092933411717612789263493493351013459481104668848`. Good Luck!

The program in Listing 3.5 computed the statement

```Java
lotteryOdds = lotteryOdds * (n - i + 1) / i;
```

When big numbers are used, the equivalent statement becomes

```Java
lotteryOdds = lotteryOdds.multiply(BigInteger.valueOf(n - 1 + i)).divide(BigInteger.valueOf(i));
```

### Listing 3.6

- `BigIntegerTest/BitIntegerTest.java`

```Java
import java.math.*;
import java.util.*;

/**
 * This program uses big numbers to compute the odds of winning the grand prize in a lottery.
 * @version 1.20 2004-02-10
 * @author Cay Horstman
 **/
public class BigIntegerTest {

    public static void main(String[] args) {

        Scanner in = new Scanner(System.in);

        System.out.print("How many numbers do you need to draw? ");
        int k = in.nextInt();

        System.out.print("What is the highest number you can draw? ");
        int n = in.nextInt();

        /**
         * compute binomial coefficient n*(n-1)*(n-2)*...*(n-k+1)/(1*2*3*...*k)
         **/

        BigInteger lotteryOdds = BigInteger.valueOf(1);

        for (int i = 1; i <= k; i++) {
            lotteryOdds = lotteryOdds.multiply(BigInteger.valueOf(n - i + 1)).divide(BigInteger.valueOf(i));
        }

        System.out.println("Your odds are 1 in " + lotteryOdds + ". Good luck!");
    }
}
```

- `java.math.BigInteger` 1.1

    - `BigInteger add(BigInteger other)`

    - `BigInteger subtract(BigInteger other)`

    - `BigInteger multiply(BigInteger other)`

    - `BigInteger divide(BigInteger other)`

    - `BigInteger mod(BigInteger other)`

        - _returns_ the _sum_, _difference_, _product_, _quotient_, and _remainder_ of this big integer and `other`.

    - `int compareTo(BigInteger other)`

        - _returns_ `0` if this big integer equals `other`, a _negative_ result if this big integer is less than `other`, and a _positive_ result otherwise.

    - `static BigInteger valueOf(long x)`

        - _returns_ a big integer whose value equals `x`.

    - `BigDecimal add(BigDecimal other)`

    - `BigDecimal subtract(BigDecimal other)`

    - `BigDecimal multiply(BigDecimal other)`

    - `BigDecimal divide(BigDecimal other, RoundingMode mode)`
        - _returns_ the _sum_, _difference_, _product_, or _quotient_ of this big decimal and `other`. To compute the quotient, you must supply a _rounding mode_. The mode `RoundingMode.HALF_UP` is the rounding mode that you learned in school: round down the digits `0` to `4`, round up the digits `5` to `9`. It is appropriate for routine calculations. See the API documentation for other rounding modes.

    - `int compareTo(BigDecimal other)`

        - returns `0` if this big decimal equals `other`, a _negative_ result if this big decimal is less than `other`, and a _positive_ result otherwise.

    - `static BigDecimal valueOf(long x)`

    - `static BigDecimal valueOf(long x, int scale)`

        - returns a big decimal whose value equals `x` or `x` / $10^{scale}$.
