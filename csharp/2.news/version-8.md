# What’s New in C# 8.0
C# 8.0 first shipped with Visual Studio 2019, and is still used today when you target .NET Core 3 or .NET Standard 2.1.
# Indices and ranges
Indices and ranges simplify working with elements or portions of an array (or the low-level types Span<T> and ReadOnlySpan<T>).

Indices let you refer to elements relative to the end of an array by using the ^ operator. `^1 refers to the last element, ^2 refers to the second-to-last element`, and so on:
```c#
char[] vowels = new char[] {'a','e','i','o','u'};
char lastElement  = vowels [^1];   // 'u'
char secondToLast = vowels [^2];   // 'o'
```

Ranges let you “slice” an array by using the .. operator:
```c#
char[] firstTwo =  vowels [..2];    // 'a', 'e'
char[] lastThree = vowels [2..];    // 'i', 'o', 'u'
char[] middleOne = vowels [2..3]    // 'i'
char[] lastTwo =   vowels [^2..];   // 'o', 'u'
```

C# implements indexes and ranges with the help of the Index and Range types:
```c#
Index last = ^1;
Range firstTwoRange = 0..2;
char[] firstTwo = vowels [firstTwoRange];   // 'a', 'e'
```

You can support indices and ranges in your own classes by defining an indexer with a parameter type of Index or Range:
```c#
class Sentence
{
  string[] words = "The quick brown fox".Split();

  public string this   [Index index] => words [index];
  public string[] this [Range range] => words [range];
}
```

# Null-coalescing assignment
The ??= operator assigns a variable only if it’s null. Instead of
```c#
if (s == null) s = "Hello, world";
```
you can now write this:
```c#
s ??= "Hello, world";
```

# Using declarations
If you omit the brackets and statement block following a using statement, it becomes a using declaration. The resource is then disposed when execution falls outside the enclosing statement block:
```c#
if (File.Exists ("file.txt"))
{
  using var reader = File.OpenText ("file.txt");
  Console.WriteLine (reader.ReadLine());
  ...
  // reader is disposed here
}
```
In this case, `reader will be disposed when execution falls outside the if statement block`.
```c#
void ReadFile(string path)
{
  using FileStream fs = File.OpenRead(path); // using declaration
  byte[] b = new byte[1024];
  UTF8Encoding temp = new UTF8Encoding(true);
  while (fs.Read(b,0,b.Length) > 0)
  {
    Console.WriteLine(temp.GetString(b));
  }
  // fs is disposed here
}
```
The variable fs is disposed when the closing brace of the method is reached.

# Read-only members

C# 8 lets you apply the readonly modifier to a struct’s functions, ensuring that if the function attempts to modify any field, a compile-time error is generated:

```c#
struct Point
{
  public int X, Y;
  public readonly void ResetX() => X = 0;  // Error!
} 
```
If a readonly function calls a non-readonly function, the compiler generates a warning (and defensively copies the struct to avoid the possibility of a mutation).

# Static local methods
Adding the static modifier to a local method prevents it from seeing the local variables and parameters of the enclosing method. This helps to reduce coupling and enables the local method to declare variables as it pleases, without risk of colliding with those in the containing method.

```c#
void AreaofCircle(double a)
{
    double ar;
    Console.WriteLine("Radius of the circle: " + a);
 
    ar = 3.14 * a * a;
 
    Console.WriteLine("Area of circle: " + ar);
 
    // Calling static local function
    circumference(a);
 
    // Circumference is the Static local function
    // If circumference() try to access the enclosing
    // scope variable, then the compile will give error
    static void circumference(double radii)
    {
        ar = 1; // error
        double cr;
        cr = 2 * 3.14 * radii * a; // error
        Console.WriteLine("Circumference of the circle is: " + cr);
    }
}
```

# Default interface members
C# 8 lets you add a default implementation to an interface member, making it optional to implement. This means that you can add a member to an interface without breaking implementations. Default implementations must be called explicitly through the interface
```c#
interface ILogger
{
    void Log (string text) => Console.WriteLine (text);
    int LogInt() => 1;
}

class Logger : ILogger
{
    
}
((ILogger)new Logger()).Log ("message");

// OUTPUT
message
```

Interfaces can also define static members (including fields), which can be accessed from code inside default implementations:
```c#
interface ILogger
{
    static string Prefix = "";
    string Postfix = ""; // error: Instance fields are prohibited.
    void Log (string text) => Console.WriteLine (Prefix + text);
    int LogInt() => 1;
}
```
# Switch expressions
```c#
string cardName = cardNumber switch    // assuming cardNumber is an int
{
  13 => "King",
  12 => "Queen",
  11 => "Jack",
  _ => "Pip card"   // equivalent to 'default'
};

```

# Tuple, positional, and property patterns
Tuple patterns let you switch on multiple values:
```c#
int cardNumber = 12; string suite = "spades";
string cardName = (cardNumber, suite) switch
{
  (13, "spades") => "King of spades",
  (13, "clubs") => "King of clubs",
  ...
};
```
Positional patterns allow a similar syntax for objects that expose a deconstructor, and property patterns let you match on an object’s properties. The following example uses a property pattern to test whether obj is a string with a length of 4:
```c#
if (obj is string { Length:4 }) ...
```

# Nullable reference types
Nullable reference types introduce a level of safety that’s enforced purely by the compiler in the form of warnings or errors when it detects code that’s at risk of generating a NullReferenceException.

Nullable reference types can be enabled either at the project level (via the Nullable element in the .csproj project file) or in code (via the #nullable directive). After it’s enabled, the compiler makes non-nullability the default: if you want a reference type to accept nulls, you must apply the ? suffix to indicate a nullable reference type:

```c#
#nullable enable    // Enable nullable reference types from this point on

string s1 = null;   // Generates a compiler warning! (s1 is non-nullable)
string? s2 = null;  // OK: s2 is nullable reference type
```
Uninitialized fields also generate a warning (if the type is not marked as nullable), as does dereferencing a nullable reference type, if the compiler thinks a NullReferenceException might occur:
```c#
void Foo (string? s) => Console.Write (s.Length);  // Warning (.Length)
```

To remove the warning, you can use the null-forgiving operator (!):
```c#
void Foo (string? s) => Console.Write (s!.Length);
```

# Asynchronous streams
You could use yield return to write an iterator, or await to write an asynchronous function. But you couldn’t do both and write an iterator that awaits, yielding elements asynchronously. C# 8 fixes this through the introduction of asynchronous streams:

```c#
async IAsyncEnumerable<int> RangeAsync (
  int start, int count, int delay)
{
  for (int i = start; i < start + count; i++)
  {
    await Task.Delay (delay);
    yield return i;
  }
}
```
The await foreach statement consumes an asynchronous stream:
```c#
await foreach (var number in RangeAsync (0, 10, 100))
  Console.WriteLine (number);
```

# short circuiting
https://www.youtube.com/watch?v=rXdwX2X4-gw&ab_channel=NickChapsas

# The New “Interceptors”
https://www.youtube.com/watch?v=91xir2oUQPg&ab_channel=NickChapsas

# Why You Might Not Need Interfaces in C# 12
https://www.youtube.com/watch?v=ptJ5vGhvQmI&ab_channel=NickChapsas