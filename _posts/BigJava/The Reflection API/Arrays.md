
# Identifying Array Types

Array types may be identified by invoking 
[`Class.isArray()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isArray--). To obtain a 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) use one of the methods described in [Retrieving Class Objects](../class/classNew.html) section of this trail.

The 
[`<code>ArrayFind`</code>](example/ArrayFind.java) example identifies the fields in the named class that are of array type and reports the component type for each of them.

```


import java.lang.reflect.Field;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ArrayFind {
    public static void main(String... args) {
	boolean found = false;
 	try {
	    Class&lt;?&gt; cls = Class.forName(args[0]);
	    Field[] flds = cls.getDeclaredFields();
	    for (Field f : flds) {
 		Class&lt;?&gt; c = f.getType();
		if (c.isArray()) {
		    found = true;
		    out.format("%s%n"
                               + "           Field: %s%n"
			       + "            Type: %s%n"
			       + "  Component Type: %s%n",
			       f, f.getName(), c, c.getComponentType());
		}
	    }
	    if (!found) {
		out.format("No array fields%n");
	    }

        // production code should handle this exception more gracefully
 	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

The syntax for the returned value of `Class.get*Type()` is described in 
[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--). The number of '`[`' characters at the beginning of the type name indicates the number of dimensions (i.e. depth of nesting) of the array.

Samples of the output follows. User input is in italics. An array of primitive type `byte`:

```

$**java ArrayFind java.nio.ByteBuffer**
final byte[] java.nio.ByteBuffer.hb
           Field: hb
            Type: class [B
  Component Type: byte

```

An array of reference type 
[`StackTraceElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/StackTraceElement.html):

```

$ **java ArrayFind java.lang.Throwable**
private java.lang.StackTraceElement[] java.lang.Throwable.stackTrace
           Field: stackTrace
            Type: class [Ljava.lang.StackTraceElement;
  Component Type: class java.lang.StackTraceElement

```

`predefined` is a one-dimensional array of reference type 
[`java.awt.Cursor`](https://docs.oracle.com/javase/8/docs/api/java/awt/Cursor.html) and `cursorProperties` is a two-dimensional array of reference type 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html):

```

$ **java ArrayFind java.awt.Cursor**
protected static java.awt.Cursor[] java.awt.Cursor.predefined
           Field: predefined
            Type: class [Ljava.awt.Cursor;
  Component Type: class java.awt.Cursor
static final java.lang.String[][] java.awt.Cursor.cursorProperties
           Field: cursorProperties
            Type: class [[Ljava.lang.String;
  Component Type: class [Ljava.lang.String;

```
