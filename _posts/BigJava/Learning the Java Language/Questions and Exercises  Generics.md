
# Questions and Exercises: Generics

1. Write a generic method to count the number of elements in a collection that have a specific property (for example, odd integers, prime numbers, palindromes).<br /><br />
<li>Will the following class compile? If not, why?
<pre><code>
public final class Algorithm {
    public static &lt;T&gt; T max(T x, T y) {
        return x &gt; y ? x : y;
    }
}
</code></pre>
</li>
1. Write a generic method to exchange the positions of two different elements in an array.<br /><br />
1. If the compiler erases all type parameters at compile time, why should you use generics?<br /><br />
<li>What is the following class converted to after type erasure?
<pre><code>
public class Pair&lt;K, V&gt; {

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey(); { return key; }
    public V getValue(); { return value; }

    public void setKey(K key)     { this.key = key; }
    public void setValue(V value) { this.value = value; }

    private K key;
    private V value;
}
</code></pre>
</li>
<li>What is the following method converted to after type erasure?
<pre><code>
public static &lt;T extends Comparable&lt;T&gt;&gt;
    int findFirstGreaterThan(T[] at, T elem) {
    // ...
}
</code></pre>
</li>
<li>Will the following method compile? If not, why?
<pre><code>
public static void print(List&lt;? extends Number&gt; list) {
    for (Number n : list)
        System.out.print(n + " ");
    System.out.println();
}
</code></pre>
</li>
1. Write a generic method to find the maximal element in the range <tt>[begin, end)</tt> of a list.<br /><br />
<li>Will the following class compile?  If not, why?
<pre><code>
public class Singleton&lt;T&gt; {

    public static T getInstance() {
        if (instance == null)
            instance = new Singleton&lt;T&gt;();

        return instance;
    }

    private static T instance = null;
}
</code></pre>
</li>
<li> Given the following classes:
<pre><code>
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }

class Node&lt;T&gt; { /* ... */ }
</code></pre>
Will the following code compile? If not, why?
<pre><code>
Node&lt;Circle&gt; nc = new Node&lt;&gt;();
Node&lt;Shape&gt;  ns = nc;
</code></pre>
</li>
<li>Consider this class:
<pre><code>
class Node&lt;T&gt; implements Comparable&lt;T&gt; {
    public int compareTo(T obj) { /* ... */ }
    // ...
}
</code></pre>
Will the following code compile? If not, why?
<pre><code>
Node&lt;String&gt; node = new Node&lt;&gt;();
Comparable&lt;String&gt; comp = node;
</code></pre>
</li>
<li>How do you invoke the following method to find the first integer in a list that is relatively prime to a list of specified integers?
<pre><code>
public static &lt;T&gt;
    int findFirst(List&lt;T&gt; list, int begin, int end, UnaryPredicate&lt;T&gt; p)
</code></pre>
Note that two integers **a** and **b** are relatively prime if gcd(**a, b**) = 1, where gcd is short for greatest common divisor.</li>
