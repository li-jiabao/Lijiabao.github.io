
# Generic Types


A **generic type** is a generic class or interface that is parameterized over types. The following <tt>Box</tt> class will be modified to demonstrate the concept.

## A Simple Box Class


Begin by examining a non-generic <tt>Box</tt> class that operates on objects of any type. It needs only to provide two methods: <tt>set</tt>, which adds an object to the box, and <tt>get</tt>, which retrieves it:

```

public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}

```


Since its methods accept or return an <tt>Object</tt>, you are free to pass in whatever you want, provided that it is not one of the primitive types. There is no way to verify, at compile time, how the class is used. One part of the code may place an <tt>Integer</tt> in the box and expect to get <tt>Integer</tt>s out of it, while another part of the code may mistakenly pass in a <tt>String</tt>, resulting in a runtime error.

## A Generic Version of the Box Class


A **generic class** is defined with the following format:

```

class name&lt;T1, T2, ..., Tn&gt; { /* ... */ }

```


The type parameter section, delimited by angle brackets (<tt>&lt;&gt;</tt>), follows the class name. It specifies the **type parameters** (also called **type variables**) <tt>T1</tt>, <tt>T2</tt>, ..., and <tt>Tn</tt>.


To update the <tt>Box</tt> class to use generics, you create a **generic type declaration** by changing the code "<tt>public class Box</tt>" to "<tt>public class Box&lt;T&gt;</tt>". This introduces the type variable, <tt>T</tt>, that can be used anywhere inside the class.


With this change, the <tt>Box</tt> class becomes:

```

/**
 * Generic version of the Box class.
 * @param &lt;T&gt; the type of the value being boxed
 */
public class Box&lt;T&gt; {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}

```


As you can see, all occurrences of <tt>Object</tt> are replaced by <tt>T</tt>.  A type variable can be any **non-primitive** type you specify: any class type, any interface type, any array type, or even another type variable.


This same technique can be applied to create generic interfaces.

## Type Parameter Naming Conventions

By convention, type parameter names are single, uppercase letters. This stands in sharp contrast to the variable 
[naming](../nutsandbolts/variables.html#naming) conventions that you already know about, and with good reason: Without this convention, it would be difficult to tell the difference between a type variable and an ordinary class or interface name.

The most commonly used type parameter names are:

- E - Element (used extensively by the Java Collections Framework)
- K - Key
- N - Number
- T - Type
- V - Value
- S,U,V etc. - 2nd, 3rd, 4th types

You'll see these names used throughout the Java SE API and the rest of this lesson.

## <a name="instantiation" id="instantiation">Invoking and Instantiating a Generic Type</a>


To reference the generic <tt>Box</tt> class from within your code, you must perform a **generic type invocation**, which replaces <tt>T</tt> with some concrete value, such as <tt>Integer</tt>:

```

Box&lt;Integer&gt; integerBox;

```


You can think of a generic type invocation as being similar to an ordinary method invocation, but instead of passing an argument to a method, you are passing a **type argument** &#8212; <tt>Integer</tt> in this case &#8212; to the <tt>Box</tt> class itself.


Like any other variable declaration, this code does not actually create a new <tt>Box</tt> object. It simply declares that <tt>integerBox</tt> will hold a reference to a "<tt>Box</tt> of <tt>Integer</tt>", which is how <tt>Box&lt;Integer&gt;</tt> is read.


An invocation of a generic type is generally known as a **parameterized type**.


To instantiate this class, use the <tt>new</tt> keyword, as usual, but place <tt>&lt;Integer&gt;</tt> between the class name and the parenthesis:

```

Box&lt;Integer&gt; integerBox = new Box&lt;Integer&gt;();

```

## <a name="diamond" id="diamond">The Diamond</a>


In Java SE 7 and later, you can replace the type arguments required to invoke the constructor of a generic class with an empty set of type arguments (&lt;&gt;) as long as the compiler can determine, or infer, the type arguments from the context. This pair of angle brackets, &lt;&gt;, is informally called **the diamond**. For example, you can create an instance of <tt>Box&lt;Integer&gt;</tt> with the following statement:

```

Box&lt;Integer&gt; integerBox = new Box&lt;&gt;();

```


For more information on diamond notation and type inference, see
[Type Inference](genTypeInference.html).

## <a name="multiple" id="multiple">Multiple Type Parameters</a>


As mentioned previously, a generic class can have multiple type parameters. For example, the generic <tt>OrderedPair</tt> class, which implements the generic <tt>Pair</tt> interface:

```

public interface Pair&lt;K, V&gt; {
    public K getKey();
    public V getValue();
}

public class OrderedPair&lt;K, V&gt; implements Pair&lt;K, V&gt; {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}

```


The following statements create two instantiations of the <tt>OrderedPair</tt> class:

```

Pair&lt;String, Integer&gt; p1 = new OrderedPair&lt;String, Integer&gt;("Even", 8);
Pair&lt;String, String&gt;  p2 = new OrderedPair&lt;String, String&gt;("hello", "world");

```


The code, <tt>new OrderedPair&lt;String, Integer&gt;</tt>, instantiates <tt>K</tt> as a <tt>String</tt> and <tt>V</tt> as an <tt>Integer</tt>. Therefore, the parameter types of <tt>OrderedPair</tt>'s constructor are <tt>String</tt> and <tt>Integer</tt>, respectively. Due to 
[autoboxing](../data/autoboxing.html), it is valid to pass a <tt>String</tt> and an <tt>int</tt> to the class.


As mentioned in [The Diamond](#diamond), because a Java compiler can infer the <tt>K</tt> and <tt>V</tt> types from the declaration <tt>OrderedPair&lt;String, Integer&gt;</tt>, these statements can be shortened using diamond notation:

```

OrderedPair&lt;String, Integer&gt; p1 = new OrderedPair**&lt;&gt;**("Even", 8);
OrderedPair&lt;String, String&gt;  p2 = new OrderedPair**&lt;&gt;**("hello", "world");

```


To create a generic interface, follow the same conventions as for creating a generic class.

## Parameterized Types


You can also substitute a type parameter (i.e., <tt>K</tt> or <tt>V</tt>) with a parameterized type (i.e., <tt>List&lt;String&gt;</tt>). For example, using the <tt>OrderedPair&lt;K, V&gt;</tt> example:

```

OrderedPair&lt;String, **Box&lt;Integer&gt;**&gt; p = new OrderedPair&lt;&gt;("primes", new Box&lt;Integer&gt;(...));

```
