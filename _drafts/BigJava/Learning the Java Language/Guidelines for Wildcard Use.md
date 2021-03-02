
# Guidelines for Wildcard Use


One of the more confusing aspects when learning to program with generics is determining when to use an upper bounded wildcard and when to use a lower bounded wildcard. This page provides some guidelines to follow when designing your code.


For purposes of this discussion, it is helpful to think of variables as providing one of two functions:


Of course, some variables are used both for "in" and "out" purposes &#8212; this scenario is also addressed in the guidelines.


You can use the "in" and "out" principle when deciding whether to use a wildcard and what type of wildcard is appropriate. The following list provides the guidelines to follow:

- An "in" variable is defined with an upper bounded wildcard, using the <tt>extends</tt> keyword.
- An "out" variable is defined with a lower bounded wildcard, using the <tt>super</tt> keyword.
- In the case where the "in" variable can be accessed using methods defined in the <tt>Object</tt> class, use an unbounded wildcard.
- In the case where the code needs to access the variable as both an "in" and an "out" variable, do not use a wildcard.


These guidelines do not apply to a method's return type. Using a wildcard as a return type should be avoided because it forces programmers using the code to deal with wildcards.


A list defined by <tt>List&lt;? extends ...&gt;</tt> can be informally thought of as read-only, but that is not a strict guarantee.  Suppose you have the following two classes:

```

class NaturalNumber {

    private int i;

    public NaturalNumber(int i) { this.i = i; }
    // ...
}

class EvenNumber extends NaturalNumber {

    public EvenNumber(int i) { super(i); }
    // ...
}

```


Consider the following code:

```

List&lt;EvenNumber&gt; le = new ArrayList&lt;&gt;();
List&lt;? extends NaturalNumber&gt; ln = le;
ln.add(new NaturalNumber(35));  // compile-time error

```


Because <tt>List&lt;EvenNumber&gt;</tt> is a subtype of <tt>List&lt;? extends NaturalNumber&gt;</tt>, you can assign <tt>le</tt> to <tt>ln</tt>. But you cannot use <tt>ln</tt> to add a natural number to a list of even numbers. The following operations on the list are possible:

- You can add <tt>null</tt>.
- You can invoke <tt>clear</tt>.
- You can get the iterator and invoke <tt>remove</tt>.
- You can capture the wildcard and write elements that you've read from the list.


You can see that the list defined by <tt>List&lt;? extends NaturalNumber&gt;</tt> is not read-only in the strictest sense of the word, but you might think of it that way because you cannot store a new element or change an existing element in the list.
