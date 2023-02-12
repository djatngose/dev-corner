# What’s New in C# 7.x
C# 7.x was first shipped with Visual Studio 2017. C# 7.3 is still used today by Visual Studio 2019 when you target .NET Core 2, .NET Framework 4.6 to 4.8, or .NET Standard 2.0.

# Numeric literal improvements
Numeric literals in C# 7 can include underscores to improve readability. These are called digit separators and are ignored by the compiler:
```c#
int million = 1_000_000;
```
Binary literals can be specified with the 0b prefix:
```c#
var b = 0b1010_1011_1100_1101_1110_1111;
```

# Out variables and discards
C# 7 makes it easier to call methods that contain out parameters. First, you can now declare out variables on the fly (see “Out variables and discards”):

bool successful = int.TryParse ("123", out int result);
Console.WriteLine (result);
And when calling a method with multiple out parameters, you can discard ones you’re uninterested in with the underscore character:

SomeBigMethod (out _, out _, out _, out int x, out _, out _, out _);
Console.WriteLine (x);

# Type patterns and pattern variables
You can also introduce variables on the fly with the is operator. These are called pattern variables (see “Introducing a pattern variable”):

void Foo (object x)
{
  if (x is string s)
    Console.WriteLine (s.Length);
}
The switch statement also supports type patterns, so you can switch on type as well as constants (see “Switching on types”). You can specify conditions with a when clause and also switch on the null value:

switch (x)
{
  case int i:
    Console.WriteLine ("It's an int!");
    break;
  case string s:
    Console.WriteLine (s.Length);    // We can use the s variable
    break;
  case bool b when b == true:        // Matches only when b is true
    Console.WriteLine ("True");
    break;
  case null:
    Console.WriteLine ("Nothing");
    break;
}

# Local methods
A local method is a method declared within another function (see “Local methods”):

void WriteCubes()
{
  Console.WriteLine (Cube (3));
  Console.WriteLine (Cube (4));
  Console.WriteLine (Cube (5));

  int Cube (int value) => value * value * value;
}
Local methods are visible only to the containing function and can capture local variables in the same way that lambda expressions do.

# More expression-bodied members
C# 6 introduced the expression-bodied “fat-arrow” syntax for methods, read-only properties, operators, and indexers. C# 7 extends this to constructors, read/write properties, and finalizers:

public class Person
{
  string name;

  public Person (string name) => Name = name;

  public string Name
  {
    get => name;
    set => name = value ?? "";
  }

  ~Person () => Console.WriteLine ("finalize");
}

# Deconstructors
C# 7 introduces the deconstructor pattern (see “Deconstructors”). Whereas a constructor typically takes a set of values (as parameters) and assigns them to fields, a deconstructor does the reverse and assigns fields back to a set of variables. We could write a deconstructor for the Person class in the preceding example as follows (exception handling aside):

public void Deconstruct (out string firstName, out string lastName)
{
  int spacePos = name.IndexOf (' ');
  firstName = name.Substring (0, spacePos);
  lastName = name.Substring (spacePos + 1);
}
Deconstructors are called with the following special syntax:

var joe = new Person ("Joe Bloggs");
var (first, last) = joe;          // Deconstruction
Console.WriteLine (first);        // Joe
Console.WriteLine (last);         // Bloggs

# Tuples
Perhaps the most notable improvement to C# 7 is explicit tuple support (see “Tuples”). Tuples provide a simple way to store a set of related values:

var bob = ("Bob", 23);
Console.WriteLine (bob.Item1);   // Bob
Console.WriteLine (bob.Item2);   // 23
C#’s new tuples are syntactic sugar for using the System.ValueTuple<…> generic structs. But thanks to compiler magic, tuple elements can be named:

var tuple = (name:"Bob", age:23);
Console.WriteLine (tuple.name);     // Bob
Console.WriteLine (tuple.age);      // 23
With tuples, functions can return multiple values without resorting to out parameters or extra type baggage:

static (int row, int column) GetFilePosition() => (3, 10);

static void Main()
{
  var pos = GetFilePosition();
  Console.WriteLine (pos.row);      // 3
  Console.WriteLine (pos.column);   // 10
}
Tuples implicitly support the deconstruction pattern, so you can easily deconstruct them into individual variables:

static void Main()
{
  (int row, int column) = GetFilePosition();   // Creates 2 local variables
  Console.WriteLine (row);      // 3 
  Console.WriteLine (column);   // 10
}

# throw expressions
Prior to C# 7, throw was always a statement. Now it can also appear as an expression in expression-bodied functions:

public string Foo() => throw new NotImplementedException();
A throw expression can also appear in a ternary conditional expression:

string Capitalize (string value) =>
  value == null ? throw new ArgumentException ("value") :
  value == "" ? "" :
  char.ToUpper (value[0]) + value.Substring (1);