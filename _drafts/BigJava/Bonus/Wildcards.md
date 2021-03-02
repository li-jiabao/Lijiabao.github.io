
# Generic Methods

Consider writing a method that takes an array of objects and a collection and puts all objects in the array into the collection. Here's a first attempt:

```

**static void** fromArrayToCollection(Object[] a, Collection&lt;?&gt; c) {
    **for** (Object o : a) { 
        c.add(o); // *compile-time error*
    }
}

```

By now, you will have learned to avoid the beginner's mistake of trying to use `Collection&lt;Object&gt;` as the type of the collection parameter. You may or may not have recognized that using `Collection&lt;?&gt;` isn't going to work either. Recall that you cannot just shove objects into a collection of unknown type.

The way to do deal with these problems is to use *generic methods*. Just like type declarations, method declarations can be generic&#151;that is, parameterized by one or more type parameters.

```

**static** &lt;T&gt; **void** fromArrayToCollection(T[] a, Collection&lt;T&gt; c) {
    **for** (T o : a) {
        c.add(o); // *Correct*
    }
}

```

We can call this method with any kind of collection whose element type is a supertype of the element type of the array.

```

Object[] oa = new Object[100];
Collection&lt;Object&gt; co = new ArrayList&lt;Object&gt;();

// *T inferred to be Object*
fromArrayToCollection(oa, co); 

String[] sa = new String[100];
Collection&lt;String&gt; cs = new ArrayList&lt;String&gt;();

// *T inferred to be String*
fromArrayToCollection(sa, cs);

// *T inferred to be Object*
fromArrayToCollection(sa, co);

Integer[] ia = new Integer[100];
Float[] fa = new Float[100];
Number[] na = new Number[100];
Collection&lt;Number&gt; cn = new ArrayList&lt;Number&gt;();

// *T inferred to be Number*
fromArrayToCollection(ia, cn);

// *T inferred to be Number*
fromArrayToCollection(fa, cn);

// *T inferred to be Number*
fromArrayToCollection(na, cn);

// *T inferred to be Object*
fromArrayToCollection(na, co);

// *compile-time error*
fromArrayToCollection(na, cs);

```

Notice that we don't have to pass an actual type argument to a generic method. The compiler infers the type argument for us, based on the types of the actual arguments. It will generally infer the most specific type argument that will make the call type-correct. <!--
% show examples from library

% show wildcard examples and show how they could be done using
% type parameters, and explain when to use generic methods and when not
-->

One question that arises is: when should I use generic methods, and when should I use wildcard types? To understand the answer, let's examine a few methods from the `Collection` libraries.

```

**interface** Collection&lt;E&gt; {
    **public boolean** containsAll(Collection&lt;?&gt; c);
    **public boolean** addAll(Collection&lt;? **extends E**&gt; c);
}

```

We could have used generic methods here instead:

```

**interface** Collection&lt;E&gt; {
    **public** &lt;T&gt; **boolean** containsAll(Collection&lt;T&gt; c);
    **public** &lt;T **extends** E&gt; **boolean** addAll(Collection&lt;T&gt; c);
    // *Hey, type variables can have bounds too!*
}

```

However, in both `containsAll` and `addAll`, the type parameter `T` is used only once. The return type doesn't depend on the type parameter, nor does any other argument to the method (in this case, there simply is only one argument). This tells us that the type argument is being used for polymorphism; its only effect is to allow a variety of actual argument types to be used at different invocation sites. If that is the case, one should use wildcards. Wildcards are designed to support flexible subtyping, which is what we're trying to express here.

Generic methods allow type parameters to be used to express dependencies among the types of one or more arguments to a method and/or its return type. If there isn't such a dependency, a generic method should not be used.

It is possible to use both generic methods and wildcards in tandem. Here is the method `Collections.copy()`:

```

**class** Collections {
    **public static** &lt;T&gt; **void** copy(List&lt;T&gt; dest, List&lt;? **extends** T&gt; src) {
    ...
}

```

Note the dependency between the types of the two parameters. Any object copied from the source list, `src`, must be assignable to the element type `T` of the destination list, `dst`. So the element type of `src` can be any subtype of `T`&#151;we don't care which. The signature of `copy` expresses the dependency using a type parameter, but uses a wildcard for the element type of the second parameter.

We could have written the signature for this method another way, without using wildcards at all:

```

**class** Collections {
    **public static** &lt;T, S **extends** T&gt; **void** copy(List&lt;T&gt; dest, List&lt;S&gt; src) {
    ...
}

```

This is fine, but while the first type parameter is used both in the type of `dst` and in the bound of the second type parameter, `S`, `S` itself is only used once, in the type of `src`&#151;nothing else depends on it. This is a sign that we can replace `S` with a wildcard. Using wildcards is clearer and more concise than declaring explicit type parameters, and should therefore be preferred whenever possible.

Wildcards also have the advantage that they can be used outside of method signatures, as the types of fields, local variables and arrays. Here is an example.

Returning to our shape drawing problem, suppose we want to keep a history of drawing requests. We can maintain the history in a static variable inside class `Shape`, and have `drawAll()` store its incoming argument into the history field.

```

**static** List&lt;List&lt;? extends Shape&gt;&gt; 
    history = new ArrayList&lt;List&lt;? extends Shape&gt;&gt;();

**public void** drawAll(List&lt;? **extends** Shape&gt; shapes) {
    history.addLast(shapes);
    **for** (Shape s: shapes) {
        s.draw(**this**);
    }
}

```

Finally, again let's take note of the naming convention used for the type parameters. We use `T` for type, whenever there isn't anything more specific about the type to distinguish it. This is often the case in generic methods. If there are multiple type parameters, we might use letters that neighbor `T` in the alphabet, such as `S`. If a generic method appears inside a generic class, it's a good idea to avoid using the same names for the type parameters of the method and class, to avoid confusion. The same applies to nested generic classes. <!--
% somewhere figure out how to explain ``any'' and ``some'' -
% subtlety between taking any type and some unknown type, why each 
% occurence of ? is different.

% more details - definitions with bounds, multiple type params


% interfacing with legacy code - raw types
-->
