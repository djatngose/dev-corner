
# MinBy and MaxBy
MinBy and MaxBy (introduced in .NET 6) return the element with the smallest or largest value, as determined by a keySelector:
```c#
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
Console.WriteLine (names.MaxBy (n => n.Length));   // Harry
```
In contrast, Min and Max (which we will cover in the following section) return the smallest or largest value itself:
```c#
Console.WriteLine (names.Max   (n => n.Length));   // 5
```
If two or more elements share a minimum/maximum value, MinBy/MaxBy returns the first:
```c#
Console.WriteLine (names.MinBy (n => n.Length));   // Tom
```
If the input sequence is empty, MinBy and MaxBy return null if the element type is nullable (or throw an exception if the element type is not nullable).