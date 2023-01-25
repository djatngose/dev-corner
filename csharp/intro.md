# Introducing C# and .NET
- C# is a general-purpose, type-safe, object-oriented programming language. The goal of the language is programmer productivity
- C# is a rich implementation of the object-orientation paradigm, which includes encapsulation, inheritance, and polymorphism. Encapsulation means creating a boundary around an object to separate its `external (public)` behavior from its `internal (private)` implementation details. There are features from object-oriented perspective:
  - Classes and interfaces
  - Properties, methods, and events

# Type Safety
- C# is primarily a type-safe language, meaning that instances of types can interact only through protocols they define, thereby ensuring each type’s internal consistency. For instance, C# prevents you from interacting with a string type as though it were an integer type.
- C# supports static typing, meaning that the language enforces type safety at compile time. This is in addition to type safety being enforced at runtime. Static typing eliminates a large class of errors before a program is even run. It shifts the burden away from runtime unit tests onto the compiler to verify that all the types in a program fit together correctly.

# Memory Management
- C# relies on the runtime to perform automatic memory management. The Common Language Runtime has a garbage collector that executes as part of your program, reclaiming memory for objects that are no longer referenced. This frees programmers from explicitly deallocating the memory for an object, eliminating the problem of incorrect pointers encountered in languages such as C++.
- C# does not eliminate pointers: it merely makes them unnecessary for most programming tasks. For performance-critical hotspots and interoperability, pointers and explicit memory allocation is permitted in blocks that are marked `unsafe`.

# Platform Support
- Window
- MacOS
- Linux
- Android and iOS
- Widnow devices(Xbox, surface hub, Hololens)