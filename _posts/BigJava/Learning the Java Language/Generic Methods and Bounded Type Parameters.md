
# Generic Methods and Bounded Type Parameters


Bounded type parameters are key to the implementation of generic algorithms. Consider the following method that counts the number of elements in an array <tt>T[]</tt> that are greater than a specified element <tt>elem</tt>.

```

public static &lt;T&gt; int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e &gt; elem)  // compiler error
            ++count;
    return count;
}

```


The implementation of the method is straightforward, but it does not compile because the greater than operator (<tt>&gt;</tt>) applies only to primitive types such as <tt>short</tt>, <tt>int</tt>, <tt>double</tt>, <tt>long</tt>, <tt>float</tt>, <tt>byte</tt>, and <tt>char</tt>. You cannot use the <tt>&gt;</tt> operator to compare objects. To fix the problem, use a type parameter bounded by the <tt>Comparable&lt;T&gt;</tt> interface:

```

public interface Comparable&lt;T&gt; {
    public int compareTo(T o);
}

```


The resulting code will be:

```

public static &lt;T extends Comparable&lt;T&gt;&gt; int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e.compareTo(elem) &gt; 0)
            ++count;
    return count;
}

```
