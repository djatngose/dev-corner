# when  another assembly requires recompilation of current assembly in c#?
In C#, another assembly may require `recompilation` of the current assembly in the following scenarios:

`Change in Public Interface`: If the public interface of the current assembly changes, such as adding, removing, or modifying a `public method, property, field, or event, any assemblies` that depend on it will need to be recompiled to use the new interface. This is because the public interface defines the contract between the current assembly and its consumers, and any changes to the interface can break the compatibility between the two.

`Change in Assembly Signature:` If the digital signature of the current assembly changes, such as when the assembly is signed with a new key or certificate, any assemblies that depend on it will need to be recompiled to use the new signature. This is because the signature is used to verify the authenticity and integrity of the assembly, and any changes to the signature can cause the assembly to fail to load or execute properly.

`Change in Assembly References`: If the current assembly depends on other assemblies that change, such as when a referenced assembly is updated to a new version or replaced with a different assembly, the current assembly may need to be recompiled to use the new references. This is because the references define the types and members that the current assembly depends on, and any changes to the references can cause the types and members to become unavailable or incompatible.

## Example
Suppose we have a library project called `MyLib` that contains a class called `MyClass` with `two public methods, MethodA and MethodB:`
```c#
namespace MyLib
{
    public class MyClass
    {
        public void MethodA()
        {
            // implementation
        }

        public void MethodB()
        {
            // implementation
        }
    }
}

```
Now suppose we have a client application that references MyLib and uses MethodA:

csharp

```c#
using MyLib;

namespace MyApp
{
    class Program
    {
        static void Main(string[] args)
        {
            MyClass obj = new MyClass();
            obj.MethodA();
        }
    }
}

```
If we add a new public method called MethodC to MyClass in MyLib, and recompile MyLib, the client application will need to be recompiled as well to use the new method:
```c#
namespace MyLib
{
    public class MyClass
    {
        public void MethodA()
        {
            // implementation
        }

        public void MethodB()
        {
            // implementation
        }

        public void MethodC()
        {
            // implementation
        }
    }
}

```
```c#
using MyLib;

namespace MyApp
{
    class Program
    {
        static void Main(string[] args)
        {
            MyClass obj = new MyClass();
            obj.MethodA();
            obj.MethodC(); // compile error until MyApp is recompiled
        }
    }
}

```
In this case, the addition of MethodC changes the public interface of MyClass, and any client applications that use MyClass will need to be recompiled to use the new method. If the client application is not recompiled, it will fail to build due to a compile error when attempting to call MethodC.

# Changing a public property
Suppose we have a library project called MyLib that contains a class called MyClass with a public property called MyProperty:
namespace MyLib
{
    public class MyClass
    {
        public string MyProperty { get; set; } // change data type to string
    }
}

Now suppose we have a client application that references MyLib and uses MyClass and MyProperty:

using MyLib;

namespace MyApp
{
    class Program
    {
        static void Main(string[] args)
        {
            MyClass obj = new MyClass();
            obj.MyProperty = 42;
        }
    }
}


If we change the definition of MyProperty to a different data type in MyLib, and recompile MyLib, the client application will need to be recompiled as well to use the new data type:

namespace MyLib
{
    public class MyClass
    {
        public string MyProperty { get; set; } // change data type to string
    }
}

using MyLib;

namespace MyApp
{
    class Program
    {
        static void Main(string[] args)
        {
            MyClass obj = new MyClass();
            obj.MyProperty = "hello"; // compile error until MyApp is recompiled
        }
    }
}

In this case, changing the data type of MyProperty changes the public interface of MyClass, and any client applications that use MyClass will need to be recompiled to use the new data type. If the client application is not recompiled, it will fail to build due to a compile error when attempting to set MyProperty to a value of the wrong type.


# The this Reference
The `this` reference refers to the `instance itself`. In the following example, the Marry method uses this to set the partner’s mate field:
```c#
public class Panda
{
  public Panda Mate;

  public void Marry (Panda partner)
  {
    Mate = partner;
    partner.Mate = this;
  }
}
```

