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
- There is also a technology called Blazor that can compile C# to web assembly that runs in a browser.

# .NET 6
- .NET 6 is Microsoft’s flagship open-source runtime. You can write web and console applications that run on Windows, Linux, and macOS; rich-client applications that run on Windows 7 through 11 and macOS; and mobile apps that run on iOS and Android. 

- Unlike .NET Framework, .NET 6 is not preinstalled on Windows machines. If you try to run a .NET 6 application without the correct runtime being present, a message will appear directing you to a web page where you can download the runtime. You can avoid this by creating a self-contained deployment, which includes the parts of the runtime required by the application.

- .NET 6’s predecessor was .NET 5, whose predecessor was .NET Core 3. (Microsoft removed “Core” from the name and skipped version 4.) The reason for skipping a version was to avoid confusion with .NET Framework 4.x.

- This means that assemblies compiled under .NET Core versions 1, 2, and 3 (and .NET 5) will, in most cases, run without modification under .NET 6. In contrast, assemblies compiled under (any version of) .NET Framework are usually incompatible with .NET 6.
- `Note`: The .NET 6 BCL and CLR are very similar to .NET 5 (and .NET Core 3), with their differences centering mostly on performance and deployment.

# MAUI
- MAUI (Multi-platform App UI, early 2022) is designed for creating mobile apps for iOS and Android, as well as cross-platform desktop apps for macOS and Windows. MAUI is an evolution of Xamarin, and allows a single project to target multiple platforms.

# UWP and WinUI 3
- Universal Windows Platform (UWP) is designed for writing immersive touch-first applications that run on Windows 10+ desktop and devices (Xbox, Surface Hub, and HoloLens). UWP apps are sandboxed and ship via the Windows Store. UWP is preinstalled with Windows 10. It uses a version of the .NET Core 2.2 CLR/BCL, and it’s unlikely that this dependency will be updated. Instead, Microsoft has released a successor called WinUI 3, as part of the Windows App SDK.

- The Windows App SDK works with the latest .NET, integrates better with the .NET desktop APIs, and can run outside a sandbox. However, it does not yet support devices such as Xbox or HoloLens.

# .NET Framework
- .NET Framework is Microsoft’s original Windows-only runtime for writing web and rich-client applications that run (only) on Windows desktops/servers. `No major new releases are planned, although Microsoft will continue to support and maintain the current 4.8 release due to the wealth of existing applications`.

- With the .NET Framework, the CLR/BCL is integrated with the application layer. Applications written in .NET Framework can be recompiled under .NET 6, although they usually require some modification. Some features of .NET Framework are not present in .NET 6 (and vice versa).

- .NET Framework is preinstalled with Windows and is automatically patched via Windows Update. When you target .NET Framework 4.8, you can use the features of C# 7.3 and earlier.

