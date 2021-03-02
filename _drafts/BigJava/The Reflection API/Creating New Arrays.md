
# Getting and Setting Arrays and Their Components

Just as in non-reflective code, an array field may be set or retrieved in its entirety or component by component. To set the entire array at once, use 
[`java.lang.reflect.Field.set(Object obj, Object value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#set-java.lang.Object-java.lang.Object-). To retrieve the entire array, use 
[`Field.get(Object)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#get-java.lang.Object-). Individual components can be set or retrieved using methods in 
[`java.lang.reflect.Array`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html).


[`Array`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html) provides methods of the form `set**Foo**()` and `get**Foo**()` for setting and getting components of any primitive type. For example, the component of an `int` array may be set with 
[`Array.setInt(Object array, int index, int value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#setInt-java.lang.Objectint-int-) and may be retrieved with 
[`Array.getInt(Object array, int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#getInt-java.lang.Object-int-).

These methods support automatic **widening** of data types. Therefore, 
[`Array.getShort()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#getShort-java.lang.Object-int-) may be used to set the values of an `int` array since a 16-bit `short` may be widened to a 32-bit `int` without loss of data; on the other hand, invoking 
[`Array.setLong()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#setLong-java.lang.Object-int-long-) on an array of `int` will cause an 
[`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html) to be thrown because a 64-bit `long` can not be narrowed to for storage in a 32-bit `int` with out loss of information. This is true regardless of whether the actual values being passed could be accurately represented in the target data type. [**The Java Language Specification, Java SE 7 Edition**](https://docs.oracle.com/javase/specs/jls/se7/html/index.html), sections [Widening Primitive Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.2) and [Narrowing Primitive Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.3) contains a complete discussion of widening and narrowing conversions.

The components of arrays of reference types (including arrays of arrays) are set and retrieved using 
[`Array.set(Object array, int index, int value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#set-java.lang.Object-int-int-) and  
[`Array.get(Object array, int index)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#get-java.lang.Object-int-).

## Setting a Field of Type Array

The 
[`<code>GrowBufferedReader`</code>](example/GrowBufferedReader.java) example illustrates how to replace the value of a field of type array. In this case, the code replaces the backing array for a 
[`java.io.BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html) with a larger one. (This assumes that the creation of the original `BufferedReader` is in code that is not modifiable; otherwise, it would be trivial to simply use the alternate constructor 
[`BufferedReader(java.io.Reader in, int size)`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html#BufferedReader-java.io.Reader-int-) which accepts an input buffer size.)

```


import java.io.BufferedReader;
import java.io.CharArrayReader;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.lang.reflect.Field;
import java.util.Arrays;
import static java.lang.System.out;

public class GrowBufferedReader {
    private static final int srcBufSize = 10 * 1024;
    private static char[] src = new char[srcBufSize];
    static {
	src[srcBufSize - 1] = 'x';
    }
    private static CharArrayReader car = new CharArrayReader(src);

    public static void main(String... args) {
	try {
	    BufferedReader br = new BufferedReader(car);

	    Class&lt;?&gt; c = br.getClass();
	    Field f = c.getDeclaredField("cb");

	    // cb is a private field
	    f.setAccessible(true);
	    char[] cbVal = char[].class.cast(f.get(br));

	    char[] newVal = Arrays.copyOf(cbVal, cbVal.length * 2);
	    if (args.length &gt; 0 &amp;&amp; args[0].equals("grow"))
		f.set(br, newVal);

	    for (int i = 0; i &lt; srcBufSize; i++)
		br.read();

	    // see if the new backing array is being used
	    if (newVal[srcBufSize - 1] == src[srcBufSize - 1])
		out.format("Using new backing array, size=%d%n", newVal.length);
	    else
		out.format("Using original backing array, size=%d%n", cbVal.length);

        // production code should handle these exceptions more gracefully
	} catch (FileNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (IOException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java GrowBufferedReader grow**
Using new backing array, size=16384
$ **java GrowBufferedReader**
Using original backing array, size=8192

```

Note that the above example makes use of the array utility method 
[`java.util.Arrays.copyOf)`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#copyOf-char:A-int-). 
[`java.util.Arrays`](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html) contains many methods which are convenient when operating on arrays.

## Accessing Elements of a Multidimensional Array

Multi-dimensional arrays are simply nested arrays. A two-dimensional array is an array of arrays. A three-dimensional array is an array of two-dimensional arrays, and so on. The 
[`<code>CreateMatrix`</code>](example/CreateMatrix.java) example illustrates how to create and initialize a multi-dimensional array using reflection.

```


import java.lang.reflect.Array;
import static java.lang.System.out;

public class CreateMatrix {
    public static void main(String... args) {
        Object matrix = Array.newInstance(int.class, 2, 2);
        Object row0 = Array.get(matrix, 0);
        Object row1 = Array.get(matrix, 1);

        Array.setInt(row0, 0, 1);
        Array.setInt(row0, 1, 2);
        Array.setInt(row1, 0, 3);
        Array.setInt(row1, 1, 4);

        for (int i = 0; i &lt; 2; i++)
            for (int j = 0; j &lt; 2; j++)
                out.format("matrix[%d][%d] = %d%n", i, j, ((int[][])matrix)[i][j]);
    }
}

```

```

$ **java CreateMatrix**
matrix[0][0] = 1
matrix[0][1] = 2
matrix[1][0] = 3
matrix[1][1] = 4

```

The same result could be obtained by using the following code fragment:

```

Object matrix = Array.newInstance(int.class, 2);
Object row0 = Array.newInstance(int.class, 2);
Object row1 = Array.newInstance(int.class, 2);

Array.setInt(row0, 0, 1);
Array.setInt(row0, 1, 2);
Array.setInt(row1, 0, 3);
Array.setInt(row1, 1, 4);

Array.set(matrix, 0, row0);
Array.set(matrix, 1, row1);

```

The variable argument 
[`Array.newInstance(Class&lt;?&gt; componentType, int... dimensions)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#newInstance-java.lang.Class-int...-) provides a convenient way to create multi-dimensional arrays, but the components still need to initialized using the principle that that multi-dimensional arrays are nested arrays. (Reflection does not provide multiple indexed `get`/`set` methods for this purpose.)