The this reference also disambiguates a local variable or parameter from a field; for example:
```c#
public class Test
{
  string name;
  public Test (string name) { this.name = name; }
}
```
`Note`: The this reference is valid only within nonstatic members of a class or struct.

# Properties
Properties look like fields from the outside, but internally they contain logic, like methods do. For example, you can’t tell by looking at the following code whether CurrentPrice is a field or a property:
```c#
Stock msft = new Stock();
msft.CurrentPrice = 30;
msft.CurrentPrice -= 3;
Console.WriteLine (msft.CurrentPrice);
```

Properties allow the following modifiers:

Static modifier	`static`
Access modifiers	`public internal private protected`
Inheritance modifiers	`new virtual abstract override sealed`
Unmanaged code modifiers	`unsafe extern`

## Init-only setters
From C# 9, you can declare a property accessor with init instead of set:
```c#
public class Note
{
  public int Pitch    { get; init; } = 20;   // “Init-only” property
  public int Duration { get; init; } = 100;  // “Init-only” property
}
```
These init-only properties act like read-only properties, except that they can also be set via an object initializer:
```c#
var note = new Note { Pitch = 50 }; // OK when there are constructor predefined

note.Pitch = 123;   // Error – init-only setter!
```
`Init-only properties cannot even be set from inside their class, except via their property initializer, the constructor, or another init-only accessor.`

The alternative to init-only properties is to have read-only properties that you populate via a constructor:

```c#
public class Note
{
  public int Pitch    { get; }
  public int Duration { get; }

  public Note (int pitch = 20, int duration = 100)
  {
    Pitch = pitch; Duration = duration;
  }
}

```
Should the class be part of a public library, `this approach makes versioning difficult, in that adding an optional parameter to the constructor at a later date breaks binary compatibility with consumers (whereas adding a new init-only property breaks nothing).`

As with ordinary set accessors, init-only accessors can provide an implementation:
```c#
public class Note
{
  readonly int _pitch;
  public int Pitch { get => _pitch; init => _pitch = value; }
  ...
```
Notice that the _pitch field is read-only: init-only setters are permitted to modify readonly fields in their own class. (Without this feature, _pitch would need to be writable, and the class would fail at being internally immutable.)
`Waring`:
Changing a property’s accessor from init to set (or vice versa) is a binary breaking change: anyone that references your assembly will need to `recompile` their assembly.

This should not be an issue when creating wholly immutable types, in that your type will never require properties with a (writable) set accessor.

# CLR property implementation
C# property accessors internally compile to methods called get_XXX and set_XXX:

public decimal get_CurrentPrice {...}
public void set_CurrentPrice (decimal value) {...}
An init accessor is processed like a set accessor, but with an extra flag encoded into the set accessor’s “modreq” metadata (see “Init-only properties”).

Simple nonvirtual property accessors are inlined by the Just-in-Time (JIT) compiler, eliminating any performance difference between accessing a property and a field. Inlining is an optimization in which a method call is replaced with the body of that method.

# Indexers
Indexers provide a natural syntax for accessing elements in a class or struct that encapsulate a list or dictionary of values. Indexers are similar to properties but are accessed via an index argument rather than a property name. The string class has an indexer that lets you access each of its char values via an int index:

string s = "hello";
Console.WriteLine (s[0]); // 'h'
Console.WriteLine (s[3]); // 'l'

## Implementing an indexer
To write an indexer, define a property called this, specifying the arguments in square brackets:

class Sentence
{
  string[] words = "The quick brown fox".Split();

  public string this [int wordNum]      // indexer
  {
    get { return words [wordNum];  }
    set { words [wordNum] = value; }
  }
}
Here’s how we could use this indexer:

Sentence s = new Sentence();
Console.WriteLine (s[3]);       // fox
s[3] = "kangaroo";
Console.WriteLine (s[3]);       // kangaroo

