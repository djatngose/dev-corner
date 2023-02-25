# Class
A class is the most common kind of reference type.
Attributes and class modifiers. The non-nested class modifiers are `public, internal, abstract, sealed, static, unsafe, and partial.`

# Fields
A field is a variable that is a member of a class or struct; for example:
```c#
class Octopus
{
  string name;
  public int Age = 10;
}
```
Fields allow the following modifiers:

- Static modifier	`static`
- Access modifiers	`public internal private protected`
- Inheritance modifier	`new`
- Unsafe code modifier	`unsafe`
- Read-only modifier	`readonly`
- Threading modifier	`volatile`

# Methods
A method performs an action in a series of statements. A method can receive input data from the caller by specifying parameters, and output data back to the caller by specifying a return type. A method can specify a void return type, indicating that it doesn’t return any value to its caller. A method can also output data back to the caller via ref/out parameters.

A method’s signature must be unique within the type. A method’s signature comprises its name and parameter types in order (but not the parameter names, nor the return type).

Methods allow the following modifiers:

Static modifier	`static`
Access modifiers	`public internal private protected`
Inheritance modifiers	`new virtual abstract override sealed`
Partial method modifier	`partial`
Unmanaged code modifiers	`unsafe extern`
Asynchronous code modifier	`async`

# Expression-bodied methods
A method that comprises a single expression, such as
```c#
int Foo (int x) { return x * 2; }
```
can be written more tersely as an expression-bodied method. A fat arrow replaces the braces and return keyword:
```c#
int Foo (int x) => x * 2;
```
Expression-bodied functions can also have a void return type:

void Foo (int x) => Console.WriteLine (x);

# Local methods
You can define a method within another method:
```c#
void WriteCubes()
{
  Console.WriteLine (Cube (3));
  Console.WriteLine (Cube (4));
  Console.WriteLine (Cube (5));

  int Cube (int value) => value * value * value;
}
```
The local method (Cube, in this case) is visible only to the enclosing method (WriteCubes). This simplifies the containing type and instantly signals to anyone looking at the code that Cube is used nowhere else. Another benefit of local methods is that they can access the local variables and parameters of the enclosing method.

# Static local methods

