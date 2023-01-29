# What’s New in C# 9.0

C# 9.0 shipped with Visual Studio 2019, and is used when you target .NET 5.

# Top-level statements

You can write a program without the baggage of a Main method and Program class:

```c#
using System;
Console.WriteLine ("Hello, world");
```

Top-level statements can include methods (which act as local methods). You can also access command-line arguments via the `magic` args variable and return a value to the caller. Top-level statements can be followed by type and namespace declarations.

# Init-only setters

An init-only setter in a property declaration uses the init keyword instead of the set keyword:

```c#
class Foo { public int ID { get; init; } }
```

This behaves like a read-only property, except that it can also be set via an object initializer:

```c#
var foo = new Foo { ID = 123 };
```

This makes it possible to create immutable (read-only) types that can be populated via an object initializer instead of a constructor, and helps to avoid the antipattern of constructors that accept a large number of optional parameters. Init-only setters also allow for nondestructive mutation when used in records.

# Records

A record is a special kind of class that’s designed to work well with immutable data. Its most special feature is that it supports nondestructive mutation via a new keyword `(with)`:

```c#
Point p1 = new Point (2, 3);
Point p2 = p1 with { Y = 4 };   // p2 is a copy of p1, but with Y set to 4
Console.WriteLine (p2);         // Point { X = 2, Y = 4 }

record Point
{
  public Point (double x, double y) => (X, Y) = (x, y);

  public double X { get; init; }
  public double Y { get; init; }
}
```

We can replace our Point record definition with the following, without loss of functionality:

```c#
record Point (double X, double Y);
```

# Pattern-matching improvements

```c#
string GetWeightCategory (decimal bmi) => bmi switch {
  < 18.5m => "underweight",
  < 25m => "normal",
  < 30m => "overweight",
  _ => "obese" };
```

With pattern combinators, you can combine patterns via three new keywords `(and, or, and not)`. As with the && and || operators, and has higher precedence than or. You can override this with parentheses.

```c#
bool IsVowel (char c) => c is 'a' or 'e' or 'i' or 'o' or 'u';

bool IsLetter (char c) => c is >= 'a' and <= 'z'
                            or >= 'A' and <= 'Z';
```

The `not` combinator can be used with the type pattern to test whether an object is (not) a type:

```c#
if (obj is not string) ...
```

# Target-typed new expressions

When constructing an object, C# 9 lets you omit the type name when the compiler can infer it unambiguously:

```c#
System.Text.StringBuilder sb1 = new();
System.Text.StringBuilder sb2 = new ("Test");

MyMethod (new ("test"));
void MyMethod (System.Text.StringBuilder sb) { ... }
```

# Interop improvements

C# 9 introduced function pointers. Their main purpose is to allow unmanaged code to call static methods in C# without the overhead of a delegate instance, with the ability to bypass the P/Invoke layer when the arguments and return types are blittable (represented identically on each side).

C# 9 also introduced the nint and nuint native-sized integer types, which map at runtime to System.IntPtr and System.UIntPtr. At compile time, they behave like numeric types with support for arithmetic operations.

# Covariant return types

Override a method or read-only property such that it returns a more derived type

```c#
public class Asset
{
  public string Name;
  public virtual Asset Clone() => new Asset { Name = Name };
}

public class House : Asset
{
  public decimal Mortgage;
  public override House Clone() => new House
                                   { Name = Name, Mortgage = Mortgage };
}
```

Prior to C# 9, you had to override methods with the identical return type:

```c#
public override Asset Clone() => new House { ... }
```

# Apply attributes to local functions

C# 9 do support attributes on local function

```c#
static void Main(string[] args)
{
    [Conditional("DEBUG")]
    static void DoAction()
    {
      // Perform action
      Console.WriteLine("Performing action");
    }

    DoAction();
}
```

# Static lambdas
Apply the `static` keyword to lambda expressions or local functions to ensure that you don’t accidentally capture local or instance variables
```c#
Func<int, int> multiplier = static n => n * 2;
```
# Extended partial methods
Write extended partial methods that are mandatory to implement—enabling scenarios such as Roslyn’s new source generators 

```c#
public partial class Test
{
  public partial void M1();    // Extended partial method
  private partial void M2();   // Extended partial method
}
```

# SkipLocalsInit
Use the [SkipLocalsInit] attribute if you need to get the maximum performance possible. It's a quick win, but the gains are also very small for most use cases.

By default, the C# compiler emits the .locals init directive. This instructs the JIT to generate a prolog to set all local variables to their default value. This is safer as you cannot use uninitialized memory.
Apply an attribute to methods, types, or modules to prevent local variables from being initialized by the runtime
```c#
static unsafe void DemoZeroing()
{
    int i;
    Console.WriteLine(*&i);
    // Display 0 as the local variable is automatically initialized with the default value
}
```
Here's how to use it on a method to suppress emitting .locals init flag
```c#
[System.Runtime.CompilerServices.SkipLocalsInit]
static unsafe void DemoZeroing()
{
    int i;
    Console.WriteLine(*&i); // Unpredictable output as i is not initialized
}
```
When decompiling the application, you can see that the method now uses locals instead of `.locals init` which means that the variables won't be automatically initialized by the JIT.

Ref: https://www.meziantou.net/csharp-9-improve-performance-using-skiplocalsinit.htm

# Other features
Make any type work with the foreach statement, by writing a GetEnumerator extension method

Define a module initializer method that executes once when an assembly is first loaded, by applying the [ModuleInitializer] attribute to a (static void parameterless) method

Use a “discard” (underscore symbol) as a lambda expression argument