A type can declare multiple indexers, each with parameters of different types. An indexer can also take more than one parameter:

public string this [int arg1, string arg2]
{
  get { ... }  set { ... }
}
If you omit the set accessor, an indexer becomes read-only, and you can use expression-bodied syntax to shorten its definition:

public string this [int wordNum] => words [wordNum];

## CLR indexer implementation
Indexers internally compile to methods called get_Item and set_Item, as follows:

public string get_Item (int wordNum) {...}
public void set_Item (int wordNum, string value) {...}

## Using indices and ranges with indexers
You can support indices and ranges (see “Indices and ranges”) in your own classes by defining an indexer with a parameter type of Index or Range. We could extend our previous example by adding the following indexers to the Sentence class:

  public string this [Index index] => words [index];
  public string[] this [Range range] => words [range];
This then enables the following:

Sentence s = new Sentence();
Console.WriteLine (s [^1]);         // fox  
string[] firstTwoWords = s [..2];   // (The, quick)

# Static Constructors
A static constructor executes once per type rather than once per instance. A type can define only one static constructor, and it must be parameterless and have the same name as the type:

class Test
{
  static Test() { Console.WriteLine ("Type Initialized"); }
}

The runtime automatically invokes a static constructor just prior to the type being used. Two things trigger this:

`Instantiating the type`

`Accessing a static member in the type`
The only modifiers allowed by static constructors are unsafe and extern.
The static constructor is called only once, when the class is first loaded into memory. The instance constructor is called each time an instance of the class is create

```c#
var a = new Test();
var b = new Test(); 
class Test
{
    public Test()
    {
        Console.WriteLine("Instance constructor called.");
    }
    static Test() { Console.WriteLine ("Type Initialized"); }
}

//OUTPUT
Type Initialized
Instance constructor called.
Instance constructor called.

```

From C# 9, you can also define `module initializers`, which execute once per assembly (when the assembly is first loaded). To define a module initializer, write a static void method and then apply the [ModuleInitializer] attribute to that method:

[System.Runtime.CompilerServices.ModuleInitializer]
internal static void InitAssembly()
{
  ...
} 

ModuleInitializer is a new attribute in C# 9 that allows you to specify a method to be called automatically when a module is loaded. A module is essentially a compiled unit of code, such as an assembly or a .NET module.

The ModuleInitializer attribute can be used to specify a static method that should be executed when the module is loaded. This can be useful for performing initialization tasks, such as registering services or initializing static data.

The method that is marked with the ModuleInitializer attribute must be static and have a return type of void. It can take any number of parameters, but these must be optional.
```c#
using System.Runtime.CompilerServices;

class Program
{
    static void Main(string[] args)
    {
        // Do something
    }
}

public static class MyInitializer
{
    [ModuleInitializer]
    public static void Initialize()
    {
        // Perform initialization tasks
    }
}
class A
{
    public string myString { get; set; }
}

var a = new A();
a.myString = "abbb";
Console.WriteLine("aaaaa");
Console.WriteLine(a.myString);

//OUTPUT
 initialization tasks
aaaaa
abbb

```
# Static constructors and field initialization order
Static field initializers run just before the static constructor is called. If a type has no static constructor, static field initializers will execute just prior to the type being used—or anytime earlier at the whim of the runtime.

Static field initializers run in the order in which the fields are declared. The following example illustrates this. X is initialized to 0, and Y is initialized to 3:
```c#
class Foo
{
  public static int X = Y;    // 0
  public static int Y = 3;    // 3
}
```
If we swap the two field initializers around, both fields are initialized to 3. The next example prints 0 followed by 3 because the field initializer that instantiates a Foo executes before X is initialized to 3:
```c#
Console.WriteLine (Foo.X);    // 3

class Foo
{
  public static Foo Instance = new Foo();
  public static int X = 3;

  Foo() => Console.WriteLine (X);    // 0
}
//OUTPUT
0
3
```
If we swap the two lines in boldface, the example prints 3 followed by 3.

