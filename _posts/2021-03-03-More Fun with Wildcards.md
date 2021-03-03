---
title: More Fun with Wildcards
author: lijiabao
date: 2020-12-06 01:38:21.705883000 +0800
category: Bonus
categories: Bonus
tags: Bonus
---

# Converting Legacy Code to Use Generics

Earlier, we showed how new and legacy code can interoperate. Now, it's time to look at the harder problem of "generifying" old code.

If you decide to convert old code to use generics, you need to think carefully about how you modify the API.

You need to make certain that the generic API is not unduly restrictive; it must continue to support the original contract of the API. Consider again some examples from `java.util.Collection`. The pre-generic API looks like:

```

**interface** Collection {
    **public boolean** containsAll(Collection c);
    **public boolean** addAll(Collection c);
}

```

A naive attempt to generify it would be the following:

```

**interface** Collection&lt;E&gt; {

    **public boolean** containsAll(Collection&lt;E&gt; c);
    **public boolean** addAll(Collection&lt;E&gt; c);
}

```

While this is certainly type safe, it doesn't live up to the API's original contract. The `containsAll()` method works with any kind of incoming collection. It will only succeed if the incoming collection really contains only instances of `E`, but:

- The static type of the incoming collection might differ, perhaps because the caller doesn't know the precise type of the collection being passed in, or perhaps because it is a `Collection&lt;S&gt;`,where `S` is a subtype of `E`.
- It's perfectly legitimate to call `containsAll()` with a collection of a different type. The routine should work, returning `false`.

In the case of `addAll()`, we should be able to add any collection that consists of instances of a subtype of `E`. We saw how to handle this situation correctly in section 
[Generic Methods](methods.html).

You also need to ensure that the revised API retains binary compatibility with old clients. This implies that the erasure of the API must be the same as the original, ungenerified API. In most cases, this falls out naturally, but there are some subtle cases. We'll examine one of the subtlest cases we've encountered, the method `Collections.max()`. As we saw in section 
[More Fun with Wildcards](morefun.html), a plausible signature for `max()` is:

```

**public static** &lt;T **extends** Comparable&lt;? **super** T&gt;&gt; 
        T max(Collection&lt;T&gt; coll)

```

This is fine, except that the erasure of this signature is:

```

**public static** Comparable max(Collection coll)

```

which is different than the original signature of `max()`:

```

**public static** Object max(Collection coll)

```

One could certainly have specified this signature for `max()`, but it was not done, and all the old binary class files that call `Collections.max()` depend on a signature that returns `Object`.

We can force the erasure to be different, by explicitly specifying a superclass in the bound for the formal type parameter `T`.

```

**public static** &lt;T **extends** Object &amp; Comparable&lt;? **super** T&gt;&gt; 
        T max(Collection&lt;T&gt; coll)

```

This is an example of giving *multiple bounds* for a type parameter, using the syntax `T1 &amp; T2 ... &amp; Tn`. A type variable with multiple bounds is known to be a subtype of all of the types listed in the bound. When a multiple bound is used, the first type mentioned in the bound is used as the erasure of the type variable.

Finally, we should recall that `max` only reads from its input collection, and so is applicable to collections of any subtype of `T`.

This brings us to the actual signature used in the JDK:

```

**public static** &lt;T **extends** Object &amp; Comparable&lt;? **super** T&gt;&gt; 
        T max(Collection&lt;? **extends** T&gt; coll)

```

It's very rare that anything so involved comes up in practice, but expert library designers should be prepared to think very carefully when converting existing APIs.

Another issue to watch out for is *covariant returns*, that is, refining the return type of a method in a subclass. You should not take advantage of this feature in an old API. To see why, let's look at an example.

Assume your original API was of the form:

```

**public class** Foo {
    // *Factory. Should create an instance of* 
    // *whatever class it is declared in.*
    **public** Foo create() {
        ...
    }
}

**public class** Bar **extends** Foo {
    // *Actually creates a Bar.*
    **public** Foo create() {
        ...
    }
}

```

Taking advantage of covariant returns, you modify it to:

```

**public class** Foo {
    // *Factory. Should create an instance of* 
    // *whatever class it is declared in.*
    **public** Foo create() {
        ...
    }
}

**public class** Bar **extends** Foo {
    // *Actually creates a Bar.*
    **public** Bar create() {
        ...
    }
}

```

Now, assume a third party client of your code wrote the following:

```

**public class** Baz **extends** Bar {
    // *Actually creates a Baz.*
    **public** Foo create() {
        ...
    }
}

```

The Java virtual machine does not directly support overriding of methods with different return types. This feature is supported by the compiler. Consequently, unless the class `Baz` is recompiled, it will not properly override the `create()` method of `Bar`. Furthermore, `Baz` will have to be modified, since the code will be rejected as written--the return type of `create()` in `Baz` is not a subtype of the return type of `create()` in `Bar`.
