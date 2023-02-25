
# var—Implicitly Typed Local Variables

It is often the case that you declare and initialize a variable in one step. If the compiler is able to infer the type from the initialization expression, you can use the keyword var in place of the type declaration; for example:

var x = "hello";
var y = new System.Text.StringBuilder();
var z = (float)Math.PI;
This is precisely equivalent to the following:

string x = "hello";
System.Text.StringBuilder y = new System.Text.StringBuilder();
float z = (float)Math.PI;

Because of this direct equivalence, implicitly typed variables are statically typed. For example, the following generates a compile-time error:

var x = 5;
x = "hello";    // Compile-time error; x is of type int

Note: var can decrease code readability when you can’t deduce the type purely by looking at the variable declaration; for example:

Random r = new Random();
var x = r.Next();
What type is x?

# Target-Typed new Expressions
Another way to reduce lexical repetition is with target-typed new expressions (from C# 9):

System.Text.StringBuilder sb1 = new();
System.Text.StringBuilder sb2 = new ("Test");
This is precisely equivalent to:

System.Text.StringBuilder sb1 = new System.Text.StringBuilder();
System.Text.StringBuilder sb2 = new System.Text.StringBuilder ("Test");
The principle is that you can call new without specifying a type name if the compiler is able to unambiguously infer it. Target-typed new expressions are particularly useful when the variable declaration and initialization are in different parts of your code. A common example is when you want to initialize a field in a constructor:

class Foo
{
  System.Text.StringBuilder sb;
  
  public Foo (string initialValue)
  {
    sb = new (initialValue);
  }
}
Target-typed new expressions are also helpful in the following scenario:

MyMethod (new ("test"));

void MyMethod (System.Text.StringBuilder sb) { ... }