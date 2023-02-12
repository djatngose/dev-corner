# Identifiers and Keywords
C# identifiers are case sensitive. By convention, parameters, local variables, and private fields should be in camel case (e.g., myVariable), and all other identifiers should be in Pascal case (e.g., MyMethod).

Most keywords are reserved, which means that you can’t use them as identifiers. Here is the full list of C# reserved keywords:
```c#
abstract
as
base
bool
break
byte
case
catch
char
checked
class
const
continue
decimal
default
delegate	do
double
else
enum
event
explicit
extern
false
finally
fixed
float
for
foreach
goto
if
implicit	in
int
interface
internal
is
lock
long
namespace
new
null
object
operator
out
override
params
private	protected
public
readonly
record
ref
return
sbyte
sealed
short
sizeof
stackalloc
static
string
struct
switch
this	throw
true
try
typeof
uint
ulong
unchecked
unsafe
ushort
using
virtual
void
volatile
while
```

If you really want to use an identifier that clashes with a reserved keyword, you can do so by qualifying it with the `@` prefix; for instance:

```c#
int using = 123;      // Illegal
int @using = 123;     // Legal
```

