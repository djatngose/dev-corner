# Value types
Value types comprise most built-in types (specifically, all numeric types, the char type, and the bool type) as well as custom struct and enum types.

The fundamental difference between value types and reference types is how they are handled in memory.

The content of a value type variable or constant is simply a value. For example, the content of the built-in value type, int, is 32 bits of data.

# predefined type
Value types
Numeric

  Signed integer (sbyte, short, int, long)

  Unsigned integer (byte, ushort, uint, ulong)

  Real number (float, double, decimal)

Logical (bool)

Character (char)
Datime
Null
Enum



You can define a custom value type with the struct keyword (see Figure 2-1):
```c#
public struct Point { public int X; public int Y; }
```
Or more tersely:
```c#
public struct Point { public int X, Y; }
```
The assignment of a value-type instance always copies the instance; for example:

```c#
Point p1 = new Point();
p1.X = 7;

Point p2 = p1;             // Assignment causes copy

Console.WriteLine (p1.X);  // 7
Console.WriteLine (p2.X);  // 7

p1.X = 9;                  // Change p1.X

Console.WriteLine (p1.X);  // 9
Console.WriteLine (p2.X);  // 7

```

# Can not assign null
In contrast, a value type cannot ordinarily have a null value:
```c#
Point p = null;  // Compile-time error
int x = null;    // Compile-time error

struct Point {...}
```

# Storage overhead
Value-type instances occupy precisely the memory required to store their fields. In this example, Point takes 8 bytes of memory:

```c#
struct Point
{
  int x;  // 4 bytes
  int y;  // 4 bytes
}
```
Technically, the CLR positions fields within the type at an address that’s a multiple of the fields’ size (up to a maximum of 8 bytes). Thus, the following actually consumes 16 bytes of memory (with the 7 bytes following the first field “wasted”):
```c#
struct A { byte b; long l; }
```