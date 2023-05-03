# What’s New in C# 7.x
.NET 7 is the successor to .NET 6 and focuses on being unified, modern, simple, and fast. .NET 7 will be supported for `18 months as a standard-term support (STS) release `(previously known as a current release).
# Performance
Performance is a key focus of .NET 7, and all of its features are designed with performance in mind. In addition, .NET 7 includes the following enhancements aimed purely at performance:
  - `On-stack replacement (OSR)` is a complement to tiered compilation. It allows the runtime to change the code executed by a currently running method in the middle of its execution (that is, while it's "on stack"). Long-running methods can switch to more optimized versions mid-execution.
  - `Profile-guided optimization (PGO)` now works with OSR and is easier to enable (by adding <TieredPGO>true</TieredPGO> to your project file). PGO can also instrument and optimize additional things, such as delegates.
  - Improved code generation for Arm64.
  - Native AOT produces a standalone executable in the target platform's file format with no external dependencies. It's entirely native, with no IL or JIT, and provides fast startup time and a small, self-contained deployment. In .NET 7, Native AOT focuses on console apps and requires apps to be trimmed.
  - Performance improvements to the Mono runtime, which powers Blazor WebAssembly, Android, and iOS apps.

## LINQ
LINQ (Language-Integraded Query) is the name for a set of technologies based on the integration of query capabilities directly into C# language.

With vectorization that makes full use of newer hardware that allows .NET to process multiple pieces of data at the same time. The rapidity of aggregation of LINQ is now faster than it has ever been. As an example, a query that searches the average of an array of 1 million numbers runs almost 26 times faster.

The result is astonishingly fast. The time measured here is the time that it took for .NET 6 and .NET 7 to sort an array of 1 000 integers. Almost every aggregation operation of the LINQ is now faster.

## JIT
JIT stands for “Just-In-Time” and refers to a type of compiler that compiles code at runtime, as it is needed, instead of ahead of time. This can improve the performance of a program by allowing the compiler to optimize the code based on the specific environment in which it is running. Using some complex on-stack replacement.

Stack replacement refers to a technique used to optimize the performance of code execution. When a function is called, the arguments and local variables are typically stored in a stack frame. Stack replacement allows the JIT compiler to replace frequently accessed stack values with registers, which are faster to access. This is done by identifying which stack values are accessed most frequently and allocating registers to hold those values instead. The unused stack locations can then be used to store other data or instructions, leading to more efficient memory use and faster code execution.

## Official HTTP 3 Support
Http 3 was shipped as a preview feature in .Net 6 & will be a part of .Net 7 & enabled by default. In future .Net 7 preview versions, we’ll see performance improvements & additional tls features.
# Web development
## Minimal APIs
One of the biggest changes to web development in .NET 6 is the introduction of Minimal APIs.

Minimal APIs are architected to create HTTP APIs with minimal dependencies. This new web framework allows developers to quickly build lightweight HTTP endpoints without the need for a full MVC framework.

With Minimal APIs, you can define your endpoints using simple C# code, making it easy to get started with web development in .NET. They make it ideal for microservices and apps that want to include only the minimum files, features, and dependencies in ASP.NET Core.

.NET 7 builds on the Minimal APIs feature set by adding more capabilities and improvements. Such as endpoint filters, route groups and typed results, which makes minimal APIs even more powerful and efficient. Making it an even more powerful option for building lightweight web applications.


## Blazor
Current browsers have APIs for creating custom components that can include UI elements. These customized elements may then be utilized in any web interface.

Having also improvements for configuring data binding (Get/Set/After Modifiers).
New empty Blazor template to start your project in visual studio.
An expanding support of encryption algorithms.
New events and authentication requests.
Now with no dependency on the Blazor UI component model. You can invoke .NET code from JavaScript using the .NET WebAssembly runtime or use JavaScript functionality from .NET.

## gRPC
gRPC is a RPC (Remote Procedure Call) open-source framework that allows you to build high-performance, scalable APIs that can be used across multiple platforms and languages. .NET 6 introduced improved support for gRPC, making it easier to build gRPC-based applications with .NET.

.NET 7 continues improving gRPC support, such as:

Performance improvements
Create RESTful services with gRPC JSON transcoding
gRPC apps on Azure App Services

## OpenAPI
The OpenAPI Specification (OAS) enables REST-compliant APIs to be described, developed, tested and documented. In a word, it’s a specification for a machine-readable interface definition language with the aim of describing, producing, consuming and visualising web services.

.NET 6 introduced built-in support for OpenAPI, making it easy to generate API documentation and test your APIs using Swagger UI.

.NET 7 is expected to continue improving OpenAPI support, with more features and enhancements that could make it even easier to build and document APIs with .NET. In summary, both .NET 6 and .NET 7 offer a number of improvements and enhancements for web development, with features such as Minimal APIs, Blazor, gRPC, and OpenAPI making it easier to build fast, scalable, and efficient web applications with .NET.


# Desktop Development
## WinForms
WinForm (Short for Windows Form Application) is an UI framework for building Windows desktop applications that exist since around 20 years. Microsoft wants to “help Windows Forms developers to modernize their existing apps and to adopt new cloud-based technologies”. One way to do this is to encourage developers to move from the traditional code-behind pattern to the MVVM pattern. This more modern pattern will help developers to extract business logic from code-behind pages and make the code easier to test and more reusable. To further this strategy, WinForms’ data-binding system has been aligned with WPF and Xamarin’s one, technologies already embracing MVVM.

Additionally, Microsoft’s teams and a few community members worked on a lot of other topics. They have modernized the drag-and-drop and dialog functionalities. They have improved the accessibility, visibility, and automation of WinForms controls. And, finally, they have fixed a few rendering issues of WinForms controls. In .NET 7 devblogs, Microsoft has communicated continuing the tedious process of bug-fixing High DPI and scaling issues for special monitors or multi-monitor setups. 

## WPF
WPF (Windows Presentation Foundation) has also received a couple of updates, which focus on improving performance, accessibility, and bug fixes. The improvement of performance in WPF libraries has been achieved by avoiding the boxing of variables, removing unnecessary objects and collection allocations, and refactoring miscellaneous classes such as wrappers or parsers. This new version of .NET enhances the application accessibility by adding useful new shortcuts, and screen readers now correctly read checkable menu items. Thanks to the community feedback, many bugs have been fixed and Microsoft plans to make internal integration tests public on GitHub, which will allow developers to further improve quality control by adding their own integration tests in this repository. Microsoft’s internal team is very grateful for the community’s involvement and continues to push quality control through basic regression tests on pull requests.



1. LINQ
LINQ (Language-Integraded Query) is the name for a set of technologies based on the integration of query capabilities directly into C# language.

With vectorization that makes full use of newer hardware that allows .NET to process multiple pieces of data at the same time. The rapidity of aggregation of LINQ is now faster than it has ever been. As an example, a query that searches the average of an array of 1 million numbers runs almost 26 times faster.

Thanks to .NET being open source, you can check the source code and see that they arrived at that performance using the Vector class.

As many developers use LINQ for a plethora of use cases, it is a big plus for .NET.


The result is astonishingly fast. The time measured here is the time that it took for .NET 6 and .NET 7 to sort an array of 1 000 integers. Almost every aggregation operation of the LINQ is now faster.

2. JIT
JIT stands for “Just-In-Time” and refers to a type of compiler that compiles code at runtime, as it is needed, instead of ahead of time. This can improve the performance of a program by allowing the compiler to optimize the code based on the specific environment in which it is running. Using some complex on-stack replacement.

Stack replacement refers to a technique used to optimize the performance of code execution. When a function is called, the arguments and local variables are typically stored in a stack frame. Stack replacement allows the JIT compiler to replace frequently accessed stack values with registers, which are faster to access. This is done by identifying which stack values are accessed most frequently and allocating registers to hold those values instead. The unused stack locations can then be used to store other data or instructions, leading to more efficient memory use and faster code execution.

Web Development
Both .NET 6 and .NET 7 come with improvements to web development. For example, .NET 6 introduced a new web framework called minimal APIs, which allows developers to quickly build lightweight HTTP endpoints without the need for a full MVC framework. .NET 7 is expected to build on this with more improvements to minimal APIs, as well as other web development features such as improved support for gRPC.

1. Minimal APIs
One of the biggest changes to web development in .NET 6 is the introduction of Minimal APIs.

Minimal APIs are architected to create HTTP APIs with minimal dependencies. This new web framework allows developers to quickly build lightweight HTTP endpoints without the need for a full MVC framework.

With Minimal APIs, you can define your endpoints using simple C# code, making it easy to get started with web development in .NET. They make it ideal for microservices and apps that want to include only the minimum files, features, and dependencies in ASP.NET Core.

.NET 7 builds on the Minimal APIs feature set by adding more capabilities and improvements. Such as endpoint filters, route groups and typed results, which makes minimal APIs even more powerful and efficient. Making it an even more powerful option for building lightweight web applications.

2. Blazor
BlazorBlazor is a well-known web development framework that enables developers to create web apps in C# and .NET rather than JavaScript. Blazor received a number of enhancements with.NET 6, including enhanced support for server-side rendering and increased speed. NET 7 continues to improve Blazor, adding new features and additions that might make it even more powerful and user-friendly.

For example, you might use Blazor components from existing JavaScript projects, such as those created using well-known front-end frameworks like Angular, React, or Vue.

Current browsers have APIs for creating custom components that can include UI elements. These customized elements may then be utilized in any web interface.

Having also improvements for configuring data binding (Get/Set/After Modifiers).
New empty Blazor template to start your project in visual studio.
An expanding support of encryption algorithms.
New events and authentication requests.
Now with no dependency on the Blazor UI component model. You can invoke .NET code from JavaScript using the .NET WebAssembly runtime or use JavaScript functionality from .NET.
3. gRPC
gRPC is a RPC (Remote Procedure Call) open-source framework that allows you to build high-performance, scalable APIs that can be used across multiple platforms and languages. .NET 6 introduced improved support for gRPC, making it easier to build gRPC-based applications with .NET.

.NET 7 continues improving gRPC support, such as:

Performance improvements
Create RESTful services with gRPC JSON transcoding
gRPC apps on Azure App Services
4. OpenAPI
The OpenAPI Specification (OAS) enables REST-compliant APIs to be described, developed, tested and documented. In a word, it’s a specification for a machine-readable interface definition language with the aim of describing, producing, consuming and visualising web services.

.NET 6 introduced built-in support for OpenAPI, making it easy to generate API documentation and test your APIs using Swagger UI.

OpenAPI
.NET 7 is expected to continue improving OpenAPI support, with more features and enhancements that could make it even easier to build and document APIs with .NET. In summary, both .NET 6 and .NET 7 offer a number of improvements and enhancements for web development, with features such as Minimal APIs, Blazor, gRPC, and OpenAPI making it easier to build fast, scalable, and efficient web applications with .NET.

Desktop Development
.NET 7 brings significant updates and improvements to desktop (and mobile) development frameworks making them more modern, simple, and fast:

1. WinForms
WinForm (Short for Windows Form Application) is an UI framework for building Windows desktop applications that exist since around 20 years. Microsoft wants to “help Windows Forms developers to modernize their existing apps and to adopt new cloud-based technologies”. One way to do this is to encourage developers to move from the traditional code-behind pattern to the MVVM pattern. This more modern pattern will help developers to extract business logic from code-behind pages and make the code easier to test and more reusable. To further this strategy, WinForms’ data-binding system has been aligned with WPF and Xamarin’s one, technologies already embracing MVVM.

data bindings
Previously in this article, we discussed JIT compilation. Microsoft encouraged and eased the experimentation of publishing applications with NativeAOT, an alternative to JIT, by adding a single line in the .csproj file. The benefits expected of using this technology are a faster startup time, lower memory usage, and the ability to run the application on any Windows or Linux machine that doesn’t have the .NET runtime installed.

Additionally, Microsoft’s teams and a few community members worked on a lot of other topics. They have modernized the drag-and-drop and dialog functionalities. They have improved the accessibility, visibility, and automation of WinForms controls. And, finally, they have fixed a few rendering issues of WinForms controls. In .NET 7 devblogs, Microsoft has communicated continuing the tedious process of bug-fixing High DPI and scaling issues for special monitors or multi-monitor setups. 

2. WPF
WPF (Windows Presentation Foundation) has also received a couple of updates, which focus on improving performance, accessibility, and bug fixes. The improvement of performance in WPF libraries has been achieved by avoiding the boxing of variables, removing unnecessary objects and collection allocations, and refactoring miscellaneous classes such as wrappers or parsers. This new version of .NET enhances the application accessibility by adding useful new shortcuts, and screen readers now correctly read checkable menu items. Thanks to the community feedback, many bugs have been fixed and Microsoft plans to make internal integration tests public on GitHub, which will allow developers to further improve quality control by adding their own integration tests in this repository. Microsoft’s internal team is very grateful for the community’s involvement and continues to push quality control through basic regression tests on pull requests.

3. .NET MAUI
.NET Multiple-platform App UI is the framework used to build native, cross-platform desktop and mobile apps from a single C# codebase for Android, iOS, Mac, and Windows. A ton of new controls have been added to the young MAUI framework released in April 2022. The long-awaited official Map control has finally become available, making it easier to add a map, markers, polygons, pins, user geolocation, traffic, etc in your mobile and desktop application.

In addition, a TwoPaneView control has been added. This control positions child content side-by-side or vertically and aligns its child contents based on the hinge or the screen fold on foldable Android devices. And many basic UI desktop features have also been added, such as context menus, tooltips, right clicks, pointer gestures, and pointer tracking. More global improvements are also very noticeable, such as the new delegates used in the iOS life cycle and updates of the Window class that allows for more precise management of the application window’s position and size in desktop versions.
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

# REF
  https://positivethinking.tech/insights/net-6-vs-net-7-deep-dive-into-the-upgrade/