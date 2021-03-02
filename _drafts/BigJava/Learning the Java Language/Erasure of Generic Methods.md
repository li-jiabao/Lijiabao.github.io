
# Erasure of Generic Methods


The Java compiler also erases type parameters in generic method arguments. Consider the following generic method:

```

// Counts the number of occurrences of elem in anArray.
//
public static &lt;T&gt; int count(T[] anArray, T elem) {
    int cnt = 0;
    for (T e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}

```


Because <tt>T</tt> is unbounded, the Java compiler replaces it with <tt>Object</tt>:

```

public static int count(Object[] anArray, Object elem) {
    int cnt = 0;
    for (Object e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}

```


Suppose the following classes are defined:

```

class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }

```


You can write a generic method to draw different shapes:

```

public static &lt;T extends Shape&gt; void draw(T shape) { /* ... */ }

```


The Java compiler replaces <tt>T</tt> with <tt>Shape</tt>:

```

public static void draw(Shape shape) { /* ... */ }

```
