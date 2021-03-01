
# Unbounded Wildcards


The unbounded wildcard type is specified using the wildcard character (<tt>?</tt>), for example, <tt>List&lt;?&gt;</tt>. This is called a **list of unknown type**. There are two scenarios where an unbounded wildcard is a useful approach:

- If you are writing a method that can be implemented using functionality provided in the <tt>Object</tt> class.
- When the code is using methods in the generic class that don't depend on the type parameter.  For example, <tt>List.size</tt> or <tt>List.clear</tt>. In fact, <tt>Class&lt;?&gt;</tt> is so often used because most of the methods in <tt>Class&lt;T&gt;</tt> do not depend on <tt>T</tt>.


Consider the following method, <tt>printList</tt>:

```

public static void printList(List&lt;Object&gt; list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}

```


The goal of <tt>printList</tt> is to print a list of any type, but it fails to achieve that goal &#8212; it prints only a list of <tt>Object</tt> instances; it cannot print <tt>List&lt;Integer&gt;</tt>, <tt>List&lt;String&gt;</tt>, <tt>List&lt;Double&gt;</tt>, and so on, because they are not subtypes of <tt>List&lt;Object&gt;</tt>. To write a generic <tt>printList</tt> method, use <tt>List&lt;?&gt;</tt>:

```

public static void printList(List&lt;?&gt; list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}

```


Because for any concrete type <tt>A</tt>, <tt>List&lt;A&gt;</tt> is a subtype of <tt>List&lt;?&gt;</tt>, you can use <tt>printList</tt> to print a list of any type:

```

List&lt;Integer&gt; li = Arrays.asList(1, 2, 3);
List&lt;String&gt;  ls = Arrays.asList("one", "two", "three");
printList(li);
printList(ls);

```


It's important to note that <tt>List&lt;Object&gt;</tt> and <tt>List&lt;?&gt;</tt> are not the same. You can insert an <tt>Object</tt>, or any subtype of <tt>Object</tt>, into a <tt>List&lt;Object&gt;</tt>.  But you can only insert <tt>null</tt> into a <tt>List&lt;?&gt;</tt>. The
[Guidelines for Wildcard Use](wildcardGuidelines.html) section has more information on how to determine what kind of wildcard, if any, should be used in a given situation.
