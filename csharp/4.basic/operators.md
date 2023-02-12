# Arithmetic Operators
The arithmetic operators (+, -, *, /, %) are defined for all numeric types except the 8- and 16-bit integral types:
```
+    Addition
-    Subtraction
*    Multiplication
/    Division
%    Remainder after division
```
# what is base 2 and base 10?

Base 2 and base 10 are numerical systems used to represent numbers in computing and mathematics.

`Base 10` is also known as the decimal system, and it uses 10 digits (0, 1, 2, 3, 4, 5, 6, 7, 8, 9) to represent all possible numerical values. Each digit in a decimal number represents a power of 10, with the rightmost digit representing 10^0 (1), the next digit to the left representing 10^1 (10), and so on. For example, the number 1234 in base 10 can be expanded as:

110^3 + 210^2 + 310^1 + 410^0

`Base 2` is also known as the binary system, and it uses only two digits (0 and 1) to represent numerical values. Each digit in a binary number represents a power of 2, with the rightmost digit representing 2^0 (1), the next digit to the left representing 2^1 (2), and so on. For example, the number 1010 in base 2 can be expanded as:

12^3 + 02^2 + 12^1 + 02^0

Base 2 is commonly used in computing because it is the language of digital electronics, where the binary state of a transistor or switch represents either 0 or 1. Binary numbers can also be used to represent numeric data in a more efficient manner than decimal numbers, especially in computer hardware and networking where memory and bandwidth are at a premium.

# WHen use base 2 and base 10?
Base 2 (binary) and base 10 (decimal) are both commonly used in computing and mathematics, but each has its own strengths and weaknesses depending on the context. Here are some general guidelines for when to use each base:

`Use base 10 when:`

Working with human-readable numbers that are based on the decimal system, such as currency, time, and measurements.

Performing calculations involving decimal fractions or numbers with a large number of digits.

Converting between different units of measure, such as inches to centimeters or pounds to kilograms.

Representing numbers in most programming languages and applications that use a decimal system.

`Use base 2 when:`

Working with digital electronics, such as computer hardware, networking, and embedded systems.

Working with data that can be represented as binary, such as machine language code or images.

Performing bitwise operations, such as bit shifting or setting individual bits in a binary value.

Representing numeric data in a compact and efficient way, such as in binary-encoded data formats or in computer memory.

In general, the choice of base depends on the specific needs of the problem at hand, as well as the context in which the numbers will be used.
# Increment and Decrement Operators
```c#
int x = 0, y = 0;
Console.WriteLine (x++);   // Outputs 0; x is now 1
Console.WriteLine (++y);   // Outputs 1; y is now 1
```

# Division

```c#
int a = 2 / 3;      // 0

int b = 0;
int c = 5 / b;      // throws DivideByZeroException

```

# Overflow
At runtime, arithmetic operations on integral types can overflow. By default, this happens silently—no exception is thrown, and the result exhibits “wraparound” behavior, as though the computation were done on a larger integer type and the extra significant bits discarded. For example, decrementing the minimum possible int value results in the maximum possible int value:

```c#
int a = int.MinValue;
a--;
Console.WriteLine (a == int.MaxValue); // True
```

# Overflow check operators
The `checked` operator instructs the runtime to generate an `OverflowException` rather than overflowing silently when an integral-type expression or statement exceeds the arithmetic limits of that type. The checked operator affects expressions with the ++, −−, +, − (binary and unary), *, /, and explicit conversion operators between integral types. `Overflow checking incurs a small performance cost.`

`Note`: The checked operator has no effect on the `double and float` types (which overflow to special “infinite” values, as you’ll see soon) and no effect on the decimal type (which is always checked).

```C#
int a = 1000000;
int b = 1000000;

int c = checked (a * b);      // Checks just the expression.

checked                       // Checks all expressions
{                             // in statement block.
   ...
   c = a * b;
   ...
}

```

If you then need to disable overflow checking just for specific expressions or statements, you can do so with the `unchecked` operator. For example, the following code will not throw exceptions—even if the project’s “checked” option is selected:
```C#
int x = int.MaxValue;
int y = unchecked (x + 1);
unchecked { int z = x + 1; }
```

# 8- and 16-Bit Integral Types
```C#
short x = 1, y = 1;
short z = x + y;          // Compile-time error
```

In this case, x and y are implicitly converted to int so that the addition can be performed. This means that the result is also an int, which cannot be implicitly cast back to a short (because it could cause loss of data). To make this compile, you must add an explicit cast:

```C#
short z = (short) (x + y);   // OK
```