# Static Classes
A class marked static cannot be instantiated or subclassed, and must be composed solely of static members. The System.Console and System.Math classes are good examples of static classes.
# Finalizers
Finalizers are class-only methods that execute before the garbage collector reclaims the memory for an unreferenced object. The syntax for a finalizer is the name of the class prefixed with the ~ symbol:

class Class1
{
  ~Class1()
  {
    ...
  }
}
This is actually C# syntax for overriding Object’s Finalize method, and the compiler expands it into the following method declaration:

protected override void Finalize()
{
  ...
  base.Finalize();
}
Finalizers allow the following modifier:

Unmanaged code modifier	`unsafe`

You can write single-statement finalizers using expression-bodied syntax:

~Class1() => Console.WriteLine ("Finalizing");

# Partial Types and Methods
Partial types allow a type definition to be split—typically across multiple files. A common scenario is for a partial class to be autogenerated from some other source (such as a Visual Studio template or designer), and for that class to be augmented with additional hand-authored methods:
```c#
// PaymentFormGen.cs - auto-generated
partial class PaymentForm { ... }

// PaymentForm.cs - hand-authored
partial class PaymentForm { ... }
```
Participants cannot have conflicting members. A constructor with the same parameters, for instance, cannot be repeated. Partial types are resolved entirely by the compiler, which means that each participant must be available at compile time and must reside in the same assembly.
You can specify a base class on one or more partial class declarations, as long as the base class, if specified, is the same. In addition, each participant can independently specify interfaces to implement. 
The compiler makes no guarantees with regard to field initialization order between partial type declarations.

## Partial methods
A partial type can contain partial methods. These let an autogenerated partial type provide customizable hooks for manual authoring; for example:

partial class PaymentForm    // In auto-generated file
{
  ...
  partial void ValidatePayment (decimal amount);
}

partial class PaymentForm    // In hand-authored file
{
  ...
  partial void ValidatePayment (decimal amount)
  {
    if (amount > 100)
      ...
  }
}

A partial method consists of two parts: a definition and an implementation. The definition is typically written by a code generator, and the implementation is typically manually authored. If an implementation is not provided, the definition of the partial method is compiled away (as is the code that calls it). This allows autogenerated code to be liberal in providing hooks without having to worry about bloat. Partial methods must be void and are implicitly private. They cannot include out parameters.

