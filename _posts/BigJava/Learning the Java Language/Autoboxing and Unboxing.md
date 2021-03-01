
# Autoboxing and Unboxing


**Autoboxing** is the automatic conversion that the Java compiler makes between the primitive types and their corresponding object wrapper classes. For example, converting an <tt>int</tt> to an <tt>Integer</tt>, a <tt>double</tt> to a <tt>Double</tt>, and so on. If the conversion goes the other way, this is called **unboxing**.


Here is the simplest example of autoboxing:

```

Character ch = 'a';

```


The rest of the examples in this section use generics. If you are not yet familiar with the syntax of generics, see the
[Generics (Updated)](../generics/index.html) lesson.


Consider the following code:

```

List&lt;Integer&gt; li = new ArrayList&lt;&gt;();
for (int i = 1; i &lt; 50; i += 2)
    li.add(i);

```


Although you add the <tt>int</tt> values as primitive types, rather than <tt>Integer</tt> objects, to <tt>li</tt>, the code compiles. Because <tt>li</tt> is a list of <tt>Integer</tt> objects, not a list of <tt>int</tt> values, you may wonder why the Java compiler does not issue a compile-time error. The compiler does not generate an error because it creates an <tt>Integer</tt> object from <tt>i</tt> and adds the object to <tt>li</tt>. Thus, the compiler converts the previous code to the following at runtime:

```

List&lt;Integer&gt; li = new ArrayList&lt;&gt;();
for (int i = 1; i &lt; 50; i += 2)
    li.add(Integer.valueOf(i));

```


Converting a primitive value (an <tt>int</tt>, for example) into an object of the corresponding wrapper class (<tt>Integer</tt>) is called autoboxing. The Java compiler applies autoboxing when a primitive value is:

- Passed as a parameter to a method that expects an object of the corresponding wrapper class.
- Assigned to a variable of the corresponding wrapper class.


Consider the following method:

```

public static int sumEven(List&lt;Integer&gt; li) {
    int sum = 0;
    for (Integer i: li)
        if (i % 2 == 0)
            sum += i;
        return sum;
}

```


Because the remainder (<tt>%</tt>) and unary plus (<tt>+=</tt>) operators do not apply to <tt>Integer</tt> objects, you may wonder why the Java compiler compiles the method without issuing any errors. The compiler does not generate an error because it invokes the <tt>intValue</tt> method to convert an <tt>Integer</tt> to an <tt>int</tt> at runtime:

```

public static int sumEven(List&lt;Integer&gt; li) {
    int sum = 0;
    for (Integer i : li)
        if (i.intValue() % 2 == 0)
            sum += i.intValue();
        return sum;
}

```


Converting an object of a wrapper type (<tt>Integer</tt>) to its corresponding primitive (<tt>int</tt>) value is called unboxing. The Java compiler applies unboxing when an object of a wrapper class is:

- Passed as a parameter to a method that expects a value of the corresponding primitive type.
- Assigned to a variable of the corresponding primitive type.

The 
[`<tt>Unboxing</tt>`](examples/Unboxing.java) example shows how this works:

```

import java.util.ArrayList;
import java.util.List;

public class Unboxing {

    public static void main(String[] args) {
        Integer i = new Integer(-8);

        // 1. Unboxing through method invocation
        int absVal = absoluteValue(i);
        System.out.println("absolute value of " + i + " = " + absVal);

        List&lt;Double&gt; ld = new ArrayList&lt;&gt;();
        ld.add(3.1416);    // &#928; is autoboxed through method invocation.

        // 2. Unboxing through assignment
        double pi = ld.get(0);
        System.out.println("pi = " + pi);
    }

    public static int absoluteValue(int i) {
        return (i &lt; 0) ? -i : i;
    }
}

```


The program prints the following:

```

absolute value of -8 = 8
pi = 3.1416

```


Autoboxing and unboxing lets developers write cleaner code, making it easier to read. The following table lists the primitive types and their corresponding wrapper classes, which are used by the Java compiler for autoboxing and unboxing:
<th id="h1">Primitive type</th><th id="h2">Wrapper class</th>
<td headers="h1">boolean</td><td headers="h2">Boolean</td>
<td headers="h1">byte</td><td headers="h2">Byte</td>
<td headers="h1">char</td><td headers="h2">Character</td>
<td headers="h1">float</td><td headers="h2">Float</td>
<td headers="h1">int</td><td headers="h2">Integer</td>
<td headers="h1">long</td><td headers="h2">Long</td>
<td headers="h1">short</td><td headers="h2">Short</td>
<td headers="h1">double</td><td headers="h2">Double</td>
