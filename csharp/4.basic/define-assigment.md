# Definite Assignment
C# enforces a definite assignment policy. In practice, this means that outside of an unsafe or interop context, you can’t accidentally access uninitialized memory. Definite assignment has three implications:

Local variables must be assigned a value before they can be read.

Function arguments must be supplied when a method is called (unless marked as optional; see “Optional parameters”).

All other variables (such as fields and array elements) are automatically initialized by the runtime.

```c#
int x;
Console.WriteLine (x);        // Compile-time error
```

Fields and array elements are automatically initialized with the default values for their type. The following code outputs 0 because array elements are implicitly assigned to their default values:

int[] ints = new int[2];
Console.WriteLine (ints[0]);    // 0

# Default Values
All type instances have a default value. The default value for the predefined types is the result of a bitwise zeroing of memory:
```
Type	Default value
Reference types (and nullable value types)	null
Numeric and enum types	0
char type	'\0'
bool type	false
```
You can obtain the default value for any type via the default keyword:

Console.WriteLine (default (decimal));   // 0
You can optionally omit the type when it can be inferred:

decimal d = default;
`The default value in a custom value type (i.e., struct) is the same as the default value for each field defined by the custom type.`