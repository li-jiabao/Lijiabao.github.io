
# More Fun with Wildcards

In this section, we'll consider some of the more advanced uses of wildcards. We've seen several examples where bounded wildcards were useful when reading from a data structure. Now consider the inverse, a write-only data structure. The interface `Sink` is a simple example of this sort.

```

**interface** Sink&lt;T&gt; {
    flush(T t);
}

```

We can imagine using it as demonstrated by the code below. The method `writeAll()` is designed to flush all elements of the collection `coll` to the sink `snk`, and return the last element flushed.

```

**public static** &lt;T&gt; T writeAll(Collection&lt;T&gt; coll, Sink&lt;T&gt; snk) {
    T last;
    **for** (T t : coll) {
        last = t;
        snk.flush(last);
    }
    **return** last;
}
...
Sink&lt;Object&gt; s;
Collection&lt;String&gt; cs;
String str = writeAll(cs, s); // *Illegal call.*

```

As written, the call to `writeAll()` is illegal, as no valid type argument can be inferred; neither `String` nor `Object` are appropriate types for `T`, because the `Collection` element and the `Sink` element must be of the same type.

We can fix this error by modifying the signature of `writeAll()` as shown below, using a wildcard.

```

**public static** &lt;T&gt; T writeAll(Collection&lt;? **extends** T&gt;, Sink&lt;T&gt;) {...}
...
// *Call is OK, but wrong return type.* 
String str = writeAll(cs, s);

```

The call is now legal, but the assignment is erroneous, since the return type inferred is `Object` because `T` matches the element type of `s`, which is `Object`.

The solution is to use a form of bounded wildcard we haven't seen yet: wildcards with a *lower bound*. The syntax `? **super** T` denotes an unknown type that is a supertype of `T` (or `T` itself; remember that the supertype relation is reflexive). It is the dual of the bounded wildcards we've been using, where we use `? **extends** T` to denote an unknown type that is a subtype of `T`.

```

**public static** &lt;T&gt; T writeAll(Collection&lt;T&gt; coll, Sink&lt;? **super** T&gt; snk) {
    ...
}
String str = writeAll(cs, s); // *Yes!* 

```

Using this syntax, the call is legal, and the inferred type is `String`, as desired.

Now let's turn to a more realistic example. A `java.util.TreeSet&lt;E&gt;` represents a tree of elements of type `E` that are ordered. One way to construct a `TreeSet` is to pass a `Comparator` object to the constructor. That comparator will be used to sort the elements of the `TreeSet` according to a desired ordering.

```

TreeSet(Comparator&lt;E&gt; c) 

```

The `Comparator` interface is essentially:

```

**interface** Comparator&lt;T&gt; {
    **int** compare(T fst, T snd);
}

```

Suppose we want to create a `TreeSet&lt;String&gt;` and pass in a suitable comparator, We need to pass it a `Comparator` that can compare `String`s. This can be done by a `Comparator&lt;String&gt;`, but a `Comparator&lt;Object&gt;` will do just as well. However, we won't be able to invoke the constructor given above on a `Comparator&lt;Object&gt;`. We can use a lower bounded wildcard to get the flexibility we want:

```

TreeSet(Comparator&lt;? **super** E&gt; c) 

```

This code allows any applicable comparator to be used.

As a final example of using lower bounded wildcards, lets look at the method `Collections.max()`, which returns the maximal element in a collection passed to it as an argument. Now, in order for `max()` to work, all elements of the collection being passed in must implement `Comparable`. Furthermore, they must all be comparable to *each other*.

A first attempt at generifying this method signature yields:

```

**public static** &lt;T **extends** Comparable&lt;T&gt;&gt; T max(Collection&lt;T&gt; coll)

```

That is, the method takes a collection of some type `T` that is comparable to itself, and returns an element of that type. However, this code turns out to be too restrictive. To see why, consider a type that is comparable to arbitrary objects:

```

**class** Foo **implements** Comparable&lt;Object&gt; {
    ...
}
Collection&lt;Foo&gt; cf = ... ;
Collections.max(cf); // *Should work.*

```

Every element of `cf` is comparable to every other element in `cf`, since every such element is a `Foo`, which is comparable to any object, and in particular to another `Foo`. However, using the signature above, we find that the call is rejected. The inferred type must be `Foo`, but `Foo` does not implement `Comparable&lt;Foo&gt;`.

It isn't necessary that `T` be comparable to **exactly** itself. All that's required is that `T` be comparable to one of its supertypes. This give us:

```

**public static** &lt;T **extends** Comparable&lt;? **super** T&gt;&gt; 
        T max(Collection&lt;T&gt; coll)

```

Note that the actual signature of `Collections.max()` is more involved. We return to it in the next section, 
[Converting Legacy Code to Use Generics](convert.html). This reasoning applies to almost any usage of `Comparable` that is intended to work for arbitrary types: You always want to use `Comparable&lt;? **super** T&gt;`.

In general, if you have an API that only uses a type parameter `T` as an argument, its uses should take advantage of lower bounded wildcards (`? **super** T`). Conversely, if the API only returns `T`, you'll give your clients more flexibility by using upper bounded wildcards (`? **extends** T`).

## Wildcard Capture

It should be pretty clear by now that given:

```

Set&lt;?&gt; unknownSet = new HashSet&lt;String&gt;();
...
/* Add an element  t to a Set s. */ 
**public static** &lt;T&gt; **void** addToSet(Set&lt;T&gt; s, T t) {
    ...
}

```

The call below is illegal.

```

addToSet(unknownSet, "abc"); // *Illegal.*

```

It makes no difference that the actual set being passed is a set of strings; what matters is that the expression being passed as an argument is a set of an unknown type, which cannot be guaranteed to be a set of strings, or of any type in particular.

Now, consider the following code:

```

**class** Collections {
    ...
    &lt;T&gt; **public static** Set&lt;T&gt; unmodifiableSet(Set&lt;T&gt; set) {
        ...
    }
}
...
Set&lt;?&gt; s = Collections.unmodifiableSet(unknownSet); // *This works! Why?*

```

It seems this should not be allowed; yet, looking at this specific call, it is perfectly safe to permit it. After all, `unmodifiableSet()` does work for any kind of `Set`, regardless of its element type.

Because this situation arises relatively frequently, there is a special rule that allows such code under very specific circumstances in which the code can be proven to be safe. This rule, known as *wildcard capture*, allows the compiler to infer the unknown type of a wildcard as a type argument to a generic method.
