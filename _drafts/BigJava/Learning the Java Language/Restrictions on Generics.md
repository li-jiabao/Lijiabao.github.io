
# Restrictions on Generics


To use Java generics effectively, you must consider the following restrictions:

<li>
[Cannot Instantiate Generic Types with Primitive Types](#instantiate)</li>
<li>
[Cannot Create Instances of Type Parameters](#createObjects)</li>
<li>
[Cannot Declare Static Fields Whose Types are Type Parameters](#createStatic)</li>
<li>
[Cannot Use Casts or <tt>instanceof</tt> With Parameterized Types](#cannotCast)</li>
<li>
[Cannot Create Arrays of Parameterized Types](#createArrays)</li>
<li>
[Cannot Create, Catch, or Throw Objects of Parameterized Types](#cannotCatch)</li>
<li>
[Cannot Overload a Method Where the Formal Parameter Types of Each Overload Erase to the Same Raw Type](#cannotOverload)</li>

## <a name="instantiate" id="instantiate">Cannot Instantiate Generic Types with Primitive Types</a>


Consider the following parameterized type:

```

class Pair&lt;K, V&gt; {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    // ...
}

```


When creating a <tt>Pair</tt> object, you cannot substitute a primitive type for the type parameter <tt>K</tt> or <tt>V</tt>:

```

Pair&lt;**int, char**&gt; p = new Pair&lt;&gt;(8, 'a');  // compile-time error

```


You can substitute only non-primitive types for the type parameters <tt>K</tt> and <tt>V</tt>:

```

Pair&lt;**Integer, Character**&gt; p = new Pair&lt;&gt;(8, 'a');

```


Note that the Java compiler autoboxes <tt>8</tt> to <tt>Integer.valueOf(8)</tt> and '<tt>a</tt>' to <tt>Character('a')</tt>:

```

Pair&lt;Integer, Character&gt; p = new Pair&lt;&gt;(Integer.valueOf(8), new Character('a'));

```

For more information on autoboxing, see
[Autoboxing and Unboxing](../data/autoboxing.html) in the
[Numbers and Strings](../data/index.html) lesson.

## <a name="createObjects" id="createObjects">Cannot Create Instances of Type Parameters</a>


You cannot create an instance of a type parameter. For example, the following code causes a compile-time error:

```

public static &lt;E&gt; void append(List&lt;E&gt; list) {
    E elem = new E();  // compile-time error
    list.add(elem);
}

```


As a workaround, you can create an object of a type parameter through reflection:

```

public static &lt;E&gt; void append(List&lt;E&gt; list, Class&lt;E&gt; cls) throws Exception {
    E elem = cls.newInstance();   // OK
    list.add(elem);
}

```


You can invoke the <tt>append</tt> method as follows:

```

List&lt;String&gt; ls = new ArrayList&lt;&gt;();
append(ls, String.class);

```

## <a name="createStatic" id="createStatic">Cannot Declare Static Fields Whose Types are Type Parameters</a>


A class's static field is a class-level variable shared by all non-static objects of the class.  Hence, static fields of type parameters are not allowed. Consider the following class:

```

public class MobileDevice&lt;T&gt; {
    private static T os;

    // ...
}

```


If static fields of type parameters were allowed, then the following code would be confused:

```

MobileDevice&lt;Smartphone&gt; phone = new MobileDevice&lt;&gt;();
MobileDevice&lt;Pager&gt; pager = new MobileDevice&lt;&gt;();
MobileDevice&lt;TabletPC&gt; pc = new MobileDevice&lt;&gt;();

```


Because the static field <tt>os</tt> is shared by <tt>phone</tt>, <tt>pager</tt>, and <tt>pc</tt>, what is the actual type of <tt>os</tt>? It cannot be <tt>Smartphone</tt>, <tt>Pager</tt>, and <tt>TabletPC</tt> at the same time. You cannot, therefore, create static fields of type parameters.

## <a name="cannotCast" id="cannotCast">Cannot Use Casts or <tt>instanceof</tt> with Parameterized Types</a>


Because the Java compiler erases all type parameters in generic code, you cannot verify which parameterized type for a generic type is being used at runtime:

```

public static &lt;E&gt; void rtti(List&lt;E&gt; list) {
    if (list instanceof ArrayList&lt;Integer&gt;) {  // compile-time error
        // ...
    }
}

```


The set of parameterized types passed to the <tt>rtti</tt> method is:

```

S = { ArrayList&lt;Integer&gt;, ArrayList&lt;String&gt; LinkedList&lt;Character&gt;, ... }

```


The runtime does not keep track of type parameters, so it cannot tell the difference between an <tt>ArrayList&lt;Integer&gt;</tt> and an <tt>ArrayList&lt;String&gt;</tt>. The most you can do is to use an unbounded wildcard to verify that the list is an <tt>ArrayList</tt>:

```

public static void rtti(List&lt;?&gt; list) {
    if (list instanceof ArrayList&lt;?&gt;) {  // OK; instanceof requires a reifiable type
        // ...
    }
}

```


Typically, you cannot cast to a parameterized type unless it is parameterized by unbounded wildcards. For example:

```

List&lt;Integer&gt; li = new ArrayList&lt;&gt;();
List&lt;Number&gt;  ln = (List&lt;Number&gt;) li;  // compile-time error

```


However, in some cases the compiler knows that a type parameter is always valid and allows the cast.  For example:

```

List&lt;String&gt; l1 = ...;
ArrayList&lt;String&gt; l2 = (ArrayList&lt;String&gt;)l1;  // OK

```

## <a name="createArrays" id="createArrays">Cannot Create Arrays of Parameterized Types</a>


You cannot create arrays of parameterized types. For example, the following code does not compile:

```

List&lt;Integer&gt;[] arrayOfLists = new List&lt;Integer&gt;[2];  // compile-time error

```


The following code illustrates what happens when different types are inserted into an array:

```

Object[] strings = new String[2];
strings[0] = "hi";   // OK
strings[1] = 100;    // An ArrayStoreException is thrown.

```


If you try the same thing with a generic list, there would be a problem:

```

Object[] stringLists = new List&lt;String&gt;[];  // compiler error, but pretend it's allowed
stringLists[0] = new ArrayList&lt;String&gt;();   // OK
stringLists[1] = new ArrayList&lt;Integer&gt;();  // An ArrayStoreException should be thrown,
                                            // but the runtime can't detect it.

```


If arrays of parameterized lists were allowed, the previous code would fail to throw the desired <tt>ArrayStoreException</tt>.

## <a name="cannotCatch" id="cannotCatch">Cannot Create, Catch, or Throw Objects of Parameterized Types</a>


A generic class cannot extend the <tt>Throwable</tt> class directly or indirectly.  For example, the following classes will not compile:

```

// Extends Throwable indirectly
class MathException&lt;T&gt; extends Exception { /* ... */ }    // compile-time error

// Extends Throwable directly
class QueueFullException&lt;T&gt; extends Throwable { /* ... */ // compile-time error

```


A method cannot catch an instance of a type parameter:

```

public static &lt;T extends Exception, J&gt; void execute(List&lt;J&gt; jobs) {
    try {
        for (J job : jobs)
            // ...
    } catch (T e) {   // compile-time error
        // ...
    }
}

```


You can, however, use a type parameter in a <tt>throws</tt> clause:

```

class Parser&lt;T extends Exception&gt; {
    public void parse(File file) throws T {     // OK
        // ...
    }
}

```

## <a name="cannotOverload" id="cannotOverload">Cannot Overload a Method Where the Formal Parameter Types of Each Overload Erase to the Same Raw Type</a>


A class cannot have two overloaded methods that will have the same signature after type erasure.

```

public class Example {
    public void print(Set&lt;String&gt; strSet) { }
    public void print(Set&lt;Integer&gt; intSet) { }
}

```


The overloads would all share the same classfile representation and will generate a compile-time error.
