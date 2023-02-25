# Statements
Functions comprise statements that execute sequentially in the textual order in which they appear. A statement block is a series of statements appearing between braces (the {} tokens).

# Declaration Statements
A variable declaration introduces a new variable, optionally initializing it with an expression. You may declare multiple variables of the same type in a comma-separated list:

string someWord = "rosebud";
int someNumber = 42;
bool rich = true, famous = false;
A constant declaration is like a variable declaration except that it cannot be changed after it has been declared, and the initialization must occur with the declaration

const double c = 2.99792458E08;
c += 10;                        // Compile-time Error

# Local variables
The scope of a local variable or local constant extends throughout the current block. You cannot declare another local variable with the same name in the current block or in any nested blocks:

int x;
{
  int y;
  int x;            // Error - x already defined
}
{
  int y;            // OK - y not in scope
}
Console.Write (y);  // Error - y is out of scope

Note: A variable’s scope extends in both directions throughout its code block. This means that if we moved the initial declaration of x in this example to the bottom of the method, we’d get the same error. This is in contrast to C++ and is somewhat peculiar, given that it’s not legal to refer to a variable or constant before it’s declared.

# Expression Statements
Expression statements are expressions that are also valid statements. An expression statement must either change state or call something that might change state. Changing state essentially means changing a variable. Following are the possible expression statements:

  - Assignment expressions (including increment and decrement expressions)
  - Method call expressions (both void and nonvoid)
  - Object instantiation expressions

```c#
/ Declare variables with declaration statements:
string s;
int x, y;
System.Text.StringBuilder sb;

// Expression statements
x = 1 + 2;                 // Assignment expression
x++;                       // Increment expression
y = Math.Max (x, 5);       // Assignment expression
Console.WriteLine (y);     // Method call expression
sb = new StringBuilder();  // Assignment expression
new StringBuilder();       // Object instantiation expression

```

# Selection Statements
C# has the following mechanisms to conditionally control the flow of program execution:

  - Selection statements (if, switch)
  - Conditional operator (?:)
  - Loop statements (while, do-while, for, foreach)

This section covers the simplest two constructs: the if statement and the switch statement.

# Switch case
```c#
switch (x)
{
  case bool b when b == true:     // Fires only when b is true
    Console.WriteLine ("True!");
    break;
  case bool b:
    Console.WriteLine ("False!");
    break;
}
```
If you want to switch on a type, but are uninterested in its value, you can use a discard (_):
```c#
 case DateTime _:
      Console.WriteLine ("It's a DateTime");
```
You can stack multiple case clauses. The Console.WriteLine in the following code will execute for any floating-point type greater than 1,000:
```c#
switch (x)
{
  case float f when f > 1000:
  case double d when d > 1000:
  case decimal m when m > 1000:
    Console.WriteLine ("We can refer to x here but not f or d or m");
    break;
}
```
In this example, the compiler lets us consume the pattern variables f, d, and m, only in the when clauses. When we call Console.WriteLine, its unknown which one of those three variables will be assigned, so the compiler puts all of them out of scope.

You can mix and match constants and patterns in the same switch statement. And you can also switch on the null value:

```c#
case null:
  Console.WriteLine ("Nothing here");
  break;
```
# Switch expressions
You can use switch in the context of an expression. Assuming that cardNumber is of type int, the following illustrates its use:
```c#
string cardName = cardNumber switch
{
  13 => "King",
  12 => "Queen",
  11 => "Jack",
  _ => "Pip card"   // equivalent to 'default'
};
```
If you omit the default expression (_) and the switch fails to match, an exception is thrown.

You can also switch on multiple values (the tuple pattern):
```c#
int cardNumber = 12;
string suite = "spades";

string cardName = (cardNumber, suite) switch
{
  (13, "spades") => "King of spades",
  (13, "clubs") => "King of clubs",
  ...
};
```
# for loops
For example, the following prints the numbers 0 through 2:
```c#
for (int i = 0; i < 3; i++)
  Console.WriteLine (i);
```
The following prints the first 10 Fibonacci numbers (in which each number is the sum of the previous two):
```c#
for (int i = 0, prevFib = 1, curFib = 1; i < 10; i++)
{
  Console.WriteLine (prevFib);
  int newFib = prevFib + curFib;
  prevFib = curFib; curFib = newFib;
}
```
Any of the three parts of the for statement can be omitted. You can implement an infinite loop such as the following (though while(true) can be used, instead):

