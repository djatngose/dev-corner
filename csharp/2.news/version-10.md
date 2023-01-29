# What’s New in C# 10
C# 10 ships with Visual Studio 2022, and is used when you target .NET 6.

# File-scoped namespaces
- In the common case that all types in a file are defined in a single namespace, a file-scoped namespace declaration in C# 10 reduces clutter and eliminates an unnecessary level of indentation:
```c#
namespace MyNamespace;  // Applies to everything that follows in the file.

class Class1 {}         // inside MyNamespace
class Class2 {}         // inside MyNamespace
```

# The global using directive
- When you prefix a using directive with the global keyword, it applies the directive to all files in the project:
```c#
global using System;
global using System.Collection.Generic;
```
- This lets you avoid repeating the same directives in every file. global using directives work with using static.
- Additionally, .NET 6 projects now support implicit global using directives: if the ImplicitUsings element is set to true in the project file, the most commonly used namespaces are automatically imported (based on the SDK project type)

# Nondestructive mutation for anonymous types
- C# 9 introduced the with keyword, to perform nondestructive mutation on records. In C# 10, the with keyword also works with anonymous types:
```c#
var a1 = new { A = 1, B = 2, C = 3, D = 4, E = 5 };
var a2 = a1 with { E = 10 }; 
Console.WriteLine (a2);      // { A = 1, B = 2, C = 3, D = 4, E = 10 }
```

# New deconstruction syntax
- C# 7 introduced the deconstruction syntax for tuples (or any type with a Deconstruct method). C# 10 takes this syntax further, letting you mix assignment and declaration in the same deconstruction
- this feature has the sames as typescript I guess
```c#
var point = (3, 4);
double x = 0;
(x, double y) = point;
```

# Field initializers and parameterless constructors in structs
- From C# 10, you can include field initializers and parameterless constructors in structs (see “Structs”). These execute only when the constructor is called explicitly, and so can easily be bypassed—for instance, via the default keyword. This feature was introduced primarily for the benefit of struct records.


```c#
Point p1 = new Point();       // p1.x and p1.y will be 1
Point p2 = default;           // p2.x and p2.y will be 0

struct Point
{
  int x = 1;
  int y;
  public Point() => y = 1;
}
```

# Record structs
- Records were first introduced in C# 9, where they acted as a compiled-enhanced class. In C# 10, records can also be structs:
```c#
record struct Point (int X, int Y); 
```
- Record structs have much the same features as class structs (see “Records”). An `exception` is that the compiler-generated properties on record structs are writable, unless you prefix the record declaration with the readonly keyword.
```c#
public record struct C(string name);
```
the code behind shows:
```c#
public struct C : IEquatable<C>
{
    [CompilerGenerated]
    private string <name>k__BackingField;

    public string name
    {
        [IsReadOnly]
        [CompilerGenerated]
        get
        {
            return <name>k__BackingField;
        }
        [CompilerGenerated]
        set
        {
            <name>k__BackingField = value;
        }
    }

    public C(string name)
    {
        <name>k__BackingField = name;
    }

    [IsReadOnly]
    [CompilerGenerated]
    public override string ToString()
    {
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.Append("C");
        stringBuilder.Append(" { ");
        if (PrintMembers(stringBuilder))
        {
            stringBuilder.Append(' ');
        }
        stringBuilder.Append('}');
        return stringBuilder.ToString();
    }

    [IsReadOnly]
    [CompilerGenerated]
    private bool PrintMembers(StringBuilder builder)
    {
        builder.Append("name = ");
        builder.Append((object)name);
        return true;
    }

    [CompilerGenerated]
    public static bool operator !=(C left, C right)
    {
        return !(left == right);
    }

    [CompilerGenerated]
    public static bool operator ==(C left, C right)
    {
        return left.Equals(right);
    }

    [IsReadOnly]
    [CompilerGenerated]
    public override int GetHashCode()
    {
        return EqualityComparer<string>.Default.GetHashCode(<name>k__BackingField);
    }

    [IsReadOnly]
    [CompilerGenerated]
    public override bool Equals(object obj)
    {
        if (obj is C)
        {
            return Equals((C)obj);
        }
        return false;
    }

    [IsReadOnly]
    [CompilerGenerated]
    public bool Equals(C other)
    {
        return EqualityComparer<string>.Default.Equals(<name>k__BackingField, other.<name>k__BackingField);
    }

    [IsReadOnly]
    [CompilerGenerated]
    public void Deconstruct(out string name)
    {
        name = this.name;
    }
}
```

# Lambda expression enhancements
```c#
var greeter = () => "Hello, world";
var square = (int x) => x * x;
// a lambda expression can specify a return type
var sqr = int (int x) => x;
```
- You can pass a lambda expression into a method parameter of type object, Delegate, or Expression:
```c#
M1 (() => "test");   // Implicitly typed to Func<string>
M2 (() => "test");   // Implicitly typed to Func<string>
M3 (() => "test");   // Implicitly typed to Expression<Func<string>>

void M1 (object x) {}
void M2 (Delegate x) {}
void M3 (Expression x) {}
```
- You can apply attributes to a lambda expression’s compile-generated target method (as well as its parameters and return value):
```c#
Action<int> a = [Description ("Method")]
                [return: Description ("Return value")]
                ([Description ("Parameter")]int x) => Console.Write (x);
```

# Nested property patterns
```c#
var obj = new Uri ("https://www.linqpad.net");
if (obj is Uri { Scheme.Length: 5 }) ...
```
- This is equivalent to:
```c#
if (obj is Uri { Scheme: { Length: 5 }}) ...
```

# CallerArgumentExpression
- A method parameter to which you apply the [CallerArgumentExpression] attribute captures an argument expression from the call site:
```c#
Print (Math.PI * 2);

void Print (double number,
           [CallerArgumentExpression("number")] string expr = null)
  => Console.WriteLine (expr);

// Output: Math.PI * 2
```

# Other features
Record types can seal ToString