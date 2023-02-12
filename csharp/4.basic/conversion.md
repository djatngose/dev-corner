# what is precision ?
In C#, precision refers to the number of significant digits that can be represented by a numeric data type. It is a measure of the degree of detail that a number can represent. For example, if a floating-point number has a precision of 6, it means that it can accurately represent up to 6 significant digits.

The precision of a data type is determined by the number of bits used to store the value. Generally, the more bits a data type has, the higher its precision. However, `higher precision can come at the cost of increased memory usage and slower performance`. Therefore, it is important to choose a data type with an appropriate level of precision for the intended use case.

Let's say you have a decimal number with the value of `3.14159265358979323846`. If you store this number in a float variable, which has a precision of about 7 digits, the variable will only be able to accurately represent the first 7 digits of the number `(3.141593)`. The remaining digits will be rounded off or lost due to the limited precision of the float data type.

On the other hand, if you store the same number in a decimal variable, which has a higher precision of 28-29 digits, the variable will be able to accurately represent the entire number without any loss of precision.

So, the choice of the data type with an appropriate level of precision is important to ensure the accuracy of the calculations and to avoid rounding errors.
# Types and Conversions
C# can convert between instances of compatible types. A conversion always creates a new value from an existing one. Conversions can be either implicit or explicit: implicit conversions happen automatically, and explicit conversions require a cast. In the following example, we implicitly convert an int to a long type (which has twice the bit capacity of an int) and explicitly cast an int to a short type (which has half the bit capacity of an int):

```c#
int x = 12345;       // int is a 32-bit integer
long y = x;          // Implicit conversion to 64-bit integer
short z = (short)x;  // Explicit conversion to 16-bit integer
```

# Implicit conversions are allowed when both of the following are true:

The compiler can guarantee that they will always succeed.

Note: `No information is lost in conversion`.

# Explicit conversions are required when one of the following is true:

The compiler cannot guarantee that they will always succeed.

Note: `Information might be lost during conversion`.

# Numeric Conversions
Integral type conversions are implicit when the destination type can represent every possible value of the source type. Otherwise, an explicit conversion is required; for example:

```c#
int x = 12345;       // int is a 32-bit integer
long y = x;          // Implicit conversion to 64-bit integral type
short z = (short)x;  // Explicit conversion to 16-bit integral type
```
## Converting between floating-point types

A float can be implicitly converted to a double given that a double can represent every possible value of a float. The reverse conversion must be explicit.

## Converting between floating-point and integral types
All integral types can be implicitly converted to all floating-point types:
```c#
int i = 1;
float f = i;
```
The reverse conversion must be explicit:
```c#
int i2 = (int)f;
```

Note: `When you cast from a floating-point number to an integral type, any fractional portion is truncated; no rounding is performed. The static class System.Convert provides methods that round while converting between various numeric types`

Implicitly converting a large integral type to a floating-point type preserves magnitude but can occasionally lose precision. This is because floating-point types always have more magnitude than integral types but can have less precision. Rewriting our example with a larger number demonstrates this:

```c#
int i1 = 100000001;
float f = i1;          // Magnitude preserved, precision lost
int i2 = (int)f;       // 100000000
```