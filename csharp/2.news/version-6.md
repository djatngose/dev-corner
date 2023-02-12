# Index initializers (Chapter 4) allow single-step initialization of any type that exposes an indexer:

var dict = new Dictionary<int,string>()
{
  [3] = "three",
  [10] = "ten"
};
String interpolation (see “String Type”) offers a succinct alternative to string.Format:

string s = $"It is {DateTime.Now.DayOfWeek} today";
# Exception filters (see “try Statements and Exceptions”) let you apply a condition to a catch block:

string html;
try
{
  html = await new HttpClient().GetStringAsync ("http://asef");
}
catch (WebException ex) when (ex.Status == WebExceptionStatus.Timeout)
{
  ...
}
# The using static (see “Namespaces”) directive lets you import all the static members of a type so that you can use those members unqualified:

using static System.Console;
...
WriteLine ("Hello, world");  // WriteLine instead of Console.WriteLine
The nameof (Chapter 3) operator returns the name of a variable, type, or other symbol as a string. This avoids breaking code when you rename a symbol in Visual Studio:

int capacity = 123;
string x = nameof (capacity);   // x is "capacity"
string y = nameof (Uri.Host);   // y is "Host"
And finally, you’re now allowed to await inside catch and finally blocks.