```c#
for (;;)
  Console.WriteLine ("interrupt me");
```

# The break statement
The break statement ends the execution of the body of an iteration or switch statement:

int x = 0;
while (true)
{
  if (x++ > 5)
    break;      // break from the loop
}
// execution continues here after break
...
# The continue statement
The continue statement forgoes the remaining statements in a loop and makes an early start on the next iteration. The following loop skips even numbers:

for (int i = 0; i < 10; i++)
{
  if ((i % 2) == 0)       // If i is even,
    continue;             // continue with next iteration

  Console.Write (i + " ");
}

OUTPUT: 1 3 5 7 9
# The goto statement
The goto statement transfers execution to another label within a statement block. The form is as follows:

goto statement-label;
Or, when used within a switch statement:

goto case case-constant;    // (Only works with constants, not patterns)
A label is a placeholder in a code block that precedes a statement, denoted with a colon suffix. The following iterates the numbers 1 through 5, mimicking a for loop:

int i = 1;
startLoop:
if (i <= 5)
{
  Console.Write (i + " ");
  i++;
  goto startLoop;
}

OUTPUT: 1 2 3 4 5

# The return statement
The return statement exits the method and must return an expression of the method’s return type if the method is nonvoid:

decimal AsPercentage (decimal d)
{
  decimal p = d * 100m;
  return p;             // Return to the calling method with value
}
A return statement can appear anywhere in a method (except in a finally block) and can be used more than once.

# The throw statement
The throw statement throws an exception to indicate an error has occurred (see “try Statements and Exceptions”):

if (w == null)
  throw new ArgumentNullException (...);

# using static
The using static directive imports a type rather than a namespace. All static members of the imported type can then be used without qualification. In the following example, we call the Console class’s static WriteLine method without needing to refer to the type:

using static System.Console;

WriteLine ("Hello");
The using static directive imports all accessible static members of the type, including fields, properties, and nested types (Chapter 3). You can also apply this directive to enum types (Chapter 3), in which case their members are imported. So, if we import the following enum type

using static System.Windows.Visibility;
we can specify Hidden instead of Visibility.Hidden:

var textBox = new TextBox { Visibility = Hidden };   // XAML-style
Should an ambiguity arise between multiple static imports, the C# compiler is not smart enough to infer the correct type from the context and will generate an error.

# Name scoping
Names declared in outer namespaces can be used unqualified within inner namespaces. In this example, Class1 does not need qualification within Inner:
```c#
namespace Outer
{
  class Class1 {}

  namespace Inner
  {
    class Class2 : Class1  {}
  }
}
```
If you want to refer to a type in a different branch of your namespace hierarchy, you can use a partially qualified name. In the following example, we base SalesReport on Common.ReportBase:
```c#
namespace MyTradingCompany
{
  namespace Common
  {
    class ReportBase {}
  }
  namespace ManagementReporting
  {
    class SalesReport : Common.ReportBase  {}
  }
}
```

# Name hiding
If the same type name appears in both an inner and an outer namespace, the inner name wins. To refer to the type in the outer namespace, you must qualify its name:
```c#
namespace Outer
{
  class Foo { }

  namespace Inner
  {
    class Foo { }

    class Test
    {
      Foo f1;         // = Outer.Inner.Foo
      Outer.Foo f2;   // = Outer.Foo
    }
  }
}
```

# Repeated namespaces
You can repeat a namespace declaration, as long as the type names within the namespaces don’t conflict:
```c#
namespace Outer.Middle.Inner
{
  class Class1 {}
}

namespace Outer.Middle.Inner
{
  class Class2 {}
}
```
We can even break the example into two source files such that we could compile each class into a different assembly.

Source file 1:
```c#
namespace Outer.Middle.Inner
{
  class Class1 {}
}
Source file 2:
```
```c#
namespace Outer.Middle.Inner
{
  class Class2 {}
}
```

# Nested using directives
You can nest a using directive within a namespace. This allows you to scope the using directive within a namespace declaration. In the following example, Class1 is visible in one scope but not in another:
```c#
namespace N1
{
  class Class1 {}
}

namespace N2
{
  using N1;

  class Class2 : Class1 {}
}

namespace N2
{
  class Class3 : Class1 {}   // Compile-time error
}
```