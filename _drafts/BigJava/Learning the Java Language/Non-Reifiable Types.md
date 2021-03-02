
# Non-Reifiable Types

The section
[Type Erasure](erasure.html) discusses the process where the compiler removes information related to type parameters and type arguments. Type erasure has consequences related to variable arguments (also known as **varargs** ) methods whose varargs formal parameter has a non-reifiable type. See the section
[Arbitrary Number of Arguments](../javaOO/arguments.html#varargs) in
[Passing Information to a Method or a Constructor](../javaOO/arguments.html) for more information about varargs methods.

This page covers the following topics:

- [Non-Reifiable Types](#non-reifiable-types)
- [Heap Pollution](#heap_pollution)
- [Potential Vulnerabilities of Varargs Methods with Non-Reifiable Formal Parameters](#vulnerabilities)
- [Preventing Warnings from Varargs Methods with Non-Reifiable Formal Parameters](#suppressing)

## <a name="non-reifiable-types" id="non-reifiable-types">Non-Reifiable Types</a>


A **reifiable** type is a type whose type information is fully available at runtime. This includes primitives, non-generic types, raw types, and invocations of unbound wildcards.


**Non-reifiable types** are types where information has been removed at compile-time by type erasure &#8212; invocations of generic types that are not defined as unbounded wildcards. A non-reifiable type does not have all of its information available at runtime. Examples of non-reifiable types are <tt>List&lt;String&gt;</tt> and <tt>List&lt;Number&gt;</tt>; the JVM cannot tell the difference between these types at runtime. As shown in
[Restrictions on Generics](restrictions.html), there are certain situations where non-reifiable types cannot be used: in an <tt>instanceof</tt> expression, for example, or as an element in an array.

## <a name="heap_pollution" id="heap_pollution">Heap Pollution</a>

**Heap pollution** occurs when a variable of a parameterized type refers to an object that is not of that parameterized type. This situation occurs if the program performed some operation that gives rise to an unchecked warning at compile-time. An **unchecked warning** is generated if, either at compile-time (within the limits of the compile-time type checking rules) or at runtime, the correctness of an operation involving a parameterized type (for example, a cast or method call) cannot be verified. For example, heap pollution occurs when mixing raw types and parameterized types, or when performing unchecked casts.


In normal situations, when all code is compiled at the same time, the compiler issues an unchecked warning to draw your attention to potential heap pollution. If you compile sections of your code separately, it is difficult to detect the potential risk of heap pollution. If you ensure that your code compiles without warnings, then no heap pollution can occur.

## <a name="vulnerabilities" id="vulnerabilities">Potential Vulnerabilities of Varargs Methods with Non-Reifiable Formal Parameters</a>

Generic methods that include vararg input parameters can cause heap pollution.


Consider the following <tt>ArrayBuilder</tt> class:

```

public class ArrayBuilder {

  public static &lt;T&gt; void addToList (List&lt;T&gt; listArg, T... elements) {
    for (T x : elements) {
      listArg.add(x);
    }
  }

  public static void faultyMethod(List&lt;String&gt;... l) {
    Object[] objectArray = l;     // Valid
    objectArray[0] = Arrays.asList(42);
    String s = l[0].get(0);       // ClassCastException thrown here
  }

}

```


The following example, <tt>HeapPollutionExample</tt> uses the <tt>ArrayBuiler</tt> class:

```

public class HeapPollutionExample {

  public static void main(String[] args) {

    List&lt;String&gt; stringListA = new ArrayList&lt;String&gt;();
    List&lt;String&gt; stringListB = new ArrayList&lt;String&gt;();

    ArrayBuilder.addToList(stringListA, "Seven", "Eight", "Nine");
    ArrayBuilder.addToList(stringListB, "Ten", "Eleven", "Twelve");
    List&lt;List&lt;String&gt;&gt; listOfStringLists =
      new ArrayList&lt;List&lt;String&gt;&gt;();
    ArrayBuilder.addToList(listOfStringLists,
      stringListA, stringListB);

    ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));
  }
}

```


When compiled, the following warning is produced by the definition of the <tt>ArrayBuilder.addToList</tt> method:

```

warning: [varargs] Possible heap pollution from parameterized vararg type T

```

When the compiler encounters a varargs method, it translates the varargs formal parameter into an array. However, the Java programming language does not permit the creation of arrays of parameterized types. In the method `ArrayBuilder.addToList`, the compiler translates the varargs formal parameter `T... elements` to the formal parameter `T[] elements`, an array. However, because of type erasure, the compiler converts the varargs formal parameter to `Object[] elements`. Consequently, there is a possibility of heap pollution.


The following statement assigns the varargs formal parameter `l` to the `Object` array `objectArgs`:

```

Object[] objectArray = l;

```

This statement can potentially introduce heap pollution. A value that does match the parameterized type of the varargs formal parameter `l` can be assigned to the variable `objectArray`, and thus can be assigned to `l`. However, the compiler does not generate an unchecked warning at this statement. The compiler has already generated a warning when it translated the varargs formal parameter `List&lt;String&gt;... l` to the formal parameter `List[] l`. This statement is valid; the variable `l` has the type `List[]`, which is a subtype of `Object[]`.

Consequently, the compiler does not issue a warning or error if you assign a `List` object of any type to any array component of the `objectArray` array as shown by this statement:

```

objectArray[0] = Arrays.asList(42);

```

This statement assigns to the first array component of the `objectArray` array with a `List` object that contains one object of type `Integer`.

Suppose you invoke `ArrayBuilder.faultyMethod` with the following statement:

```

ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));

```

At runtime, the JVM throws a `ClassCastException` at the following statement:

```

// ClassCastException thrown here
String s = l[0].get(0);

```

The object stored in the first array component of the variable `l` has the type `List&lt;Integer&gt;`, but this statement is expecting an object of type `List&lt;String&gt;`.

## <a name="suppressing" id="suppressing">Prevent Warnings from Varargs Methods with Non-Reifiable Formal Parameters</a>

If you declare a varargs method that has parameters of a parameterized type, and you ensure that the body of the method does not throw a `ClassCastException` or other similar exception due to improper handling of the varargs formal parameter, you can prevent the warning that the compiler generates for these kinds of varargs methods by adding the following annotation to static and non-constructor method declarations:

```

@SafeVarargs

```

The `@SafeVarargs` annotation is a documented part of the method's contract; this annotation asserts that the implementation of the method will not improperly handle the varargs formal parameter.


It is also possible, though less desirable, to suppress such warnings by adding the following to the method declaration:

```

@SuppressWarnings({"unchecked", "varargs"})

```

However, this approach does not suppress warnings generated from the method's call site.  If you are unfamiliar with the `@SuppressWarnings` syntax, see
[Annotations](../../java/annotations/index.html).
