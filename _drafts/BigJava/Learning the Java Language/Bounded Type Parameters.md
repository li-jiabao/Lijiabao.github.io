
# Bounded Type Parameters


There may be times when you want to restrict the types that can be used as type arguments in a parameterized type. For example, a method that operates on numbers might only want to accept instances of `Number` or its subclasses. This is what *bounded type parameters* are for.


To declare a bounded type parameter, list the type parameter's name, followed by the `extends` keyword, followed by its *upper bound*, which in this example is `Number`. Note that, in this context, `extends` is used in a general sense to mean either "extends" (as in classes) or "implements" (as in interfaces).

```

public class Box&lt;T&gt; {

    private T t;          

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public &lt;U **extends Number**&gt; void inspect(U u){
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }

    public static void main(String[] args) {
        Box&lt;Integer&gt; integerBox = new Box&lt;Integer&gt;();
        integerBox.set(new Integer(10));
        integerBox.inspect("some text"); // **error: this is still String!**
    }
}

```

By modifying our generic method to include this bounded type parameter, compilation will now fail, since our invocation of `inspect` still includes a `String`:

```

Box.java:21: &lt;U&gt;inspect(U) in Box&lt;java.lang.Integer&gt; cannot
  be applied to (java.lang.String)
                        integerBox.inspect("10");
                                  ^
1 error

```


In addition to limiting the types you can use to instantiate a generic type, bounded type parameters allow you to invoke methods defined in the bounds:

```

public class NaturalNumber&lt;T extends Integer&gt; {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return **n.intValue()** % 2 == 0;
    }

    // ...
}

```


The <tt>isEven</tt> method invokes the <tt>intValue</tt> method defined in the <tt>Integer</tt> class through <tt>n</tt>.

## Multiple Bounds


The preceding example illustrates the use of a type parameter with a single bound, but a type parameter can have **multiple bounds**:

```

&lt;T extends B1 &amp; B2 &amp; B3&gt;

```


A type variable with multiple bounds is a subtype of all the types listed in the bound. If one of the bounds is a class, it must be specified first.  For example:

```

Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D &lt;T extends A &amp; B &amp; C&gt; { /* ... */ }

```


If bound <tt>A</tt> is not specified first, you get a compile-time error:

```

class D &lt;T extends B &amp; A &amp; C&gt; { /* ... */ }  // compile-time error

```
