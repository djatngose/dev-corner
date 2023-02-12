# The ICollection and IList Interfaces
- Although the enumeration interfaces provide a protocol for forward-only iteration over a collection, they don’t provide a mechanism to determine `the size of the collection, access a member by index, search, or modify the collection. `
- For such functionality, .NET defines the ICollection, IList, and IDictionary interfaces. Each comes in both generic and nongeneric versions; however, the nongeneric versions exist mostly for legacy support.
# Hirachy

- `IEnumerable<T> (and IEnumerable)`
  - Provides minimum functionality (enumeration only)
- `ICollection<T> (and ICollection)`
  - Provides medium functionality (e.g., the Count property)
- `IList<T>/IDictionary<K,V> and their nongeneric versions`
  - Provide maximum functionality (including “random” access by index/key)

# ICollection<T> and ICollection
ICollection<T> is the standard interface for countable collections of objects. It provides the ability to determine the size of a collection (Count), determine whether an item exists in the collection (Contains), copy the collection into an array (ToArray), and determine whether the collection is read-only (IsReadOnly). For writable collections, you can also Add, Remove, and Clear items from the collection. And because it extends IEnumerable<T>, it can also be traversed via the foreach statement:

```c#
public interface ICollection<T> : IEnumerable<T>, IEnumerable
{
  int Count { get; }

  bool Contains (T item);
  void CopyTo (T[] array, int arrayIndex);
  bool IsReadOnly { get; }

  void Add(T item);
  bool Remove (T item);
  void Clear();
}
```
- `The nongeneric ICollection` is similar in providing a countable collection, but it doesn’t provide functionality for altering the list or checking for element membership:
```c#
public interface ICollection : IEnumerable
{
   int Count { get; }
   bool IsSynchronized { get; }
   object SyncRoot { get; }
   void CopyTo (Array array, int index);
}
```
The nongeneric interface also defines properties to assist with synchronization (Chapter 14)—these were dumped in the generic version because thread safety is no longer considered intrinsic to the collection.

Both interfaces are fairly straightforward to implement. If implementing a read-only ICollection<T>, the Add, Remove, and Clear methods should throw a NotSupported​Exception.

These interfaces are usually implemented in conjunction with either the IList or the IDictionary interface.


# IList<T> and IList
IList<T> is the standard interface for collections indexable by position. In addition to the functionality inherited from ICollection<T> and IEnumerable<T>, it provides the ability to read or write an element by position (via an indexer) and insert/remove by position
```c#
public interface IList<T> : ICollection<T>, IEnumerable<T>, IEnumerable
{
  T this [int index] { get; set; }
  int IndexOf (T item);
  void Insert (int index, T item);
  void RemoveAt (int index);
}
```
The IndexOf methods perform a linear search on the list, returning −1 if the specified item is not found.

The nongeneric version of IList has more members because it inherits less from ICollection:
```c#
public interface IList : ICollection, IEnumerable
{
  object this [int index] { get; set }
  bool IsFixedSize { get; }
  bool IsReadOnly  { get; }
  int  Add      (object value);
  void Clear();
  bool Contains (object value);
  int  IndexOf  (object value);
  void Insert   (int index, object value);
  void Remove   (object value);
  void RemoveAt (int index);
}
```
The Add method on the nongeneric IList interface returns an integer—this is the index of the newly added item. In contrast, the Add method on ICollection<T> has a void return type.

The general-purpose List<T> class is the quintessential implementation of both IList<T> and IList. C# arrays also implement both the generic and nongeneric ILists (although the methods that add or remove elements are hidden via explicit interface implementation and throw a NotSupportedException if called).

# Multidimensional array error
An ArgumentException is thrown if you try to access a multidimensional array via IList’s indexer. This is a trap when writing methods such as the following:
```c#
public object FirstOrNull (IList list)
{
  if (list == null || list.Count == 0) return null;
  return list[0];
}
```
This might appear bulletproof, but it will throw an exception if called with a multidimensional array. You can test for a multidimensional array at runtime with this expression (more on this in Chapter 19):
```c#
list.GetType().IsArray && list.GetType().GetArrayRank()>1
```
# IReadOnlyCollection<T> and IReadOnlyList<T>
```c#
public interface IReadOnlyCollection<out T> : IEnumerable<T>, IEnumerable
{
  int Count { get; }
}

public interface IReadOnlyList<out T> : IReadOnlyCollection<T>,
                                        IEnumerable<T>, IEnumerable
{
  T this[int index] { get; }
}
```
Because the type parameter for these interfaces is used only in output positions, it’s marked as covariant. This allows a list of cats, for instance, to be treated as a read-only list of animals. In contrast, T is not marked as covariant with ICollection<T> and IList<T>, because T is used in both input and output positions.

