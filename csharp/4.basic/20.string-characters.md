# Strings and Characters
C#’s char type (aliasing the System.Char type) represents a Unicode character and occupies `2 bytes` (UTF-16). A char literal is specified within single quotes:
```
char c = 'A';       // Simple character
```
Escape sequences express characters that cannot be expressed or interpreted literally. An escape sequence is a backslash followed by a character with a special meaning; for example:
```
char newLine = '\n';
char backSlash = '\\';
```

```
Char	Meaning	Value
\'	Single quote	0x0027
\"	Double quote	0x0022
\\	Backslash	0x005C
\0	Null	0x0000
\a	Alert	0x0007
\b	Backspace	0x0008
\f	Form feed	0x000C
\n	New line	0x000A
\r	Carriage return	0x000D
\t	Horizontal tab	0x0009
\v	Vertical tab	0x000B
```

The \u (or \x) escape sequence lets you specify any Unicode character via its four-digit hexadecimal code:

```
char copyrightSymbol = '\u00A9';
char omegaSymbol     = '\u03A9';
char newLine         = '\u000A';
```

# String Type
represents an immutable (unmodifiable) sequence of Unicode characters. A string literal is specified within double quotes:
string is a reference type rather than a value type. Its equality operators, however, follow value-type semantics:
```c#
string a = "test";
string b = "test";
Console.Write (a == b);  // True
```

The escape sequences that are valid for char literals also work inside strings:

string a = "Here's a tab:\t";

The cost of this is that whenever you need a literal backslash, you must write it twice:
```c#
string a1 = "\\\\server\\fileshare\\helloworld.cs";
````
To avoid this problem, C# allows verbatim string literals. A verbatim string literal is prefixed with @ and does not support escape sequences. The following verbatim string is identical to the preceding one:
```c#
string a2 = @"\\server\fileshare\helloworld.cs";
```

A verbatim string literal can also span multiple lines:

```c#
string escaped  = "First Line\r\nSecond Line";
string verbatim = @"First Line
Second Line";

// True if your text editor uses CR-LF line separators:
Console.WriteLine (escaped == verbatim);
```

You can include the double-quote character in a verbatim literal by writing it twice:
```c#
string xml = @"<customer id=""123""></customer>";

```

# String concatenation
The + operator concatenates two strings:

string s = "a" + "b";
One of the operands might be a nonstring value, in which case ToString is called on that value:

string s = "a" + 5;  // a5
Using the + operator repeatedly to build up a string is inefficient: a better solution is to use the System.Text.`StringBuilder` type

# String interpolation
A string preceded with the $ character is called an interpolated string. Interpolated strings can include expressions enclosed in braces:

int x = 4;
Console.Write ($"A square has {x} sides");  // Prints: A square has 4 sides

Any valid C# expression of any type can appear within the braces, and C# will convert the expression to a string by calling its ToString method or equivalent. 
```c#
string s = $"255 in hex is {byte.MaxValue:X2}";  // X2 = 2-digit hexadecimal
// Evaluates to "255 in hex is FF"
```