## Extended partial methods
Extended partial methods (from C# 9) are designed for the reverse code generation scenario, where a programmer defines hooks that a code generator implements. An example of where this might occur is with source generators, a Roslyn feature that lets you feed the compiler an assembly that automatically generates portions of your code.

A partial method declaration is extended if it begins with an accessibility modifier:

public partial class Test
{
  public partial void M1();    // Extended partial method
  private partial void M2();   // Extended partial method
}
The presence of the accessibility modifier doesn’t just affect accessibility: it tells the compiler to treat the declaration differently.

Extended partial methods must have implementations; they do not melt away if unimplemented. In this example, both M1 and M2 must have implementations because they each specify accessibility modifiers (public and private).

Because they cannot melt away, extended partial methods can return any type and can include out parameters:

public partial class Test
{
  public partial bool IsValid (string identifier);
  internal partial bool TryParse (string number, out int result);
}

# The nameof operator
The nameof operator returns the name of any symbol (type, member, variable, and so on) as a string:

int count = 123;
string name = nameof (count);       // name is "count" 
Its advantage over simply specifying a string is that of static type checking. Tools such as Visual Studio can understand the symbol reference, so if you rename the symbol in question, all of its references will be renamed, too.

To specify the name of a type member such as a field or property, include the type as well. This works with both static and instance members:

string name = nameof (StringBuilder.Length);
This evaluates to Length. To return StringBuilder.Length, you would do this:

nameof (StringBuilder) + "." + nameof (StringBuilder.Length);

# Inheritance
A class can inherit from another class to extend or customize the original class. Inheriting from a class lets you reuse the functionality in that class instead of building it from scratch. A class can inherit from only a single class but can itself be inherited by many classes, thus forming a class hierarchy. In this example, we begin by defining a class called Asset:

public class Asset
{
  public string Name;
}
Next, we define classes called Stock and House, which will inherit from Asset. Stock and House get everything an Asset has, plus any additional members that they define:

public class Stock : Asset   // inherits from Asset
{
  public long SharesOwned;
}

public class House : Asset   // inherits from Asset
{
  public decimal Mortgage;
}
Here’s how we can use these classes:

Stock msft = new Stock { Name="MSFT",
                         SharesOwned=1000 };

Console.WriteLine (msft.Name);         // MSFT
Console.WriteLine (msft.SharesOwned);  // 1000

House mansion = new House { Name="Mansion",
                            Mortgage=250000 };

Console.WriteLine (mansion.Name);      // Mansion
Console.WriteLine (mansion.Mortgage);  // 250000
The derived classes, Stock and House, inherit the Name field from the base class, Asset.

`NOTE`:
A derived class is also called a `subclass`.

A base class is also called a `superclass`.

# Polymorphism
References are polymorphic. This means a variable of type x can refer to an object that subclasses x. For instance, consider the following method:

public static void Display (Asset asset)
{
  System.Console.WriteLine (asset.Name);
}
This method can display both a Stock and a House because they are both Assets:

Stock msft    = new Stock ... ;
House mansion = new House ... ;

Display (msft);
Display (mansion);
Polymorphism works on the basis that subclasses (Stock and House) have all the features of their base class (Asset). The converse, however, is not true. If Display was modified to accept a House, you could not pass in an Asset:

Display (new Asset());     // Compile-time error

public static void Display (House house)         // Will not accept Asset
{
  System.Console.WriteLine (house.Mortgage);
}
## Casting and Reference Conversions
An object reference can be:

Implicitly upcast to a base class reference

Explicitly downcast to a subclass reference

Upcasting and downcasting between compatible reference types performs reference conversions: a new reference is (logically) created that points to the same object. An upcast always succeeds; a downcast succeeds only if the object is suitably typed.
## Upcasting
An upcast operation creates a base class reference from a subclass reference:

Stock msft = new Stock();
Asset a = msft;              // Upcast
After the upcast, variable a still references the same Stock object as variable msft. The object being referenced is not itself altered or converted:

Console.WriteLine (a == msft);        // True
Although a and msft refer to the identical object, a has a more restrictive view on that object:

Console.WriteLine (a.Name);           // OK
Console.WriteLine (a.SharesOwned);    // Compile-time error
The last line generates a compile-time error because the variable a is of type Asset, even though it refers to an object of type Stock. To get to its SharesOwned field, you must downcast the Asset to a Stock.

## Downcasting
A downcast operation creates a subclass reference from a base class reference:

Stock msft = new Stock();
Asset a = msft;                      // Upcast
Stock s = (Stock)a;                  // Downcast
Console.WriteLine (s.SharesOwned);   // <No error>
Console.WriteLine (s == a);          // True
Console.WriteLine (s == msft);       // True
As with an upcast, only references are affected—not the underlying object. A downcast requires an explicit cast because it can potentially fail at runtime:

House h = new House();
Asset a = h;               // Upcast always succeeds
Stock s = (Stock)a;        // Downcast fails: a is not a Stock
If a downcast fails, an InvalidCastException is thrown. This is an example of runtime type checking (we elaborate on this concept in “Static and Runtime Type Checking”).

## The as operator
The as operator performs a `downcast` that evaluates to null (rather than throwing an exception) if the downcast fails:

Asset a = new Asset();
Stock s = a as Stock;       // s is null; no exception thrown
This is useful when you’re going to subsequently test whether the result is null:

if (s != null) Console.WriteLine (s.SharesOwned);

`Note`:

Without such a test, `a cast is advantageous, because if it fails`, a more helpful exception is thrown. We can illustrate by comparing the following two lines of code:

long shares = ((Stock)a).SharesOwned;    // Approach #1
long shares = (a as Stock).SharesOwned;  // Approach #2
If a is not a Stock, the first line throws an `InvalidCastException`, which is an accurate description of what went wrong. The second line throws a `NullReferenceException`, which is ambiguous. Was a not a Stock, or was a null?

Another way of looking at it is that with the cast operator, you’re saying to the compiler, “I’m certain of a value’s type; if I’m wrong, there’s a bug in my code, so throw an exception!” Whereas with the as operator, you’re uncertain of its type and want to branch according to the outcome at runtime.

The as operator cannot perform custom conversions (see “Operator Overloading”), and it cannot do numeric conversions:

long x = 3 as long;    // Compile-time error
NOTE
`The as and cast operators will also perform upcasts, although this is not terribly useful because an implicit conversion will do the job.`

## The is operator
The is operator tests whether a variable matches a pattern. C# supports several kinds of patterns, the most important being a type pattern, where a type name follows the is keyword.

In this context, the is operator tests whether a reference conversion would succeed—in other words, whether an object derives from a specified class (or implements an interface). It is often used to test before downcasting:

if (a is Stock)
  Console.WriteLine (((Stock)a).SharesOwned);
The is operator also evaluates to true if an unboxing conversion would succeed (see “The object Type”). However, it does not consider custom or numeric conversions.

## Virtual Function Members
A function marked as virtual can be overridden by subclasses wanting to provide a specialized implementation. Methods, properties, indexers, and events can all be declared virtual:

public class Asset
{
  public string Name;
  public virtual decimal Liability => 0;   // Expression-bodied property
}
A subclass overrides a virtual method by applying the override modifier:

public class Stock : Asset
{
  public long SharesOwned;
}

public class House : Asset
{
  public decimal Mortgage;
  public override decimal Liability => Mortgage;
}

`Warning`: Calling virtual methods from a constructor is potentially dangerous because authors of subclasses are unlikely to know, when overriding the method, that they are working with a partially initialized object. In other words, the overriding method might end up accessing methods or properties that rely on fields not yet initialized by the constructor.

## Covariant return types
From C# 9, you can override a method (or property get accessor) such that it returns a more derived (subclassed) type. For example:

public class Asset
{
  public string Name;
  public virtual Asset Clone() => new Asset { Name = Name };
}

public class House : Asset
{
  public decimal Mortgage;
  public override House Clone() => new House
                                   { Name = Name, Mortgage = Mortgage };
}
This is permitted because it does not break the contract that Clone must return an Asset: it returns a House, which is an Asset (and more).

Prior to C# 9, you had to override methods with the identical return type:

public override Asset Clone() => new House { ... }
This still does the job, because the overridden Clone method instantiates a House rather than an Asset. However, to treat the returned object as a House, you must then perform a downcast:

House mansion1 = new House { Name="McMansion", Mortgage=250000 };
House mansion2 = (House) mansion1.Clone();

# Abstract Classes and Abstract Members
A class declared as abstract can never be instantiated. Instead, only its concrete subclasses can be instantiated.

Abstract classes are able to define abstract members. Abstract members are like virtual members except that they don’t provide a default implementation. That implementation must be provided by the subclass unless that subclass is also declared abstract:

public abstract class Asset
{
  // Note empty implementation
  public abstract decimal NetValue { get; }
}

public class Stock : Asset
{
  public long SharesOwned;
  public decimal CurrentPrice;

  // Override like a virtual method.
  public override decimal NetValue => CurrentPrice * SharesOwned;
}

## Hiding Inherited Members
A base class and a subclass can define identical members. For example:

public class A      { public int Counter = 1; }
public class B : A  { public int Counter = 2; }
The Counter field in class B is said to hide the Counter field in class A. Usually, this happens by accident, when a member is added to the base type after an identical member was added to the subtype. For this reason, the compiler generates a warning and then resolves the ambiguity as follows:

References to A (at compile time) bind to A.Counter.

References to B (at compile time) bind to B.Counter.

Occasionally, you want to hide a member deliberately, in which case you can apply the new modifier to the member in the subclass. The new modifier does nothing more than suppress the compiler warning that would otherwise result:

public class A     { public     int Counter = 1; }
public class B : A { public new int Counter = 2; }
The new modifier communicates your intent to the compiler—and other programmers—that the duplicate member is not an accident.
`Note`
C# overloads the new keyword to have independent meanings in different contexts. Specifically, the new operator is different from the new member modifier.

## new versus override
Consider the following class hierarchy:
```c#
public class BaseClass
{
  public virtual void Foo()  { Console.WriteLine ("BaseClass.Foo"); }
}

public class Overrider : BaseClass
{
  public override void Foo() { Console.WriteLine ("Overrider.Foo"); }
}

public class Hider : BaseClass
{
  public new void Foo()      { Console.WriteLine ("Hider.Foo"); }
}
```
The differences in behavior between Overrider and Hider are demonstrated in the following code:
```c#
Overrider over = new Overrider();
BaseClass b1 = over;
over.Foo();                         // Overrider.Foo
b1.Foo();                           // Overrider.Foo

Hider h = new Hider();
BaseClass b2 = h;
h.Foo();                           // Hider.Foo
b2.Foo();                          // BaseClass.Foo
```

## Sealing Functions and Classes
An overridden function member can seal its implementation with the sealed keyword to prevent it from being overridden by further subclasses. In our earlier virtual function member example, we could have sealed House’s implementation of Liability, preventing a class that derives from House from overriding Liability, as follows:

public sealed override decimal Liability { get { return Mortgage; } }
You can also apply the sealed modifier to the class itself, to prevent subclassing. Sealing a class is more common than sealing a function member.

Although you can seal a function member against overriding, you can’t seal a member against being hidden.

The sealed modifier in C# is used to prevent further inheritance of a class. Once a class is marked as sealed, it cannot be used as a base class for any other class.

The sealed modifier can be used when a class is not designed to be inherited from, and any further extension of the class would break its functionality or violate its design principles. It can also be used to provide better performance, as it allows the compiler to optimize certain method calls and code paths at compile time, instead of at runtime.

However, it is important to note that sealing a class should be done with care, as it limits the flexibility of the codebase and can make it harder to maintain and extend in the future. It is generally recommended to only use the sealed modifier when it is necessary to do so.

1. When you want to prevent inheritance: If you have a class that you don't want other classes to inherit from, you can mark it as sealed. This prevents other classes from subclassing it.
csharp
Copy code
public sealed class MyFinalClass
{
    // Class members...
}
2. When you want to prevent method overriding: If you have a class with a method that you don't want subclasses to override, you can mark it as sealed. This prevents subclasses from changing the behavior of the method.
csharp
Copy code
public class MyBaseClass
{
    public virtual void MyMethod()
    {
        // Method implementation...
    }
}

public class MyDerivedClass : MyBaseClass
{
    public sealed override void MyMethod()
    {
        // Method implementation...
    }
}
```c#
using System;

class BaseClass
{
    public virtual void Foo() { Console.WriteLine("BaseClass.Foo()"); }
}

class DerivedClass : BaseClass
{
    public override sealed void Foo() { Console.WriteLine("DerivedClass.Foo()"); }
}

class Subclass : DerivedClass
{
    // This line will result in a compile-time error, since the `Foo` method is sealed:
    // public override void Foo() { Console.WriteLine("Subclass.Foo()"); }
}

class Program
{
    static void Main(string[] args)
    {
        BaseClass bc = new BaseClass();
        DerivedClass dc = new DerivedClass();
        Subclass sc = new Subclass();

        bc.Foo();   // Output: BaseClass.Foo()
        dc.Foo();   // Output: DerivedClass.Foo()
        sc.Foo();   // Output: DerivedClass.Foo()  (since Subclass does not override Foo, it inherits the sealed version from DerivedClass)
    }
}

```
3. When you want to improve performance: If you have a performance-critical class or method, you can mark it as sealed to prevent virtual method calls. This can help the runtime to optimize the code and improve performance.
csharp
Copy code
public sealed class MyFinalClass
{
    public int MyMethod()
    {
        // Method implementation...
    }
}
Note that in general, it's a good idea to avoid using sealed unless you have a specific reason to do so. Overusing sealed can limit the flexibility and extensibility of your code, and can make it harder to maintain and refactor in the future.

# The base Keyword
The base keyword is similar to the this keyword. It serves two essential purposes:

Accessing an overridden function member from the subclass

Calling a base-class constructor (see the next section)

In this example, House uses the base keyword to access Asset’s implementation of Liability:

public class House : Asset
{
  ...
  public override decimal Liability => base.Liability + Mortgage;
}
With the base keyword, we access Asset’s Liability property nonvirtually. This means that we will always access Asset’s version of this property—regardless of the instance’s actual runtime type.

The same approach works if Liability is hidden rather than overridden. (You can also access hidden members by casting to the base class before invoking the function.)

# Constructors and Inheritance
A subclass must declare its own constructors. The base class’s constructors are accessible to the derived class but are never automatically inherited. For example, if we define Baseclass and Subclass as follows:

public class Baseclass
{
  public int X;
  public Baseclass () { }
  public Baseclass (int x) { this.X = x; }
}

public class Subclass : Baseclass { }
the following is illegal:

Subclass s = new Subclass (123);
Subclass must hence “redefine” any constructors it wants to expose. In doing so, however, it can call any of the base class’s constructors via the base keyword:

public class Subclass : Baseclass
{
  public Subclass (int x) : base (x) { }
}
The base keyword works rather like the this keyword except that it calls a constructor in the base class.

Base-class constructors always execute first; this ensures that base initialization occurs before specialized initialization.

# Implicit calling of the parameterless base-class constructor
If a constructor in a subclass omits the base keyword, the base type’s parameterless constructor is implicitly called:

public class BaseClass
{
  public int X;
  public BaseClass() { X = 1; }
}

public class Subclass : BaseClass
{
  public Subclass() { Console.WriteLine (X); }  // 1
}
If the base class has no accessible parameterless constructor, subclasses are forced to use the base keyword in their constructors.

# Constructor and field initialization order
When an object is instantiated, initialization takes place in the following order:

From subclass to base class:

Fields are initialized.

Arguments to base-class constructor calls are evaluated.

From base class to subclass:

Constructor bodies execute.

For example:
```c#
public class B
{
  int x = 1;         // Executes 3rd
  public B (int x)
  {
    ...              // Executes 4th
  }
}
public class D : B
{
  int y = 1;         // Executes 1st
  public D (int x)
    : base (x + 1)   // Executes 2nd
  {
     ...             // Executes 5th
  }
}
```

# Overloading and Resolution
Inheritance has an interesting impact on method overloading. Consider the following two overloads:

static void Foo (Asset a) { }
static void Foo (House h) { }
When an overload is called, the most specific type has precedence:

House h = new House (...);
Foo(h);                      // Calls Foo(House)
The particular overload to call is determined statically (at compile time) rather than at runtime. The following code calls Foo(Asset), even though the runtime type of a is House:

Asset a = new House (...);
Foo(a);                      // Calls Foo(Asset)

`Note`:
If you cast Asset to dynamic (Chapter 4), the decision as to which overload to call is deferred until runtime and is then based on the object’s actual type:

Asset a = new House (...);
Foo ((dynamic)a);   // Calls Foo(House)