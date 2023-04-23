# What is IDisposable Interface ?
```c#
class Resource: IDisposable{
  public Resource(){
    // Do something
  }
  public void Dispose(){
    // Dispose resource
  }
}
```
# Using Declarations 
We use the using statement for declaring disposable variables such as File I/O, databases, web services, etc. It ensures that classes that implement the IDisposable interface call their Dispose method. The only problem is that adding a using statement to our code introduces a new scope block

`using declarations` eliminate this problem. It also `guarantees that the Dispose method will be called, even if the code throws an exception.`

By using the “using” keyword we can declare a variable that tells the compiler that the variable is declared should be disposed of at the end of the enclosing scope.

# Using Statement (Old Way) 
```c#
public class UsingDeclarations
    {
        public static void Main()
        {
            using (var resource = new Resource())
            {
                resource.ResourceUsing();
            } // resource.Dispose is called here automatically
            Console.WriteLine("Doing Some Other Task...");
        }
    }
    class Resource : IDisposable
    {
        public Resource()
        {
            Console.WriteLine("Resource Created...");
        }
        public void ResourceUsing()
        {
            Console.WriteLine("Resource Using...");
        }
        public void Dispose()
        {
            //Dispose resource
            Console.WriteLine("Resource Disposed...");
        }
    }
// OUTPUT
Resource Created...
Resource Using...
Resource Disposed...
Doing Some Other Task...
```

# How is the Dispose Method automatically Called?
When we use the `using` statement in C#, behind the scenes, the compiler will create a code block using `try/finally to make sure Dispose method` is also called even though an exception is thrown. This is because finally block gives you a guarantee to be executed irrespective of the exception thrown in the try block.

```c#
using (var resource = new Resource())
{
    resource.ResourceUsing();
} // resource.Dispose is called here automatically
```
Behind the scene
```c#
var  resource = new Resource();
try{
  resource.ResourceUsing();
}finally{
  resource.Dispose();
}
```

# Using Declarations (New Way)
Now, the curly brackets are no longer required. At the end of the scope of the method (which is here the end of the main method), the Dispose method is also called automatically. 
```c#
using var resource = new Resource();
resource.ResourceUsing();
Console.WriteLine("Doing Some Other Task...");
```
Disposing multiple resources
```c#
public static void Main()
{
    using (var resource1 = new Resource())
    {
        using (var resource2 = new Resource())
        {
            resource1.ResourceUsing();
            resource2.ResourceUsing();
        }
    }
    Console.WriteLine("Main Method End...");
}
// OUTPUT
Resource 1 Created...
Resource 2 Created...
Resource 1 Using...
Resource 2 Using...
Resource 2 Disposed...
Resource 1 Disposed...
Main Method End...
```
Do the same with the new using declarations
```c#
 public static void Main()
{
    using var resource1 = new Resource();
    using var resource2 = new Resource();
    resource1.ResourceUsing();
    resource2.ResourceUsing();
    Console.WriteLine("Main Method End...");
}
```
The using locals will then be disposed in the reverse order in which they are declared.
```c#
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```
# How to dispose of a resource before the method ends in C# using declarations?
In that case, we just need to add a `separate scope using curly brackets`. When the variable is out of scope, the resource is disposed of. 
```c#
 public static void Main()
  {
    {
        using var resource1 = new Resource();
        resource1.ResourceUsing();
    }//resource1.Dispose() called here
    Console.WriteLine("Main Method End...");
}
```
# Realtime Examples
```c#
public static void WriteToFileUsingDeclaration()
{
    List<string> Statements = new List<string>()
    {
        "First Statement",
        "Second Statement",
        "Third Statement."
    };
    using var file = new StreamWriter("MyTestFile.txt");
    foreach (string Statement in Statements)
    {
        file.WriteLine(Statement);
    }
}// file is disposed here
```
If you go to the definition of `StreamWriter` class, then somewhere you will find that this class implements the Dispose method of the IDisposable interface. Further notice, this class implements the TextWriter abstract class and the TextWriter abstract class implements the IDisposable interface.

# Restrictions around using declaration:
- May not appear directly inside a case label but instead must be within a block inside the case label.
- May not appear as part of an out variable declaration.
- Must have an initializer for each declarator.
- The local type must be implicitly convertible to IDisposable or fulfill the using pattern.

# REF
- https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-8.0/using
- https://dotnettutorials.net/lesson/using-declarations-csharp-8/