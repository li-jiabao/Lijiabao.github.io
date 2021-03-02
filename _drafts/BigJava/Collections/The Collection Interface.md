
# The Collection Interface

A 
[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html) represents a group of objects known as its elements. The `Collection` interface is used to pass around collections of objects where maximum generality is desired. For example, by convention all general-purpose collection implementations have a constructor that takes a `Collection` argument. This constructor, known as a *conversion constructor*, initializes the new collection to contain all of the elements in the specified collection, whatever the given collection's subinterface or implementation type. In other words, it allows you to *convert* the collection's type.

Suppose, for example, that you have a `Collection&lt;String&gt; c`, which may be a `List`, a `Set`, or another kind of `Collection`. This idiom creates a new `ArrayList` (an implementation of the `List` interface), initially containing all the elements in `c`.

```

List&lt;String&gt; list = new ArrayList&lt;String&gt;(c);

```

```

List&lt;String&gt; list = new ArrayList&lt;&gt;(c);

```

The following shows the `Collection` interface.

```

public interface Collection&lt;E&gt; extends Iterable&lt;E&gt; {
    // Basic operations
    int size();
    boolean isEmpty();
    boolean contains(Object element);
    // optional
    boolean add(E element);
    // optional
    boolean remove(Object element);
    Iterator&lt;E&gt; iterator();

    // Bulk operations
    boolean containsAll(Collection&lt;?&gt; c);
    // optional
    boolean addAll(Collection&lt;? extends E&gt; c); 
    // optional
    boolean removeAll(Collection&lt;?&gt; c);
    // optional
    boolean retainAll(Collection&lt;?&gt; c);
    // optional
    void clear();

    // Array operations
    Object[] toArray();
    &lt;T&gt; T[] toArray(T[] a);
}

```


The `Collection` interface contains methods that perform basic operations, such as `int size()`, `boolean isEmpty()`, 
`boolean contains(Object element)`, `boolean add(E element)`, `boolean remove(Object element)`, and 
`Iterator&lt;E&gt; iterator()`.



 It also contains
methods that operate on entire collections, such as `boolean containsAll(Collection&lt;?&gt; c)`, 
`boolean addAll(Collection&lt;? extends E&gt; c)`, 
 `boolean removeAll(Collection&lt;?&gt; c)`, `boolean retainAll(Collection&lt;?&gt; c)`, and  
`void clear()`.



 Additional methods for array operations
