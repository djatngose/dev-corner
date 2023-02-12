# Variables and Parameters
A variable represents a storage location that has a modifiable value. A variable can be a local variable, parameter (value, ref, out, or in), field (instance or static), or array element.


# The Stack and the Heap
The stack and the heap are the places where variables reside. Each has very different lifetime semantics.

# Stack
The stack is a block of memory for storing `local variables and parameters`. The stack logically grows and shrinks as a method or function is entered and exited. Consider the following method (to avoid distraction, input argument checking is ignored):

```c#
static int Factorial (int x)
{
  if (x == 0) return 1;
  return x * Factorial (x-1);
}
```
This method is recursive, meaning that it calls itself. Each time the method is entered, a new int is allocated on the stack, and each time the method exits, the int is deallocated.

# Heap
The heap is the memory in which objects (i.e., reference-type instances) reside. Whenever a new object is created, it is allocated on the heap, and a reference to that object is returned. During a program’s execution, the heap begins filling up as new objects are created. The runtime has a garbage collector that periodically deallocates objects from the heap, so your program does not run out of memory. `An object is eligible for deallocation as soon as it’s not referenced by anything that’s itself “alive.”`

In the following example, we begin by creating a StringBuilder object referenced by the variable ref1 and then write out its content. That StringBuilder object is then immediately eligible for garbage collection because nothing subsequently uses it.

Then, we create another StringBuilder referenced by variable ref2 and copy that reference to ref3. Even though ref2 is not used after that point, ref3 keeps the same StringBuilder object alive—ensuring that it doesn’t become eligible for collection until we’ve finished using ref3:

```c#
using System;
using System.Text;

StringBuilder ref1 = new StringBuilder ("object1");
Console.WriteLine (ref1);
// The StringBuilder referenced by ref1 is now eligible for GC.

StringBuilder ref2 = new StringBuilder ("object2");
StringBuilder ref3 = ref2;
// The StringBuilder referenced by ref2 is NOT yet eligible for GC.

Console.WriteLine (ref3);      // object2
```

Value-type instances (and object references) live wherever the variable was declared. If the instance was declared as a field within a class type, or as an array element, that instance lives on the heap.

`The heap also stores static fields`. Unlike objects allocated on the heap (which can be garbage-collected), `these live until the process ends.`