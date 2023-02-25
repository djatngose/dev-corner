# Interfaces
An interface is similar to a class, but only specifies behavior and does not hold state (data). Consequently:

- An interface can define only functions and not fields.

- Interface members are implicitly abstract. (Although nonabstract functions are permitted from C# 8, this is considered a special case, which we describe in “Default Interface Members”.)

- A class (or struct) can implement multiple interfaces. In contrast, a class can inherit from only a single class, and a struct cannot inherit at all (aside from deriving from System.ValueType).

An interface declaration is like a class declaration, but it (typically) provides no implementation for its members because its members are implicitly abstract. These members will be implemented by the classes and structs that implement the interface. An interface can contain `only functions, that is, methods, properties, events, and indexers (which noncoincidentally are precisely the members of a class that can be abstract)`.

```c#
public interface IEnumerator
{
  bool MoveNext();
  object Current { get; }
  void Reset();
}
```
Interface members are always implicitly public and cannot declare an access modifier. Implementing an interface means providing a public implementation for all of its members:
```c#
internal class Countdown : IEnumerator
{
  int count = 11;
  public bool MoveNext() => count-- > 0;
  public object Current => count;
  public void Reset() { throw new NotSupportedException(); }
}
```
You can implicitly cast an object to any interface that it implements:
```c#
IEnumerator e = new Countdown();
while (e.MoveNext())
  Console.Write (e.Current);      // 109876543210

```
Even though Countdown is an internal class, its members that implement IEnumerator can be called publicly by casting an instance of Countdown to IEnumerator. For instance, if a public type in the same assembly defined a method as follows:

public static class Util
{
  public static object GetCountDown() => new CountDown();
}
a caller from another assembly could do this:

IEnumerator e = (IEnumerator) Util.GetCountDown();
e.MoveNext();

If IEnumerator were itself defined as internal, this wouldn’t be possible.

# Extending an Interface
Interfaces can derive from other interfaces; for instance:
```c#
public interface IUndoable             { void Undo(); }
public interface IRedoable : IUndoable { void Redo(); }
```
IRedoable “inherits” all the members of IUndoable. In other words, types that implement IRedoable must also implement the members of IUndoable.

# Explicit Interface Implementation

Implementing multiple interfaces can sometimes result in a collision between member signatures. You can resolve such collisions by explicitly implementing an interface member. Consider the following example:
```c#
interface I1 { void Foo(); }
interface I2 { int Foo(); }

public class Widget : I1, I2
{
  public void Foo()
  {
    Console.WriteLine ("Widget's implementation of I1.Foo");
  }

  int I2.Foo()
  {
    Console.WriteLine ("Widget's implementation of I2.Foo");
    return 42;
  }
}
```