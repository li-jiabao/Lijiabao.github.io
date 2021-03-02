
# Annotations Basics

## The Format of an Annotation

In its simplest form, an annotation looks like the following:

```

@Entity

```


The at sign character (`@`) indicates to the compiler that what follows is an annotation. In the following example, the annotation's name is `Override`:

```

@Override
void mySuperMethod() { ... }

```


The annotation can include **elements**, which can be named or unnamed, and there are values for those elements:

```

@Author(
   name = "Benjamin Franklin",
   date = "3/27/2003"
)
class MyClass() { ... }

```

or

```

@SuppressWarnings(value = "unchecked")
void myMethod() { ... }

```

If there is just one element named `value`, then the name can be omitted, as in:

```

@SuppressWarnings("unchecked")
void myMethod() { ... }

```


If the annotation has no elements, then the parentheses can be omitted, as shown in the previous `@Override` example.


It is also possible to use multiple annotations on the same declaration:

```

@Author(name = "Jane Doe")
@EBook
class MyClass { ... }

```


If the annotations have the same type, then this is called a repeating annotation:

```

@Author(name = "Jane Doe")
@Author(name = "John Smith")
class MyClass { ... }

```


Repeating annotations are supported as of the Java SE 8 release. For more information, see
[Repeating Annotations](repeating.html).


The annotation type can be one of the types that are defined in the `java.lang` or `java.lang.annotation` packages of the Java SE API. In the previous examples, `Override` and `SuppressWarnings` are
[predefined Java annotations](predefined.html). It is also possible to define your own annotation type. The `Author` and `Ebook` annotations in the previous example are custom annotation types.

## Where Annotations Can Be Used


Annotations can be applied to declarations: declarations of classes, fields, methods, and other program elements. When used on a declaration, each annotation often appears, by convention, on its own line.


As of the Java SE 8 release, annotations can also be applied to the **use** of types. Here are some examples:

<li>Class instance creation expression:
<pre><code>
    new @Interned MyObject();
</code></pre>
</li>
<li>Type cast:
<pre><code>
    myString = (@NonNull String) str;
</code></pre>
</li>
<li>`implements` clause:
<pre><code>
    class UnmodifiableList&lt;T&gt; implements
        @Readonly List&lt;@Readonly T&gt; { ... }
</code></pre>
</li>
<li>Thrown exception declaration:
<pre><code>
    void monitorTemperature() throws
        @Critical TemperatureException { ... }
</code></pre>
</li>


This form of annotation is called a **type annotation**. For more information, see
[Type Annotations and Pluggable Type Systems](type_annotations.html).
