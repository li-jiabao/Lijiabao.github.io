
# Type Inference


**Type inference** is a Java compiler's ability to look at each method invocation and corresponding declaration to determine the type argument (or arguments) that make the invocation applicable. The inference algorithm determines the types of the arguments and, if available, the type that the result is being assigned, or returned. Finally, the inference algorithm tries to find the **most specific** type that works with all of the arguments.


To illustrate this last point, in the following example, inference determines that the second argument being passed to the <tt>pick</tt> method is of type <tt>Serializable</tt>:

```

static &lt;T&gt; T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList&lt;String&gt;());

```

## <a name="type-inference-methods" id="type-inference-methods">Type Inference and Generic Methods</a>


[Generic Methods](methods.html) introduced you to type inference, which enables you to invoke a generic method as you would an ordinary method, without specifying a type between angle brackets. Consider the following example, 
[`BoxDemo`](examples/BoxDemo.java), which requires the 
[`Box`](examples/Box.java) class:

```

public class BoxDemo {

  public static &lt;U&gt; void addBox(U u, 
      java.util.List&lt;Box&lt;U&gt;&gt; boxes) {
    Box&lt;U&gt; box = new Box&lt;&gt;();
    box.set(u);
    boxes.add(box);
  }

  public static &lt;U&gt; void outputBoxes(java.util.List&lt;Box&lt;U&gt;&gt; boxes) {
    int counter = 0;
    for (Box&lt;U&gt; box: boxes) {
      U boxContents = box.get();
      System.out.println("Box #" + counter + " contains [" +
             boxContents.toString() + "]");
      counter++;
    }
  }

  public static void main(String[] args) {
    java.util.ArrayList&lt;Box&lt;Integer&gt;&gt; listOfIntegerBoxes =
      new java.util.ArrayList&lt;&gt;();
    BoxDemo.&lt;Integer&gt;addBox(Integer.valueOf(10), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(30), listOfIntegerBoxes);
    BoxDemo.outputBoxes(listOfIntegerBoxes);
  }
}

```

The following is the output from this example:

```

Box #0 contains [10]
Box #1 contains [20]
Box #2 contains [30]

```

The generic method `addBox` defines one type parameter named `U`. Generally, a Java compiler can infer the type parameters of a generic method call. Consequently, in most cases, you do not have to specify them. For example, to invoke the generic method `addBox`, you can specify the type parameter with a **type witness** as follows:

```

BoxDemo.**&lt;Integer&gt;**addBox(Integer.valueOf(10), listOfIntegerBoxes);

```

Alternatively, if you omit the type witness,a Java compiler automatically infers (from the method's arguments) that the type parameter is `Integer`:

```

BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);

```

## <a name="type-inference-instantiation" id="type-inference-instantiation">Type Inference and Instantiation of Generic Classes</a>


You can replace the type arguments required to invoke the constructor of a generic class with an empty set of type parameters (`&lt;&gt;`) as long as the compiler can infer the type arguments from the context. This pair of angle brackets is informally called
[the diamond](types.html#diamond).


For example, consider the following variable declaration:

```

Map&lt;String, List&lt;String&gt;&gt; myMap = new HashMap&lt;String, List&lt;String&gt;&gt;();

```


You can substitute the parameterized type of the constructor with an empty set of type parameters (<tt>&lt;&gt;</tt>):

```

Map&lt;String, List&lt;String&gt;&gt; myMap = new HashMap&lt;&gt;();

```


Note that to take advantage of type inference during generic class instantiation, you must use the diamond. In the following example, the compiler generates an unchecked conversion warning because the `HashMap()` constructor refers to the `HashMap` raw type, not the `Map&lt;String, List&lt;String&gt;&gt;` type:

```

Map&lt;String, List&lt;String&gt;&gt; myMap = new HashMap(); // unchecked conversion warning

```

## <a name="type-inference-constructors" id="type-inference-constructors">Type Inference and Generic Constructors of Generic and Non-Generic Classes</a>

Note that constructors can be generic (in other words, declare their own formal type parameters) in both generic and non-generic classes. Consider the following example:

```

class MyClass&lt;X&gt; {
  &lt;T&gt; MyClass(T t) {
    // ...
  }
}

```

Consider the following instantiation of the class `MyClass`:

```

new MyClass&lt;Integer&gt;("")

```

This statement creates an instance of the parameterized type `MyClass&lt;Integer&gt;`; the statement explicitly specifies the type `Integer` for the formal type parameter, `X`, of the generic class `MyClass&lt;X&gt;`. Note that the constructor for this generic class contains a formal type parameter, `T`. The compiler infers the type `String` for the formal type parameter, `T`, of the constructor of this generic class (because the actual parameter of this constructor is a `String` object).

Compilers from releases prior to Java SE 7 are able to infer the actual type parameters of generic constructors, similar to generic methods. However, compilers in Java SE 7 and later can infer the actual type parameters of the generic class being instantiated if you use the diamond (`&lt;&gt;`). Consider the following example:

```

MyClass&lt;Integer&gt; myObject = new MyClass&lt;&gt;("");

```

In this example, the compiler infers the type `Integer` for the formal type parameter, `X`, of the generic class `MyClass&lt;X&gt;`. It infers the type `String` for the formal type parameter, `T`, of the constructor of this generic class.

## <a name="target_types" id="target_types">Target Types</a>

The Java compiler takes advantage of target typing to infer the type parameters of a generic method invocation. The **target type** of an expression is the data type that the Java compiler expects depending on where the expression appears. Consider the method `Collections.emptyList`, which is declared as follows:

```

static &lt;T&gt; List&lt;T&gt; emptyList();

```

Consider the following assignment statement:

```

List&lt;String&gt; listOne = Collections.emptyList();

```

This statement is expecting an instance of `List&lt;String&gt;`;   this data type is the target type. Because the method `emptyList` returns a value of type `List&lt;T&gt;`, the compiler infers that the type argument `T` must be the value `String`. This works in both Java SE 7 and 8. Alternatively, you could use a type witness and specify the value of `T` as follows:

```

List&lt;String&gt; listOne = Collections.&lt;String&gt;emptyList();

```

However, this is not necessary in this context. It was necessary in other contexts, though. Consider the following method:

```

void processStringList(List&lt;String&gt; stringList) {
    // process stringList
}

```

Suppose you want to invoke the method `processStringList` with an empty list. In Java SE 7, the following statement does not compile:

```

processStringList(Collections.emptyList());

```

The Java SE 7 compiler generates an error message similar to the following:

```

List&lt;Object&gt; cannot be converted to List&lt;String&gt;

```

The compiler requires a value for the type argument `T` so it starts with the value `Object`. Consequently, the invocation of `Collections.emptyList` returns a value of type `List&lt;Object&gt;`, which is incompatible with the method `processStringList`. Thus, in Java SE 7, you must specify the value of the value of the type argument as follows:

```

processStringList(Collections.&lt;String&gt;emptyList());

```

This is no longer necessary in Java SE 8. The notion of what is a target type  has been expanded to include method arguments, such as the argument to the method `processStringList`. In this case, `processStringList` requires an argument of type `List&lt;String&gt;`. The method `Collections.emptyList` returns a value of `List&lt;T&gt;`, so using the target type of `List&lt;String&gt;`, the compiler infers that the type argument `T` has a value of `String`. Thus, in Java SE 8, the following statement compiles:

```

processStringList(Collections.emptyList());

```

See
[Target Typing](../../java/javaOO/lambdaexpressions.html#target-typing) in 
[Lambda Expressions](../../java/javaOO/lambdaexpressions.html) for more information.