# Special Float and Double Values
Unlike integral types, floating-point types have values that certain operations treat specially. These special values are NaN (Not a Number), +∞, −∞, and −0. The float and double classes have constants for NaN, +∞, and −∞, as well as other values (MaxValue, MinValue, and Epsilon); for example:
```C#
Console.WriteLine (double.NegativeInfinity);   // -Infinity
```

Dividing a nonzero number by zero results in an infinite value:

```c#
Console.WriteLine ( 1.0 /  0.0);                  //  Infinity
Console.WriteLine (−1.0 /  0.0);                  // -Infinity
Console.WriteLine ( 1.0 / −0.0);                  // -Infinity
Console.WriteLine (−1.0 / −0.0);                  //  Infinity
```
Dividing zero by zero, or subtracting infinity from infinity, results in a NaN:
```c#
Console.WriteLine ( 0.0 /  0.0);                  //  NaN
Console.WriteLine ((1.0 /  0.0) − (1.0 / 0.0));   //  NaN
```

When using ==, a NaN value is never equal to another value, even another NaN value:

```c#
Console.WriteLine (0.0 / 0.0 == double.NaN);    // False
```

To test whether a value is NaN, you must use the float.IsNaN or double.IsNaN method:
```c#
Console.WriteLine (double.IsNaN (0.0 / 0.0));   // True

```

`Note`: NaNs are sometimes useful in representing special values. In Windows Presentation Foundation (WPF), double.NaN represents a measurement whose value is “Automatic.” Another way to represent such a value is with a nullable type (Chapter 4); another is with a custom struct that wraps a numeric type and adds an additional field (Chapter 3).

# double Versus decimal
`double` is useful for scientific computations (such as computing spatial coordinates).
`decimal` is useful for financial computations and values that are “human-made” rather than the result of real-world measurements. Here’s a summary of the differences:

```
Category	    double	                                  decimal
Internal  representation	Base 2	          Base 10
Decimal   precision	15–16                   significant figures	28–29 significant figures
Range	    ±(~10−324 to ~10308)	            ±(~10−28 to ~1028)
Special values	+0, −0, +∞, −∞, and NaN	    None
Speed	Native to processor	                  Non-native to processor (about 10 times slower than double)
```

# Real Number Rounding Errors
float and double internally represent numbers in base 2. For this reason, only numbers expressible in base 2 are represented precisely. Practically, this means most literals with a fractional component (which are in base 10) will not be represented precisely; for example:

```c#
float x = 0.1f;  // Not quite 0.1
Console.WriteLine (x + x + x + x + x + x + x + x + x + x);    // 1.0000001
```

This is why `float` and `double` are bad for financial calculations. In contrast, `decimal` works in base 10 and so can precisely represent numbers expressible in base 10 (as well as its factors, base 2 and base 5). Because `real literals are in base 10`, decimal can precisely represent numbers such as 0.1. However, neither double nor decimal can precisely represent a fractional number whose base 10 representation is recurring:

decimal m = 1M / 6M;               // 0.1666666666666666666666666667M
double  d = 1.0 / 6.0;             // 0.16666666666666666

This leads to accumulated rounding errors:

decimal notQuiteWholeM = m+m+m+m+m+m;  // 1.0000000000000000000000000002M
double  notQuiteWholeD = d+d+d+d+d+d;  // 0.99999999999999989

which break equality and comparison operations:

Console.WriteLine (notQuiteWholeM == 1M);   // False
Console.WriteLine (notQuiteWholeD < 1.0);   // True

# bool Conversions

## Equality and Comparison Operators
== and != test for equality and inequality of any type but always return a bool value.3 Value types typically have a very simple notion of equality:

```
int x = 1;
int y = 2;
int z = 1;
Console.WriteLine (x == y);         // False
Console.WriteLine (x == z);         // True

```

For reference types, equality, by default, is based on reference, as opposed to the actual value of the underlying object 

```c#
Dude d1 = new Dude ("John");
Dude d2 = new Dude ("John");
Console.WriteLine (d1 == d2);       // False
Dude d3 = d1;
Console.WriteLine (d1 == d3);       // True

public class Dude
{
  public string Name;
  public Dude (string n) { Name = n; }
}
```

## Conditional Operators
The`&& and ||` operators short-circuit evaluation when possible. In the preceding example, if it is windy, the expression (rainy || sunny) is not even evaluated. Short-circuiting is essential in allowing expressions such as the following to run without throwing a NullReferenceException:

```c#
if (sb != null && sb.Length > 0) ...
```
The & and | operators also test for and and or conditions:
```c#
return !windy & (rainy | sunny);
```

## Conditional operator (ternary operator)
The conditional operator (more commonly called the ternary operator because it’s the only operator that takes three operands) has the form q ? a : b; thus, if condition q is true, a is evaluated, otherwise b is evaluated:
```
static int Max (int a, int b)
{
  return (a > b) ? a : b;
}
```