(such as `Object[] toArray()` and `&lt;T&gt; T[] toArray(T[] a)` exist as well.



In JDK 8 and later, the `Collection` interface also exposes methods `Stream&lt;E&gt; stream()` and `Stream&lt;E&gt; parallelStream()`, for obtaining sequential or parallel streams from the underlying collection. 

(See the lesson entitled
[Aggregate Operations](../../collections/streams/index.html) for more information about using streams.)


The `Collection` interface does about what you'd expect given that a `Collection` represents a group of objects. It has methods that tell you how many elements are in the collection (`size`, `isEmpty`), methods that check whether a given object is in the collection (`contains`), methods that add and remove an element from the collection (`add`, `remove`), and methods that provide an iterator over the collection (`iterator`).

The `add` method is defined generally enough so that it makes sense for collections that allow duplicates as well as those that don't. It guarantees that the `Collection` will contain the specified element after the call completes, and returns `true` if the `Collection` changes as a result of the call. Similarly, the `remove` method is designed to remove a single instance of the specified element from the `Collection`, assuming that it contains the element to start with, and to return `true` if the `Collection` was modified as a result. <a name="Traversing" id="Traversing"></a>

## Traversing Collections

There are three ways to traverse collections: (1) using aggregate operations (2) with the `for-each` construct and (3) by using `Iterator`s. <a name="ForEach" id="ForEach"></a>

### Aggregate Operations


In JDK 8 and later, the preferred method of iterating over a collection is to obtain a stream 
and perform aggregate operations on it.
Aggregate operations are often used in conjunction with lambda expressions
to make programming more expressive, using less lines of code. The following code sequentially iterates through a collection of shapes and prints 
out the red objects:


```

myShapesCollection.stream()
.filter(e -&gt; e.getColor() == Color.RED)
.forEach(e -&gt; System.out.println(e.getName()));

```


Likewise, you could easily request a parallel stream, which might make sense 
if the collection is large enough and your computer has enough cores:


```

myShapesCollection.parallelStream()
.filter(e -&gt; e.getColor() == Color.RED)
.forEach(e -&gt; System.out.println(e.getName()));

```


There are many different ways to collect data with this API. For example,  you might want to convert 
the elements of a `Collection` to `String` objects, then join them, separated by commas:


```

    String joined = elements.stream()
    .map(Object::toString)
    .collect(Collectors.joining(", "));

```


Or perhaps sum the salaries of all employees:


```

int total = employees.stream()
.collect(Collectors.summingInt(Employee::getSalary)));

```


These are but a few examples of what you can do with streams and 
aggregate operations.
For more information and examples, see the lesson entitled
[Aggregate Operations](../../collections/streams/index.html).



The Collections framework has always provided a number of so-called "bulk operations" as part of its API. 
These include methods that operate on entire collections, such as `containsAll`, `addAll`, `removeAll`, etc. 
Do not confuse those methods with the aggregate operations that were introduced in JDK 8.
The key difference between the new aggregate operations and the existing bulk operations
 (`containsAll`, `addAll`, etc.) is that the old versions are all **mutative**, meaning that
  they all modify the underlying collection. In contrast, the new aggregate operations **do not**
modify the underlying collection. When using the new aggregate operations and lambda expressions, you must
take care to avoid mutation so as not to introduce problems in the future, should your code be run later from a parallel stream.






### for-each Construct

The `for-each` construct allows you to concisely traverse a collection or array using a `for` loop &#151; see 
[The for Statement](../../java/nutsandbolts/for.html). The following code uses the `for-each` construct to print out each element of a collection on a separate line.

```

for (Object o : collection)
    System.out.println(o);

```

### <a name="Iterator" id="Iterator">Iterators</a>

An 
[`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html) is an object that enables you to traverse through a collection and to remove elements from the collection selectively, if desired. You get an `Iterator` for a collection by calling its `iterator` method. The following is the `Iterator` interface.

```

public interface Iterator&lt;E&gt; {
    boolean hasNext();
    E next();
    void remove(); //optional
}

```

The `hasNext` method returns `true` if the iteration has more elements, and the `next` method returns the next element in the iteration. The `remove` method removes the last element that was returned by `next` from the underlying `Collection`. The `remove` method may be called only once per call to `next` and throws an exception if this rule is violated.

Note that `Iterator.remove` is the *only* safe way to modify a collection during iteration; the behavior is unspecified if the underlying collection is modified in any other way while the iteration is in progress.

Use `Iterator` instead of the `for-each` construct when you need to:

- Remove the current element. The `for-each` construct hides the iterator, so you cannot call `remove`. Therefore, the `for-each` construct is not usable for filtering.
- Iterate over multiple collections in parallel.

The following method shows you how to use an `Iterator` to filter an arbitrary `Collection` &#151; that is, traverse the collection removing specific elements.

```

static void filter(Collection&lt;?&gt; c) {
    for (Iterator&lt;?&gt; it = c.iterator(); it.hasNext(); )
        if (!cond(it.next()))
            it.remove();
}

```

This simple piece of code is polymorphic, which means that it works for *any* `Collection` regardless of implementation. This example demonstrates how easy it is to write a polymorphic algorithm using the Java Collections Framework.

## Collection Interface Bulk Operations

*Bulk operations* perform an operation on an entire `Collection`. You could implement these shorthand operations using the basic operations, though in most cases such implementations would be less efficient. The following are the bulk operations:

- `containsAll` &#151; returns `true` if the target `Collection` contains all of the elements in the specified `Collection`.
- `addAll` &#151; adds all of the elements in the specified `Collection` to the target `Collection`.
- `removeAll` &#151; removes from the target `Collection` all of its elements that are also contained in the specified `Collection`.
- `retainAll` &#151; removes from the target `Collection` all its elements that are *not* also contained in the specified `Collection`. That is, it retains only those elements in the target `Collection` that are also contained in the specified `Collection`.
- `clear` &#151; removes all elements from the `Collection`.

The `addAll`, `removeAll`, and `retainAll` methods all return `true` if the target `Collection` was modified in the process of executing the operation.

As a simple example of the power of bulk operations, consider the following idiom to remove *all* instances of a specified element, `e`, from a `Collection`, `c`.

```

c.removeAll(Collections.singleton(e));

```

More specifically, suppose you want to remove all of the `null` elements from a `Collection`.

```

c.removeAll(Collections.singleton(null));

```

This idiom uses `Collections.singleton`, which is a static factory method that returns an immutable `Set` containing only the specified element. <a name="ArrayOps" id="ArrayOps"></a>

## Collection Interface Array Operations

The `toArray` methods are provided as a bridge between collections and older APIs that expect arrays on input. The array operations allow the contents of a `Collection` to be translated into an array. The simple form with no arguments creates a new array of `Object`. The more complex form allows the caller to provide an array or to choose the runtime type of the output array.

For example, suppose that `c` is a `Collection`. The following snippet dumps the contents of `c` into a newly allocated array of `Object` whose length is identical to the number of elements in `c`.

```

Object[] a = c.toArray();

```

Suppose that `c` is known to contain only strings (perhaps because `c` is of type `Collection&lt;String&gt;`). The following snippet dumps the contents of `c` into a newly allocated array of `String` whose length is identical to the number of elements in `c`.

```

String[] a = c.toArray(new String[0]);

```