# List<T> and ArrayList
- `ArrayList implements IList, whereas List<T> implements both IList and IList<T> (and the read-only version, IReadOnlyList<T>)`. Unlike with arrays, all interfaces are implemented publicly, and methods such as Add and Remove are exposed and work as you would expect.
- Internally, List<T> and ArrayList work by maintaining an internal array of objects, replaced with a larger array upon reaching capacity. Appending elements is efficient (because there is usually a free slot at the end), but inserting elements can be slow (because all elements after the insertion point must be shifted to make a free slot), as can removing elements (especially near the start). As with arrays, searching is efficient if the BinarySearch method is used on a list that has been sorted, but it is otherwise inefficient because each item must be individually checked.
- `List<T> is up to several times faster than ArrayList if T is a value type, because List<T> avoids the overhead of boxing and unboxing elements.`
- List<T> and ArrayList provide constructors that accept an existing collection of elements: these copy each element from the existing collection into the new List<T> or ArrayList:
```c#
  public class ArrayList : IList, ICollection, IEnumerable, ICloneable
  {
    Count;
    bool IsReadOnly;
    int Add();
    void AddRange()
    int BinarySearch();
    Clear
    Contains
    CopyTo
    IndexOf
    Insert
  }
```
```c#
public class List<T> : IList<T>, IReadOnlyList<T>
{
  public List ();
  public List (IEnumerable<T> collection);
  public List (int capacity);

  // Add+Insert
  public void Add         (T item);
  public void AddRange    (IEnumerable<T> collection);
  public void Insert      (int index, T item);
  public void InsertRange (int index, IEnumerable<T> collection);

  // Remove
  public bool Remove      (T item);
  public void RemoveAt    (int index);
  public void RemoveRange (int index, int count);
  public int  RemoveAll   (Predicate<T> match);

  // Indexing
  public T this [int index] { get; set; }
  public List<T> GetRange (int index, int count);
  public Enumerator<T> GetEnumerator();

  // Exporting, copying and converting:
  public T[] ToArray();
  public void CopyTo (T[] array);
  public void CopyTo (T[] array, int arrayIndex);
  public void CopyTo (int index, T[] array, int arrayIndex, int count);
  public ReadOnlyCollection<T> AsReadOnly();
  public List<TOutput> ConvertAll<TOutput> (Converter <T,TOutput>
                                            converter);
  // Other:
  public void Reverse();            // Reverses order of elements in list.
  public int Capacity { get;set; }  // Forces expansion of internal array.
  public void TrimExcess();         // Trims internal array back to size.
  public void Clear();              // Removes all elements, so Count=0.
}

public delegate TOutput Converter <TInput, TOutput> (TInput input);
```

- The nongeneric ArrayList class requires clumsy casts—as the following example demonstrates:

```c#
var words = new List<string>();    // New string-typed list

words.Add ("melon");
words.Add ("avocado");
words.AddRange (new[] { "banana", "plum" } );
words.Insert (0, "lemon");                           // Insert at start
words.InsertRange (0, new[] { "peach", "nashi" });   // Insert at start

words.Remove ("melon");
words.RemoveAt (3);                         // Remove the 4th element
words.RemoveRange (0, 2);                   // Remove first 2 elements

// Remove all strings starting in 'n':
words.RemoveAll (s => s.StartsWith ("n"));

Console.WriteLine (words [0]);                          // first word
Console.WriteLine (words [words.Count - 1]);            // last word
foreach (string s in words) Console.WriteLine (s);      // all words
List<string> subset = words.GetRange (1, 2);            // 2nd->3rd words

string[] wordsArray = words.ToArray();    // Creates a new typed array

// Copy first two elements to the end of an existing array:
string[] existing = new string [1000];
words.CopyTo (0, existing, 998, 2);

List<string> upperCaseWords = words.ConvertAll (s => s.ToUpper());
List<int> lengths = words.ConvertAll (s => s.Length);
```
```c#
ArrayList al = new ArrayList();
al.Add ("hello");
string first = (string) al [0];
string[] strArr = (string[]) al.ToArray (typeof (string));
```
- Such casts cannot be verified by the compiler; the following compiles successfully but then fails at runtime:
```c#
int first = (int) al [0];    // Runtime exception
```
- An ArrayList is functionally similar to List<object>. Both are useful when you need a list of mixed-type elements that share no common base type (other than object). `A possible advantage of choosing an ArrayList, in this case, would be if you need to deal with the list using reflection (Chapter 19). Reflection is easier with a nongeneric ArrayList than a List<object>.`


# REF
https://learning.oreilly.com/library/view/c-10-in/9781098121945/ch07.html#icollectionless_thantgreater_than_and_i