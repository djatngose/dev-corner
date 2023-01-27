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
- C# 7 introduced the deconstruction syntax for tuples (or any type with a Deconstruct method). C# 10 takes this syntax further, letting you mix assignment and declaration in the same deconstruction:
```c#
var point = (3, 4);
double x = 0;
(x, double y) = point;
```