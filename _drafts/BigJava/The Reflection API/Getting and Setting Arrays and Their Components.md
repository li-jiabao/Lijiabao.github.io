
# Troubleshooting

The following examples show typical errors which may occur when operating on arrays.

## IllegalArgumentException due to Inconvertible Types

The 
[`<code>ArrayTroubleAgain`</code>](example/ArrayTroubleAgain.java) example will generate an 
[`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html). 
[`Array.setInt()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#setInt-java.lang.Object-int-int-) is invoked to set a component that is of the reference type `Integer` with a value of primitive type `int`. In the non-reflection equivalent `ary[0] = 1`, the compiler would convert (or **box**) the value `1` to a reference type as `new Integer(1)` so that its type checking will accept the statement. When using reflection, type checking only occurs at runtime so there is no opportunity to box the value.

```


import java.lang.reflect.Array;
import static java.lang.System.err;

public class ArrayTroubleAgain {
    public static void main(String... args) {
	Integer[] ary = new Integer[2];
	try {
	    Array.setInt(ary, 0, 1);  // IllegalArgumentException

        // production code should handle these exceptions more gracefully
	} catch (IllegalArgumentException x) {
	    err.format("Unable to box%n");
	} catch (ArrayIndexOutOfBoundsException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java ArrayTroubleAgain**
Unable to box

```

To eliminate this exception, the problematic line should be replaced by the following invocation of 
[`Array.set(Object array, int index, Object value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#set-java.lang.Object-int-java.lang.Object-):

```

Array.set(ary, 0, new Integer(1));

```

```

Integer.class.isAssignableFrom(int.class) == false 

```

Similarly, automatic conversion from primitive to reference type is also impossible in reflection.

```

int.class.isAssignableFrom(Integer.class) == false

```

## ArrayIndexOutOfBoundsException for Empty Arrays

The 
[`<code>ArrayTrouble`</code>](example/ArrayTrouble.java) example illustrates an error which will occur if an attempt is made to access the elements of an array of zero length:

```


import java.lang.reflect.Array;
import static java.lang.System.out;

public class ArrayTrouble {
    public static void main(String... args) {
        Object o = Array.newInstance(int.class, 0);
        int[] i = (int[])o;
        int[] j = new int[0];
        out.format("i.length = %d, j.length = %d, args.length = %d%n",
                   i.length, j.length, args.length);
        Array.getInt(o, 0);  // ArrayIndexOutOfBoundsException
    }
}

```

```

$ **java ArrayTrouble**
i.length = 0, j.length = 0, args.length = 0
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException
        at java.lang.reflect.Array.getInt(Native Method)
        at ArrayTrouble.main(ArrayTrouble.java:11)

```

## IllegalArgumentException if Narrowing is Attempted

The 
[`<code>ArrayTroubleToo`</code>](example/ArrayTroubleToo.java) example contains code which fails because it attempts perform an operation which could potentially lose data:

```


import java.lang.reflect.Array;
import static java.lang.System.out;

public class ArrayTroubleToo {
    public static void main(String... args) {
        Object o = new int[2];
        Array.setShort(o, 0, (short)2);  // widening, succeeds
        Array.setLong(o, 1, 2L);         // narrowing, fails
    }
}

```

```

$ **java ArrayTroubleToo**
Exception in thread "main" java.lang.IllegalArgumentException: argument type
  mismatch
        at java.lang.reflect.Array.setLong(Native Method)
        at ArrayTroubleToo.main(ArrayTroubleToo.java:9)

```
