# Access Modifiers
To promote encapsulation, a type or type member can limit its accessibility to other types and other assemblies by adding one of five access modifiers to the declaration:

`public`
Fully accessible. This is the implicit accessibility for members of an enum or interface.
`internal`
Accessible only within the containing assembly or friend assemblies. This is the default accessibility for non-nested types.
`private`
Accessible only within the containing type. This is the default accessibility for members of a class or struct.
`protected`
Accessible only within the containing type or subclasses.
`protected internal`
The union of protected and internal accessibility. A member that is protected internal is accessible in two ways.
means that a member is accessible from within the same assembly or from within a derived class in any assembly.
`private protected` (from C# 7.2)
The intersection of protected and internal accessibility. A member that is private protected is accessible only within the containing type, or subclasses that reside in the same assembly (making it less accessible than protected or internal alone).

# Example
Examples
Class2 is accessible from outside its assembly; Class1 is not:

class Class1 {}                  // Class1 is internal (default)
public class Class2 {}
ClassB exposes field x to other types in the same assembly; ClassA does not:

class ClassA { int x;          } // x is private (default)
class ClassB { internal int x; }
Functions within Subclass can call Bar but not Foo:

class BaseClass
{
  void Foo()           {}        // Foo is private (default)
  protected void Bar() {}
}


class Subclass : BaseClass
{
  void Test1() { Foo(); }       // Error - cannot access Foo
  void Test2() { Bar(); }       // OK
}

# Accessibility Capping
A type caps the accessibility of its declared members. The most common example of capping is when you have an internal type with public members. For example, consider this:

class C { public void Foo() {} }
C’s (default) internal accessibility caps Foo’s accessibility, effectively making Foo internal. A common reason Foo would be marked public is to make for easier refactoring should C later be changed to public.

# Restrictions on Access Modifiers
When overriding a base class function, accessibility must be identical on the overridden function; for example:

class BaseClass             { protected virtual  void Foo() {} }
class Subclass1 : BaseClass { protected override void Foo() {} }  // OK
class Subclass2 : BaseClass { public    override void Foo() {} }  // Error
(An exception is when overriding a protected internal method in another assembly, in which case the override must simply be protected.)

The compiler prevents any inconsistent use of access modifiers. For example, a subclass itself can be less accessible than a base class, but not more:

internal class A {}
public class B : A {}          // Error