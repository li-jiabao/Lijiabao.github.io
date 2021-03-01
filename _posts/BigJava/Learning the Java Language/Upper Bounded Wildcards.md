
# Upper Bounded Wildcards


You can use an upper bounded wildcard to relax the restrictions on a variable.  For example, say you want to write a method that works on <tt>List&lt;Integer&gt;</tt>, <tt>List&lt;Double&gt;</tt>, **and** <tt>List&lt;Number&gt;</tt>;  you can achieve this by using an upper bounded wildcard.


To declare an upper-bounded wildcard, use the wildcard character ('<tt>?</tt>'), followed by the <tt>extends</tt> keyword, followed by its **upper bound**. Note that, in this context, <tt>extends</tt> is used in a general sense to mean either "extends" (as in classes) or "implements" (as in interfaces).


To write the method that works on lists of <tt>Number</tt> and the subtypes of <tt>Number</tt>, such as <tt>Integer</tt>, <tt>Double</tt>, and <tt>Float</tt>, you would specify <tt>List&lt;? extends Number&gt;</tt>. The term <tt>List&lt;Number&gt;</tt> is more restrictive than <tt>List&lt;? extends Number&gt;</tt> because the former matches a list of type <tt>Number</tt> only, whereas the latter matches a list of type <tt>Number</tt> or any of its subclasses.


Consider the following <tt>process</tt> method:

```

public static void process(List**&lt;? extends Foo&gt;** list) { /* ... */ }

```


The upper bounded wildcard, <tt>&lt;? extends Foo&gt;</tt>, where <tt>Foo</tt> is any type, matches <tt>Foo</tt> and any subtype of <tt>Foo</tt>. The <tt>process</tt> method can access the list elements as type <tt>Foo</tt>:

```

public static void process(List&lt;? extends Foo&gt; list) {
    for (Foo elem : list) {
        // ...
    }
}

```


In the <tt>foreach</tt> clause, the <tt>elem</tt> variable iterates over each element in the list. Any method defined in the <tt>Foo</tt> class can now be used on <tt>elem</tt>.


The <tt>sumOfList</tt> method returns the sum of the numbers in a list:

```

public static double sumOfList(List&lt;? extends Number&gt; list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}

```


The following code, using a list of <tt>Integer</tt> objects, prints <tt>sum = 6.0</tt>:

```

List&lt;Integer&gt; li = Arrays.asList(1, 2, 3);
System.out.println("sum = " + sumOfList(li));

```


A list of <tt>Double</tt> values can use the same <tt>sumOfList</tt> method. The following code prints <tt>sum = 7.0</tt>:

```

List&lt;Double&gt; ld = Arrays.asList(1.2, 2.3, 3.5);
System.out.println("sum = " + sumOfList(ld));

```
