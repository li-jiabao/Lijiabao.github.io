
# Defining Simple Generics

Here is a small excerpt from the definitions of the interfaces `List` and `Iterator` in package `java.util`:

```

**public interface** List &lt;E&gt; {
    **void** add(E x);
    Iterator&lt;E&gt; iterator();
}

**public interface** Iterator&lt;E&gt; {
    E next();
    **boolean** hasNext();
}

```

This code should all be familiar, except for the stuff in angle brackets. Those are the declarations of the *formal type parameters* of the interfaces `List` and `Iterator`.

Type parameters can be used throughout the generic declaration, pretty much where you would use ordinary types (though there are some important restrictions; see the section 
[The Fine Print](fineprint.html).

In the introduction, we saw *invocations* of the generic type declaration `List`, such as `List&lt;Integer&gt;`. In the invocation (usually called a *parameterized type*), all occurrences of the formal type parameter (`E` in this case) are replaced by the *actual type argument* (in this case, `Integer`).

You might imagine that `List&lt;Integer&gt;` stands for a version of `List` where `E` has been uniformly replaced by `Integer`:

```

**public interface** IntegerList {
    **void** add(Integer x);
    Iterator&lt;Integer&gt; iterator();
}

```

This intuition can be helpful, but it's also misleading.

It is helpful, because the parameterized type `List&lt;Integer&gt;` does indeed have methods that look just like this expansion.

It is misleading, because the declaration of a generic is never actually expanded in this way. There aren't multiple copies of the code--not in source, not in binary, not on disk and not in memory. If you are a C++ programmer, you'll understand that this is very different than a C++ template.

A generic type declaration is compiled once and for all, and turned into a single class file, just like an ordinary class or interface declaration.

Type parameters are analogous to the ordinary parameters used in methods or constructors. Much like a method has *formal value parameters* that describe the kinds of values it operates on, a generic declaration has formal type parameters. When a method is invoked, *actual arguments* are substituted for the formal parameters, and the method body is evaluated. When a generic declaration is invoked, the actual type arguments are substituted for the formal type parameters.

A note on naming conventions. We recommend that you use pithy (single character if possible) yet evocative names for formal type parameters. It's best to avoid lower case characters in those names, making it easy to distinguish formal type parameters from ordinary classes and interfaces. Many container types use `E`, for element, as in the examples above. We'll see some additional conventions in later examples.
