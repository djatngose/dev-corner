# Parameters
A method may have a sequence of parameters. Parameters define the set of arguments that must be provided for that method. In the following example, the method Foo has a single parameter named p, of type int:
```
Foo (8);                        // 8 is an argument
static void Foo (int p) {...}   // p is a parameter
```

You can control how parameters are passed with the ref, in, and out modifiers:

```
Parameter modifier	Passed by	Variable must be definitely assigned
(None)	Value	Going in
ref	Reference	Going in
in	Reference (read-only)	Going in
out	Reference	Going out

```

# Passing arguments by value
By default, arguments in C# are passed by value, which is by far the most common case. This means that a copy of the value is created when passed to the method:
```c#
int x = 8;
Foo (x);                    // Make a copy of x
Console.WriteLine (x);      // x will still be 8

static void Foo (int p)
{
  p = p + 1;                // Increment p by 1
  Console.WriteLine (p);    // Write p to screen
}
```
`Assigning p a new value does not change the contents of x, because p and x reside in different memory locations.`

`Passing a reference-type argument by value copies the reference but not the object`. In the following example, Foo sees the same StringBuilder object we instantiated (sb) but has an independent reference to it. In other words, sb and fooSB are separate variables that reference the same StringBuilder object:

```c#
StringBuilder sb = new StringBuilder();
Foo (sb);
Console.WriteLine (sb.ToString());    // test

static void Foo (StringBuilder fooSB)
{
  fooSB.Append ("test");
  fooSB = null;
}
```
Note: `Because fooSB is a copy of a reference, setting it to null doesn’t make sb null. (If, however, fooSB was declared and called with the ref modifier, sb would become null.)`

# The ref modifier