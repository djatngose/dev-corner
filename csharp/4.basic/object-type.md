# The object Type
object (System.Object) is the ultimate base class for all types. Any type can be upcast to object.

To illustrate how this is useful, consider a general-purpose stack. A stack is a data structure based on the principle of LIFO—“last in, first out.” A stack has two operations: push an object on the stack, and pop an object off the stack. Here is a simple implementation that can hold up to 10 objects:

```c#
public class Stack
{
  int position;
  object[] data = new object[10];
  public void Push (object obj)   { data[position++] = obj;  }
  public object Pop()             { return data[--position]; }
}
```
Because Stack works with the object type, we can Push and Pop instances of any type to and from the Stack:
```c#
Stack stack = new Stack();
stack.Push ("sausage");
string s = (string) stack.Pop();   // Downcast, so explicit cast is needed

Console.WriteLine (s);             // sausage

```

object is a reference type, by virtue of being a class. Despite this, value types, such as int, can also be cast to and from object, and so be added to our stack. This feature of C# is called type unification and is demonstrated here:
stack.Push (3);
int three = (int) stack.Pop();
When you cast between a value type and object, the CLR must perform some special work to bridge the difference in semantics between value and reference types. This process is called boxing and unboxing.

# Boxing and Unboxing
Boxing is the act of converting a value-type instance to a reference-type instance. The reference type can be either the object class or an interface (which we visit later in the chapter).1 In this example, we box an int into an object:

int x = 9;
object obj = x;           // Box the int
Unboxing reverses the operation by casting the object back to the original value type:

int y = (int)obj;         // Unbox the int
Unboxing requires an explicit cast. The runtime checks that the stated value type matches the actual object type, and throws an InvalidCastException if the check fails. For instance, the following throws an exception because long does not exactly match int:

object obj = 9;           // 9 is inferred to be of type int
long x = (long) obj;      // InvalidCastException
The following succeeds, however:

object obj = 9;
long x = (int) obj;
As does this:

object obj = 3.5;              // 3.5 is inferred to be of type double
int x = (int) (double) obj;    // x is now 3
In the last example, (double) performs an unboxing, and then (int) performs a numeric conversion.

# Copying semantics of boxing and unboxing
Boxing copies the value-type instance into the new object, and unboxing copies the contents of the object back into a value-type instance. In the following example, changing the value of i doesn’t change its previously boxed copy:

int i = 3;
object boxed = i;
i = 5;
Console.WriteLine (boxed);    // 3

# when box and unbox happen?
`Boxing` occurs in the following cases in C#:

Assigning a value type to a variable of type object.
```c#
int x = 42;
object obj = x; // box operation occurs here
```
Assigning a value type to a variable of type System.ValueType.
```c#
int x = 42;
object[] array = new object[] { x }; // box operation occurs here
```
Assigning a value type to a variable of an interface that the value type implements.
```c#
int x = 42;
IConvertible convertible = x; // box operation occurs here
```
Passing a value type as an argument to a method that expects an object or System.ValueType parameter.

Using a value type as a type argument for a generic type constraint.
```c#
int x = 42;
ArrayList list = new ArrayList();
list.Add(x); // box operation occurs here

```
Accessing a value type through a reference type.
```

```
`Unboxing` occurs when a boxed value type is converted back to its original value type. This can happen when:

A boxed value type is assigned to a variable of its original value type.
A boxed value type is passed as an argument to a method that expects its original value type.
A boxed value type is accessed through a reference type and is subsequently assigned to a variable of its original value type.

# How box and unbox impact the performance in c#?
Boxing and unboxing can have a significant impact on the performance of a C# application. Boxed value types are allocated on the heap, which can result in additional memory allocation and garbage collection overhead. Additionally, unboxing involves casting the value type back to its original form, which can also result in a performance penalty.

Unboxing involves the conversion of a value type, which is stored on the heap as an object, back to its original value type. This process can be costly in terms of performance because it involves copying the value from the heap onto the stack, which can be an expensive operation. Additionally, unboxing requires additional type checking and casting, which can add extra overhead. Therefore, unboxing can impact performance just as much as boxing.

Consider the following example:

java
Copy code
int x = 42;
object obj = x; // Boxing occurs here
int y = (int)obj; // Unboxing occurs here
In this example, the value of x is boxed into an object on the heap, which involves allocating memory for the object and copying the value of x into the object. When the value is later unboxed, it must be cast back to its original type (int), which can result in a performance penalty.


To avoid the performance penalty of boxing and unboxing, it is often better to use generic collections such as List<T> and Dictionary<TKey, TValue> instead of non-generic collections such as ArrayList and Hashtable, which require boxing and unboxing of value types. It is also generally a good practice to use value types instead of reference types whenever possible, to minimize the need for boxing and unboxing.






# Static and Runtime Type Checking
C# programs are type-checked both statically (at compile time) and at runtime (by the CLR).

Static type checking enables the compiler to verify the correctness of your program without running it. The following code will fail because the compiler enforces static typing:

int x = "5";
Runtime type checking is performed by the CLR when you downcast via a reference conversion or unboxing:

object y = "5";
int z = (int) y;          // Runtime error, downcast failed
Runtime type checking is possible because each object on the heap internally stores a little type token. You can retrieve this token by calling the GetType method of object.

# The GetType Method and typeof Operator
All types in C# are represented at runtime with an instance of System.Type. There are two basic ways to get a System.Type object:

Call GetType on the instance.

Use the typeof operator on a type name.

GetType is evaluated at runtime; typeof is evaluated statically at compile time (when generic type parameters are involved, it’s resolved by the JIT compiler).

System.Type has properties for such things as the type’s name, assembly, base type, and so on:

Point p = new Point();
Console.WriteLine (p.GetType().Name);             // Point
Console.WriteLine (typeof (Point).Name);          // Point
Console.WriteLine (p.GetType() == typeof(Point)); // True
Console.WriteLine (p.X.GetType().Name);           // Int32
Console.WriteLine (p.Y.GetType().FullName);       // System.Int32

public class Point { public int X, Y; }

# The ToString Method
The ToString method returns the default textual representation of a type instance. This method is overridden by all built-in types. Here is an example of using the int type’s ToString method:

int x = 1;
string s = x.ToString();     // s is "1"
You can override the ToString method on custom types as follows:

Panda p = new Panda { Name = "Petey" };
Console.WriteLine (p);   // Petey

public class Panda
{
  public string Name;
  public override string ToString() => Name;
}
If you don’t override ToString, the method returns the type name.

`Note`
When you call an overridden object member such as ToString directly on a value type, boxing doesn’t occur. Boxing then occurs only if you cast:

int x = 1;
string s1 = x.ToString();    // Calling on nonboxed value
object box = x;
string s2 = box.ToString();  // Calling on boxed value

# Object Member Listing
Here are all the members of object:

public class Object
{
  public Object();

  public extern Type GetType();

  public virtual bool Equals (object obj);
  public static bool Equals  (object objA, object objB);
  public static bool ReferenceEquals (object objA, object objB);

  public virtual int GetHashCode();

  public virtual string ToString();

  protected virtual void Finalize();
  protected extern object MemberwiseClone();
}