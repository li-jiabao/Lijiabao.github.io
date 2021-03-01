
# Questions and Exercises: Interfaces

## Questions

1. What methods would a class that implements the `java.lang.CharSequence` interface have to implement?
<li>What is wrong with the following interface?
<pre><code>
public interface SomethingIsWrong {
    void aMethod(int aValue){
        System.out.println("Hi Mom");
    }
}
</code></pre>
</li>
1. Fix the interface in question 2.
<li>Is the following interface valid?
<pre><code>
public interface Marker {
}
</code></pre>
</li>

## Exercises

1. Write a class that implements the `CharSequence` interface found in the `java.lang` package. Your implementation should return the string backwards. Select one of the sentences from this book to use as the data. Write a small `main` method to test your class; make sure to call all four methods.
1. Suppose you have written a time server that periodically notifies its clients of the current date and time. Write an interface the server could use to enforce a particular protocol on its clients.


[Check your answers.](interfaces-answers.html)
