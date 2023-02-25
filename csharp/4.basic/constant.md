
# Constants
A constant is evaluated statically at compile time, and the compiler literally substitutes its value whenever used (rather like a macro in C++). A constant can be `bool, char, string, any of the built-in numeric types, or an enum type.`

A constant can serve a similar role to a static readonly field, but it is much more restrictive—both in the types you can use and in field initialization semantics. `A constant also differs from a static readonly field in that the evaluation of the constant occurs at compile time; thus`

```c#
public static double Circumference (double radius)
{
  return 2 * System.Math.PI * radius;
}
```
is compiled to:
```c#
public static double Circumference (double radius)
{
  return 6.2831853071795862 * radius;
}
```
It makes sense for PI to be a constant because its value is predetermined at compile time. In contrast, a static readonly field’s value can potentially differ each time the program is run:
```c#
static readonly DateTime StartupTime = DateTime.Now;
```
NOTE
A static readonly field is also advantageous when exposing to other assemblies a value that might change in a later version. For instance, suppose that assembly X exposes a constant as follows:
```c#
public const decimal ProgramVersion = 2.3;
```
If assembly Y references X and uses this constant, the value 2.3 will be baked into assembly Y when compiled. This means that if X is later recompiled with the constant set to 2.4, Y will still use the old value of 2.3 until Y is recompiled. A static readonly field avoids this problem.

Another way of looking at this is that any value that might change in the future is not constant by definition; thus, it should not be represented as one.

Constants can also be declared local to a method:
```c#
void Test()
{
  const double twoPI = 2 * System.Math.PI;
  ...
}
```
Nonlocal constants allow the following modifiers:

Access modifiers	public internal private protected
Inheritance modifier	ne