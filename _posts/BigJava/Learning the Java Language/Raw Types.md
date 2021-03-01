
# Raw Types


A **raw type** is the name of a generic class or interface without any type arguments. For example, given the generic <tt>Box</tt> class:

```

public class Box&lt;T&gt; {
    public void set(T t) { /* ... */ }
    // ...
}

```


To create a parameterized type of <tt>Box&lt;T&gt;</tt>, you supply an actual type argument for the formal type parameter <tt>T</tt>:

```

Box&lt;Integer&gt; intBox = new Box&lt;&gt;();

```


If the actual type argument is omitted, you create a raw type of <tt>Box&lt;T&gt;</tt>:

```

Box rawBox = new Box();

```


Therefore, <tt>Box</tt> is the raw type of the generic type <tt>Box&lt;T&gt;</tt>. However, a non-generic class or interface type is **not** a raw type.


Raw types show up in legacy code because lots of API classes (such as the <tt>Collections</tt> classes) were not generic prior to JDK 5.0. When using raw types, you essentially get pre-generics behavior &#8212; a <tt>Box</tt> gives you <tt>Object</tt>s.  For backward compatibility, assigning a parameterized type to its raw type is allowed:

```

Box&lt;String&gt; stringBox = new Box&lt;&gt;();
Box rawBox = stringBox;               // OK

```


But if you assign a raw type to a parameterized type, you get a warning:

```

Box rawBox = new Box();           // rawBox is a raw type of Box&lt;T&gt;
Box&lt;Integer&gt; intBox = rawBox;     // warning: unchecked conversion

```


You also get a warning if you use a raw type to invoke generic methods defined in the corresponding generic type:

```

Box&lt;String&gt; stringBox = new Box&lt;&gt;();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)

```


The warning shows that raw types bypass generic type checks, deferring the catch of unsafe code to runtime. Therefore, you should avoid using raw types.


The
[Type Erasure](erasure.html) section has more information on how the Java compiler uses raw types.

## Unchecked Error Messages


As mentioned previously, when mixing legacy code with generic code, you may encounter warning messages similar to the following:

```

Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.

```


This can happen when using an older API that operates on raw types, as shown in the following example:

```

public class WarningDemo {
    public static void main(String[] args){
        Box&lt;Integer&gt; bi;
        bi = createBox();
    }

    static Box createBox(){
        return new Box();
    }
}

```


The term "unchecked" means that the compiler does not have enough type information to perform all type checks necessary to ensure type safety. The "unchecked" warning is disabled, by default, though the compiler gives a hint. To see all "unchecked" warnings, recompile with <tt>-Xlint:unchecked</tt>.


Recompiling the previous example with <tt>-Xlint:unchecked</tt> reveals the following additional information:

```

WarningDemo.java:4: warning: [unchecked] unchecked conversion
found   : Box
required: Box&lt;java.lang.Integer&gt;
        bi = createBox();
                      ^
1 warning

```


To completely disable unchecked warnings, use the <tt>-Xlint:-unchecked</tt> flag. The <tt>@SuppressWarnings("unchecked")</tt> annotation suppresses unchecked warnings.  If you are unfamiliar with the `@SuppressWarnings` syntax, see
[Annotations](../../java/annotations/index.html).
