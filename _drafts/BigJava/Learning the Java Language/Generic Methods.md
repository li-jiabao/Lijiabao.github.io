
# Generic Methods


**Generic methods** are methods that introduce their own type parameters. This is similar to declaring a generic type, but the type parameter's scope is limited to the method where it is declared. Static and non-static generic methods are allowed, as well as generic class constructors.

The syntax for a generic method includes a list of type parameters, inside angle brackets, which appears before the method's return type. For static generic methods, the type parameter section must appear before the method's return type.


The <tt>Util</tt> class includes a generic method, <tt>compare</tt>, which compares two <tt>Pair</tt> objects:

```

public class Util {
    **public static &lt;K, V&gt; boolean compare(Pair&lt;K, V&gt; p1, Pair&lt;K, V&gt; p2)** {
        return p1.getKey().equals(p2.getKey()) &amp;&amp;
               p1.getValue().equals(p2.getValue());
    }
}

public class Pair&lt;K, V&gt; {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}

```


The complete syntax for invoking this method would be:

```

Pair&lt;Integer, String&gt; p1 = new Pair&lt;&gt;(1, "apple");
Pair&lt;Integer, String&gt; p2 = new Pair&lt;&gt;(2, "pear");
boolean same = Util.**&lt;Integer, String&gt;**compare(p1, p2);

```


The type has been explicitly provided, as shown in bold.  Generally, this can be left out and the compiler will infer the type that is needed:

```

Pair&lt;Integer, String&gt; p1 = new Pair&lt;&gt;(1, "apple");
Pair&lt;Integer, String&gt; p2 = new Pair&lt;&gt;(2, "pear");
boolean same = Util.compare(p1, p2);

```


This feature, known as **type inference**, allows you to invoke a generic method as an ordinary method, without specifying a type between angle brackets. This topic is further discussed in the following section,
[Type Inference](genTypeInference.html).
