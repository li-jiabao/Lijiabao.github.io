
# Generics, Inheritance, and Subtypes


As you already know, it is possible to assign an object of one type to an object of another type provided that the types are compatible. For example, you can assign an <tt>Integer</tt> to an <tt>Object</tt>, since <tt>Object</tt> is one of <tt>Integer</tt>'s supertypes:

```

Object someObject = new Object();
Integer someInteger = new Integer(10);
someObject = someInteger;   // OK

```


In object-oriented terminology, this is called an "is a" relationship. Since an <tt>Integer</tt> **is a** kind of <tt>Object</tt>, the assignment is allowed.  But <tt>Integer</tt> is also a kind of <tt>Number</tt>, so the following code is valid as well:

```

public void someMethod(Number n) { /* ... */ }

someMethod(new Integer(10));   // OK
someMethod(new Double(10.1));   // OK

```


The same is also true with generics.  You can perform a generic type invocation, passing <tt>Number</tt> as its type argument, and any subsequent invocation of <tt>add</tt> will be allowed if the argument is compatible with <tt>Number</tt>:

```

Box&lt;Number&gt; box = new Box&lt;Number&gt;();
box.add(new Integer(10));   // OK
box.add(new Double(10.1));  // OK

```


Now consider the following method:

```

public void boxTest(Box&lt;Number&gt; n) { /* ... */ }

```


What type of argument does it accept?  By looking at its signature, you can see that it accepts a single argument whose type is <tt>Box&lt;Number&gt;</tt>.  But what does that mean?  Are you allowed to pass in <tt>Box&lt;Integer&gt;</tt> or <tt>Box&lt;Double&gt;</tt>, as you might expect?  The answer is "no", because <tt>Box&lt;Integer&gt;</tt> and <tt>Box&lt;Double&gt;</tt> are not subtypes of <tt>Box&lt;Number&gt;</tt>.


This is a common misunderstanding when it comes to programming with generics, but it is an important concept to learn.

## <a name="subtypes" id="subtypes">Generic Classes and Subtyping</a>


You can subtype a generic class or interface by extending or implementing it.  The relationship between the type parameters of one class or interface and the type parameters of another are determined by the <tt>extends</tt> and <tt>implements</tt> clauses.


Using the <tt>Collections</tt> classes as an example, <tt>ArrayList&lt;E&gt;</tt> implements <tt>List&lt;E&gt;</tt>, and <tt>List&lt;E&gt; extends Collection&lt;E&gt;</tt>.  So <tt>ArrayList&lt;String&gt;</tt> is a subtype of <tt>List&lt;String&gt;</tt>, which is a subtype of <tt>Collection&lt;String&gt;</tt>.  So long as you do not vary the type argument, the subtyping relationship is preserved between the types.


Now imagine we want to define our own list interface, <tt>PayloadList</tt>, that associates an optional value of generic type <tt>P</tt> with each element.  Its declaration might look like:

```

interface PayloadList&lt;E,P&gt; extends List&lt;E&gt; {
  void setPayload(int index, P val);
  ...
}

```


The following parameterizations of <tt>PayloadList</tt> are subtypes of <tt>List&lt;String&gt;</tt>:

- <tt>PayloadList&lt;String,String&gt;</tt>
- <tt>PayloadList&lt;String,Integer&gt;</tt>
- <tt>PayloadList&lt;String,Exception&gt;</tt>
