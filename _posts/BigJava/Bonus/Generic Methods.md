
# Interoperating with Legacy Code

Until now, all our examples have assumed an idealized world, where everyone is using the latest version of the Java programming language, which supports generics.

Alas, in reality this isn't the case. Millions of lines of code have been written in earlier versions of the language, and they won't all be converted overnight.

Later, in the 
[Converting Legacy Code to Use Generics](convert.html) section, we will tackle the problem of converting your old code to use generics. In this section, we'll focus on a simpler problem: how can legacy code and generic code interoperate? This question has two parts: using legacy code from within generic code and using generic code within legacy code.

## Using Legacy Code in Generic Code

How can you use old code, while still enjoying the benefits of generics in your own code?

As an example, assume you want to use the package `com.Example.widgets`. The folks at Example.com market a system for inventory control, highlights of which are shown below:

```

**package** com.Example.widgets;

**public interface** Part {...}

**public class** Inventory {
    /**
     * Adds a new Assembly to the inventory database.
     * The assembly is given the name name, and 
     * consists of a set parts specified by parts. 
     * All elements of the collection parts
     * must support the Part interface.
     **/ 
    **public static void** addAssembly(String name, Collection parts) {...}
    **public static** Assembly getAssembly(String name) {...}
}

**public interface** Assembly {
    // *Returns a collection of Parts*
    Collection getParts();
}

```

Now, you'd like to add new code that uses the API above. It would be nice to ensure that you always called `addAssembly()` with the proper arguments - that is, that the collection you pass in is indeed a `Collection` of `Part`. Of course, generics are tailor made for this:

```

**package** com.mycompany.inventory;

**import** com.Example.widgets.*;

**public class** Blade implements Part {
    ...
}

**public class** Guillotine implements Part {
}

**public class** Main {
    **public static void** main(String[] args) {
        Collection&lt;Part&gt; c = new ArrayList&lt;Part&gt;();
        c.add(new Guillotine()) ;
        c.add(new Blade());
        Inventory.addAssembly("thingee", c);
        Collection&lt;Part&gt; k = Inventory.getAssembly("thingee").getParts();
    }
}

```

When we call `addAssembly`, it expects the second parameter to be of type `Collection`. The actual argument is of type `Collection&lt;Part&gt;`. This works, but why? After all, most collections don't contain `Part` objects, and so in general, the compiler has no way of knowing what kind of collection the type `Collection` refers to.

In proper generic code, `Collection` would always be accompanied by a type parameter. When a generic type like `Collection` is used without a type parameter, it's called a *raw type*.

Most people's first instinct is that `Collection` really means `Collection&lt;Object&gt;`. However, as we saw earlier, it isn't safe to pass a `Collection&lt;Part&gt;` in a place where a `Collection&lt;Object&gt;` is required. It's more accurate to say that the type `Collection` denotes a collection of some unknown type, just like `Collection&lt;?&gt;`.

But wait, that can't be right either! Consider the call to `getParts()`, which returns a `Collection`. This is then assigned to `k`, which is a `Collection&lt;Part&gt;`. If the result of the call is a `Collection&lt;?&gt;`, the assignment would be an error.

In reality, the assignment is legal, but it generates an *unchecked warning*. The warning is needed, because the fact is that the compiler can't guarantee its correctness. We have no way of checking the legacy code in `getAssembly()` to ensure that indeed the collection being returned is a collection of `Part`s. The type used in the code is `Collection`, and one could legally insert all kinds of objects into such a collection.

So, shouldn't this be an error? Theoretically speaking, yes; but practically speaking, if generic code is going to call legacy code, this has to be allowed. It's up to you, the programmer, to satisfy yourself that in this case, the assignment is safe because the contract of `getAssembly()` says it returns a collection of `Part`s, even though the type signature doesn't show this.

So raw types are very much like wildcard types, but they are not typechecked as stringently. This is a deliberate design decision, to allow generics to interoperate with pre-existing legacy code.

Calling legacy code from generic code is inherently dangerous; once you mix generic code with non-generic legacy code, all the safety guarantees that the generic type system usually provides are void. However, you are still better off than you were without using generics at all. At least you know the code on your end is consistent.

