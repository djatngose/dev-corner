# Defining a Main method
Without top-level statements, a simple console or Windows application looks like this:
```c#
using System;

class Program
{
  static void Main()   // Program entry point
  {
    int x = 12 * 30;
    Console.WriteLine (x);
  }
}  
```
The Main method can optionally return an integer (rather than void) in order to return a value to the execution environment (where a nonzero value typically indicates an error). The Main method can also optionally accept an array of strings as a parameter (that will be populated with any arguments passed to the executable). For example:
```c#
static int Main (string[] args) {...}
```
Note: An array (such as string[]) represents a fixed number of elements of a particular type. Arrays are specified by placing square brackets after the element type. 

The Main method can also be declared `async and return a Task or Task<int>` in support of asynchronous programming, 

# TOP-LEVEL STATEMENTS
Top-level statements (introduced in C# 9) let you avoid the baggage of a static Main method and a containing class. A file with top-level statements comprises three parts, in this order:
```c#
using System;                           // Part 1

Console.WriteLine ("Hello, world");     // Part 2
void SomeMethod1() { ... }              // Part 2
Console.WriteLine ("Hello again!");     // Part 2
void SomeMethod2() { ... }              // Part 2

class SomeClass { ... }                 // Part 3
namespace SomeNamespace { ... }         // Part 3
```
Because the CLR doesn’t explicitly support top-level statements, the compiler translates your code into something like this:
```c#
using System;                           // Part 1

static class Program$   // Special compiler-generated name
{
   static void Main$ (string[] args)   // Compiler-generated name
   {
    Console.WriteLine ("Hello, world");     // Part 2
    void SomeMethod1() { ... }              // Part 2
    Console.WriteLine ("Hello again!");     // Part 2
    void SomeMethod2() { ... }              // Part 2
  }
 }

class SomeClass { ... }                 // Part 3
namespace SomeNamespace { ... }         // Part 3
```

This means that SomeMethod1 and SomeMethod2 act as local methods. the most important being that local methods (unless declared as static) can access variables declared within the containing method:


```c#
int x = 3;
LocalMethod();

void LocalMethod() { Console.WriteLine (x); }   // We can access x

```

Another consequence is that top-level methods cannot be accessed from other classes or types.

Top-level statements can optionally return an integer value to the caller and access a “magic” variable of type string[] called args, corresponding to command-line arguments passed by the caller.

As a program can have only one entry point, there can be at most one file with top-level statements in a C# project.