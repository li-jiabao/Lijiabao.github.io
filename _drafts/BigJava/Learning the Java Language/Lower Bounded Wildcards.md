
# Lower Bounded Wildcards


The
[Upper Bounded Wildcards](upperBounded.html) section shows that an upper bounded wildcard restricts the unknown type to be a specific type or a subtype of that type and is represented using the <tt>extends</tt> keyword. In a similar way, a **lower bounded** wildcard restricts the unknown type to be a specific type or a **super type** of that type.


A lower bounded wildcard is expressed using the wildcard character ('<tt>?</tt>'), following by the <tt>super</tt> keyword, followed by its **lower bound**: <tt>&lt;? super A&gt;</tt>.


Say you want to write a method that puts <tt>Integer</tt> objects into a list. To maximize flexibility, you would like the method to work on <tt>List&lt;Integer&gt;</tt>, <tt>List&lt;Number&gt;</tt>, and <tt>List&lt;Object&gt;</tt> &#8212; anything that can hold <tt>Integer</tt> values.


To write the method that works on lists of <tt>Integer</tt> and the supertypes of <tt>Integer</tt>, such as <tt>Integer</tt>, <tt>Number</tt>, and  <tt>Object</tt>, you would specify <tt>List&lt;? super Integer&gt;</tt>. The term <tt>List&lt;Integer&gt;</tt> is more restrictive than <tt>List&lt;? super Integer&gt;</tt> because the former matches a list of type <tt>Integer</tt> only, whereas the latter matches a list of any type that is a supertype of <tt>Integer</tt>.


The following code adds the numbers 1 through 10 to the end of a list:

```

public static void addNumbers(List&lt;? super Integer&gt; list) {
    for (int i = 1; i &lt;= 10; i++) {
        list.add(i);
    }
}

```


The
[Guidelines for Wildcard Use](wildcardGuidelines.html) section provides guidance on when to use upper bounded wildcards and when to use lower bounded wildcards.