Static local methods
Adding the static modifier to a local method (from C# 8) prevents it from seeing the local variables and parameters of the enclosing method. This helps to reduce coupling and prevents the local method from accidentally referring to variables in the containing method.

```c#
void WriteCubes()
{
    var a = 1;
    Console.WriteLine (Cube (3));
    Console.WriteLine (Cube (4));
    Console.WriteLine (Cube (5));

   static int Cube (int value) => a* value * value * value; // error compiled
}
```
# Local methods and top-level statements
Any methods that you declare in top-level statements are treated as local methods. This means that (unless marked as static) they can access the variables in the top-level statements:
```c#
int x = 3;
Foo();

void Foo() => Console.WriteLine (x);
```
- `WARNING`: Local methods cannot be overloaded. This means that methods declared in top-level statements (which are treated as local methods) cannot be overloaded.

A` type can overload methods (define multiple methods with the same name) as long as the signatures are different`. For example, the following methods can all coexist in the same type:

```c#
void Foo (int x) {...}
void Foo (double x) {...}
void Foo (int x, float y) {...}
void Foo (float x, int y) {...}
```

However, the following pairs of methods cannot coexist in the same type, because the return type and the params modifier are not part of a method’s signature:

```c#
void  Foo (int x) {...}
float Foo (int x) {...}           // Compile-time error

void  Goo (int[] x) {...}
void  Goo (params int[] x) {...}  // Compile-time error
```
Whether a parameter is pass-by-value or pass-by-reference is also part of the signature. For example, `Foo(int) can coexist with either Foo(ref int) or Foo(out int). However, Foo(ref int) and Foo(out int) cannot coexist`:
```c#
void Foo (int x) {...}
void Foo (ref int x) {...}     // OK so far
void Foo (out int x) {...}     // Compile-time error
```
# Instance Constructors
Constructors run initialization code on a class or struct. A constructor is defined like a method, except that the method name and return type are reduced to the name of the enclosing type:
```c#
Panda p = new Panda ("Petey");   // Call constructor

public class Panda
{
  string name;                   // Define field
  public Panda (string n)        // Define constructor
  {
    name = n;                    // Initialization code (set up field)
  }
}

```
Instance constructors allow the following modifiers:s
  - Access modifiers	`public internal private protected`
  - Unmanaged code modifiers	`unsafe extern`

Single-statement constructors can also be written as expression-bodied members:
```c#
public Panda (string n) => name = n;
```

## Overloading constructors
A class or struct may overload constructors. `To avoid code duplication, one constructor can call another`, using the this keyword:
```c#
using System;

public class Wine
{
  public decimal Price;
  public int Year;
  public Wine (decimal price) { Price = price; }
  public Wine (decimal price, int year) : this (price) { Year = year; }
}
```
When one constructor calls another, the called constructor executes first.

You can pass an expression into another constructor, as follows:
```c#
public Wine (decimal price, DateTime year) : this (price, year.Year) { }
```

# Implicit parameterless constructors
For classes, `the C# compiler automatically generates a parameterless public constructor if and only if you do not define any constructors`. However, as soon as you define at least one constructor, the parameterless constructor is no longer automatically generated.

# Nonpublic constructors
Constructors do not need to be public. A common reason to have a nonpublic constructor is to control instance creation via a static method call. The static method could be used to return an object from a pool rather than creating a new object, or to return various subclasses based on input arguments:
```c#
public class Class1
{
  Class1() {}                             // Private constructor
  public static Class1 Create (...)
  {
    // Perform custom logic here to return an instance of Class1
    ...
  }
}
```
# Deconstructors
A deconstructor (also called a deconstructing method) acts as an approximate opposite to a constructor: whereas a constructor typically takes a set of values (as parameters) and assigns them to fields, a deconstructor does the reverse and assigns fields back to a set of variables.
```c#
class Rectangle
{
  public readonly float Width, Height;
  
  public Rectangle (float width, float height)
  {
    Width = width;
    Height = height;
  }
  
  public void Deconstruct (out float width, out float height)
  {
    width = Width;
    height = Height;
  }
}

var rect = new Rectangle (3, 4);
Console.WriteLine ("init:"+rect.Width + " " + rect.Height);  // 3 4
(float width, float height) = rect;          // Deconstruction
Console.WriteLine (width + " " + height);    // 3 4
Console.WriteLine ("after init:"+rect.Width + " " + rect.Height);  // 3 4
```

This is called a deconstructing assignment. You can use a deconstructing assignment to simplify your class’s constructor:
```c#
public Rectangle (float width, float height) =>
  (Width, Height) = (width, height);

```
From C# 10, you can mix and match existing and new variables when deconstructing:
```c#
double x1 = 0;
(x1, double y2) = rect;
```

# Object Initializers
Using object initializers, you can instantiate Bunny objects as follows:
```c#
// Note parameterless constructors can omit empty parentheses
Bunny b1 = new Bunny { Name="Bo", LikesCarrots=true, LikesHumans=false };
Bunny b2 = new Bunny ("Bo")     { LikesCarrots=true, LikesHumans=false };
```
Instead of using object initializers, we could make Bunny’s constructor accept optional parameters:
```c#
public Bunny (string name,
              bool likesCarrots = false,
              bool likesHumans = false)
{
  Name = name;
  LikesCarrots = likesCarrots;
  LikesHumans = likesHumans; 
}
```
Optional parameters have two `drawbacks`. The `first` is that while their use in constructors allows for read-only types, they don’t (easily) allow for nondestructive mutation.
The `second` drawback of optional parameters is that when used in public libraries, they hinder backward compatibility. This is because the act of adding an optional parameter at a later date breaks the assembly’s binary compatibility with existing consumers. (This is particularly important when a library is published on NuGet: the problem becomes intractable when a consumer references packages A and B, if A and B each depend on incompatible versions of L.)

This is problematic if we instantiate the Bunny class from another assembly and later modify Bunny by adding another optional parameter—such as likesCats. Unless the referencing assembly is also recompiled, it will continue to call the (now nonexistent) constructor with three parameters and fail at runtime. (A subtler problem is that if we changed the value of one of the optional parameters, callers in other assemblies would continue to use the old optional value until they were recompiled.)