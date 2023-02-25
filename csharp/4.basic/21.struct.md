# Structs
A struct is similar to a class, with the following key differences:

A struct is a value type, whereas a class is a reference type.

A struct does not support inheritance (other than implicitly deriving from object, or more precisely, System.ValueType).

A struct can have all of the members that a class can, except for a finalizer. And because it cannot be subclassed, members cannot be marked as virtual, abstract, or protected.

A struct can have all of the members that a class can, except for a finalizer. And because it cannot be subclassed, members cannot be marked as virtual, abstract, or protected.

Warning:
Prior to C# 10, structs were further prohibited from defining fields initializers and parameterless constructors. Although this prohibition has now been relaxed—primarily for the benefit of record structs (see “Records”)—it’s worth thinking carefully before defining these constructs, as they can result in confusing behavior that we’ll describe in “Struct Construction Semantics”.

A struct is appropriate when value-type semantics are desirable. Good examples of structs are numeric types, where it is more natural for assignment to copy a value rather than a reference. Because a struct is a value type, each instance does not require instantiation of an object on the heap; this results in useful savings when creating many instances of a type. For instance, creating an array of value type elements requires only a single heap allocation.

Because structs are value types, an instance cannot be null. The default value for a struct is an empty instance, with all fields empty (set to their default values).

# Struct Construction Semantics
Unlike with classes, every field in a struct must be explicitly assigned in the constructor (or field initializer). For example:

struct Point
{
  int x, y;
  public Point (int x, int y) { this.x = x; this.y = y; }  // OK
}
If we added the following constructor, the struct would not compile, because y would remain unassigned:

  public Point (int x)        { this.x = x; }  // Not OK

# The default constructor
In addition to any constructors that you define, a struct always has an implicit parameterless constructor that performs a bitwise-zeroing of its fields (setting them to their default values):

Point p = new Point();        // p.x and p.y will be 0
struct Point { int x, y; }

Even when you define a parameterless constructor of your own, the implicit parameterless constructor still exists and can be accessed via the default keyword:

Point p1 = new Point();       // p1.x and p1.y will be 1
Point p2 = default;           // p2.x and p2.y will be 0

struct Point
{
  int x = 1;
  int y;
  public Point() => y = 1;
}

In this example, we initialized x to 1 via a field initializer, and we initialized y to 1 via the parameterless constructor. And yet with the default keyword, we were still able to create a Point that bypassed both initializations. The default constructor can be accessed other ways, too, as the following example illustrates:

var points = new Point[10];   // Each point in the array will be (0,0)
var test = new Test();        // test.p will be (0,0)

class Test { Point p; }

A good strategy with structs is to design them such that their default value is a valid state, thereby making initialization redundant. For example, rather than this:

struct WebOptions { public string Protocol { get; set; } = "https"; }
consider the following:

struct WebOptions
{
  string protocol;
  public string Protocol { get => protocol ?? "https";
                           set => protocol = value;    }
}
# Read-Only Structs and Functions
You can apply the readonly modifier to a struct to enforce that all fields are readonly; this aids in declaring intent as well as affording the compiler more optimization freedom:

readonly struct Point
{
  public readonly int X, Y;   // X and Y must be readonly
}
If you need to apply readonly at a more granular level, you can apply the readonly modifier (from C# 8) to a struct’s functions. This ensures that if the function attempts to modify any field, a compile-time error is generated:

struct Point
{
  public int X, Y;
  public readonly void ResetX() => X = 0;  // Error!
} 
If a readonly function calls a non-readonly function, the compiler generates a warning (and defensively copies the struct to avoid the possibility of a mutation).

# Ref Structs
NOTE
Ref structs were introduced in C# 7.2 as a niche feature primarily for the benefit of the Span<T> and ReadOnlySpan<T> structs that we describe in Chapter 23 (and the highly optimized Utf8JsonReader that we describe in Chapter 11). These structs help with a micro-optimization technique that aims to reduce memory allocations.

Unlike reference types, whose instances always live on the heap, value types live in-place (wherever the variable was declared). If a value type appears as a parameter or local variable, it will reside on the stack:

void SomeMethod()
{
  Point p;   // p will reside on the stack
}
struct Point { public int X, Y; }
But if a value type appears as a field in a class, it will reside on the heap:

class MyClass
{
  Point p;   // Lives on heap, because MyClass instances live on the heap
}
Similarly, arrays of structs live on the heap, and boxing a struct sends it to the heap.

Adding the ref modifier to a struct’s declaration ensures that it can only ever reside on the stack. Attempting to use a ref struct in such a way that it could reside on the heap generates a compile-time error:

var points = new Point [100];           // Error: will not compile!

ref struct Point { public int X, Y; }
class MyClass    { Point P;         }   // Error: will not compile!

Ref structs were introduced mainly for the benefit of the Span<T> and ReadOnlySpan<T> structs. Because Span<T> and ReadOnlySpan<T> instances can exist only on the stack, it’s possible for them to safely wrap stack-allocated memory.

Ref structs cannot partake in any C# feature that directly or indirectly introduces the possibility of existing on the heap. This includes a number of advanced C# features that we describe in Chapter 4, namely lambda expressions, iterators, and asynchronous functions (because, behind the scenes, these features all create hidden classes with fields). Also, ref structs cannot appear inside non-ref structs, and they cannot implement interfaces (because this could result in boxing).
# When to use struct c#?

n C#, you can define your own custom value types using the struct keyword. Structs are similar to classes, but they are value types rather than reference types. Here are some guidelines on when to use structs:

For small objects that are used frequently and have a short lifetime, such as Point, Rectangle, or Color.
When you want to create a lightweight object that can be passed around by value, rather than by reference.
When you want to avoid heap allocations, which can be expensive in terms of memory and performance.
When you want to define a type that has a default constructor and can be initialized with an object initializer.
When you want to define a type that has value semantics rather than reference semantics, such as a complex number or a date-time value.

In C#, you can define your own custom value types using the struct keyword. Structs are similar to classes, but they are value types rather than reference types. Here are some guidelines on when to use structs:

For small objects that are used frequently and have a short lifetime, such as Point, Rectangle, or Color.
When you want to create a lightweight object that can be passed around by value, rather than by reference.
When you want to avoid heap allocations, which can be expensive in terms of memory and performance.
When you want to define a type that has a default constructor and can be initialized with an object initializer.
When you want to define a type that has value semantics rather than reference semantics, such as a complex number or a date-time value.
However, it's important to note that there are some limitations and trade-offs when using structs. For example, structs cannot be inherited, and they may not be suitable for large or complex objects due to memory and performance considerations. As with any design decision, it's important to weigh the pros and cons and choose the approach that best suits your specific requirements.