At the moment there's a lot more non-generic code out there then there is generic code, and there will inevitably be situations where they have to mix.

If you find that you must intermix legacy and generic code, pay close attention to the unchecked warnings. Think carefully how you can justify the safety of the code that gives rise to the warning.

What happens if you still made a mistake, and the code that caused a warning is indeed not type safe? Let's take a look at such a situation. In the process, we'll get some insight into the workings of the compiler.

## Erasure and Translation

```

**public** String loophole(Integer x) {
    List&lt;String&gt; ys = new LinkedList&lt;String&gt;();
    List xs = ys;
    xs.add(x); // *Compile-time unchecked warning*
    **return** ys.iterator().next();
}

```

Here, we've aliased a list of strings and a plain old list. We insert an `Integer` into the list, and attempt to extract a `String`. This is clearly wrong. If we ignore the warning and try to execute this code, it will fail exactly at the point where we try to use the wrong type. At run time, this code behaves like:

```

**public** String loophole(Integer x) {
    List ys = new LinkedList;
    List xs = ys;
    xs.add(x); 
    **return**(String) ys.iterator().next(); // *run time error*
}

```

When we extract an element from the list, and attempt to treat it as a string by casting it to `String`, we will get a `ClassCastException`. The exact same thing happens with the generic version of `loophole()`.

The reason for this is, that generics are implemented by the Java compiler as a front-end conversion called *erasure*. You can (almost) think of it as a source-to-source translation, whereby the generic version of `loophole()` is converted to the non-generic version.

As a result, **the type safety and integrity of the Java virtual machine are never at risk, even in the presence of unchecked warnings**.

Basically, erasure gets rid of (or *erases*) all generic type information. All the type information betweeen angle brackets is thrown out, so, for example, a parameterized type like `List&lt;String&gt;` is converted into `List`. All remaining uses of type variables are replaced by the upper bound of the type variable (usually `Object`). And, whenever the resulting code isn't type-correct, a cast to the appropriate type is inserted, as in the last line of `loophole`.

The full details of erasure are beyond the scope of this tutorial, but the simple description we just gave isn't far from the truth. It's good to know a bit about this, especially if you want to do more sophisticated things like converting existing APIs to use generics (see the 
[Converting Legacy Code to Use Generics](convert.html) section), or just want to understand why things are the way they are. <!-- inverse: calling generic libraries from legacy code-->

## Using Generic Code in Legacy Code

Now let's consider the inverse case. Imagine that Example.com chose to convert their API to use generics, but that some of their clients haven't yet. So now the code looks like:

```

**package** com.Example.widgets;

**public interface** Part { 
    ...
}

**public class** Inventory {
    /**
     * Adds a new Assembly to the inventory database.
     * The assembly is given the name name, and 
     * consists of a set parts specified by parts. 
     * All elements of the collection parts
     * must support the Part interface.
     **/ 
    **public static void** addAssembly(String name, Collection&lt;Part&gt; parts) {...}
    **public static** Assembly getAssembly(String name) {...}
}

**public interface** Assembly {
    // *Returns a collection of Parts*
    Collection&lt;Part&gt; getParts();
}

```

and the client code looks like:

```

**package** com.mycompany.inventory;

**import** com.Example.widgets.*;

**public class** Blade implements Part {
...
}

**public class** Guillotine implements Part {
}

**public class** Main {
    **public static void** main(String[] args) {
        Collection c = new ArrayList();
        c.add(new Guillotine()) ;
        c.add(new Blade());

        // *1: unchecked warning*
        Inventory.addAssembly("thingee", c);

        Collection k = Inventory.getAssembly("thingee").getParts();
    }
}

```

The client code was written before generics were introduced, but it uses the package `com.Example.widgets` and the collection library, both of which are using generic types. All the uses of generic type declarations in the client code are raw types.

Line 1 generates an unchecked warning, because a raw `Collection` is being passed in where a `Collection` of `Part`s is expected, and the compiler cannot ensure that the raw `Collection` really is a `Collection` of `Part`s.

As an alternative, you can compile the client code using the source 1.4 flag, ensuring that no warnings are generated. However, in that case you won't be able to use any of the new language features introduced in JDK 5.0.

<!--restrictions - on casts, instanceOf, reflection, arrays-->
