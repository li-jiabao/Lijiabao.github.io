
# Wildcards and Subtyping


As described in
[Generics, Inheritance, and Subtypes](inheritance.html), generic classes or interfaces are not related merely because there is a relationship between their types. However, you can use wildcards to create a relationship between generic classes or interfaces.


Given the following two regular (non-generic) classes:

```

class A { /* ... */ }
class B extends A { /* ... */ }

```


It would be reasonable to write the following code:

```

B b = new B();
A a = b;

```


This example shows that inheritance of regular classes follows this rule of subtyping: class <tt>B</tt> is a subtype of class <tt>A</tt> if <tt>B</tt> extends <tt>A</tt>. This rule does not apply to generic types:

```

List&lt;B&gt; lb = new ArrayList&lt;&gt;();
List&lt;A&gt; la = lb;   // compile-time error

```


Given that <tt>Integer</tt> is a subtype of <tt>Number</tt>, what is the relationship between <tt>List&lt;Integer&gt;</tt> and <tt>List&lt;Number&gt;</tt>?


Although <tt>Integer</tt> is a subtype of <tt>Number</tt>, <tt>List&lt;Integer&gt;</tt> is not a subtype of <tt>List&lt;Number&gt;</tt> and, in fact, these two types are not related. The common parent of <tt>List&lt;Number&gt;</tt> and <tt>List&lt;Integer&gt;</tt> is <tt>List&lt;?&gt;</tt>.


In order to create a relationship between these classes so that the code can access <tt>Number</tt>'s methods through <tt>List&lt;Integer&gt;</tt>'s elements, use an upper bounded wildcard:

```

List&lt;? extends Integer&gt; intList = new ArrayList&lt;&gt;();
List&lt;? extends Number&gt;  numList = intList;  // OK. List&lt;? extends Integer&gt; is a subtype of List&lt;? extends Number&gt;

```


Because <tt>Integer</tt> is a subtype of <tt>Number</tt>, and <tt>numList</tt> is a list of <tt>Number</tt> objects, a relationship now exists between <tt>intList</tt> (a list of <tt>Integer</tt> objects) and <tt>numList</tt>. The following diagram shows the relationships between several <tt>List</tt> classes declared with both upper and lower bounded wildcards.


The
[Guidelines for Wildcard Use](wildcardGuidelines.html) section has more information about the ramifications of using upper and lower bounded wildcards.
