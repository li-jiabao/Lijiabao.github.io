
# Troubleshooting

Here are a few common problems encountered by developers with explanations for why the occur and how to resolve them.

## IllegalArgumentException due to Inconvertible Types

The 
[`<code>FieldTrouble`</code>](example/FieldTrouble.java) example will generate an 
[`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html). 
[`Field.setInt()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#setInt-java.lang.Object-int-) is invoked to set a field that is of the reference type `Integer` with a value of primitive type. In the non-reflection equivalent `Integer val = 42`, the compiler would convert (or **box**) the primitive type `42` to a reference type as `new Integer(42)` so that its type checking will accept the statement. When using reflection, type checking only occurs at runtime so there is no opportunity to box the value.

```


import java.lang.reflect.Field;

public class FieldTrouble {
    public Integer val;

    public static void main(String... args) {
	FieldTrouble ft = new FieldTrouble();
	try {
	    Class&lt;?&gt; c = ft.getClass();
	    Field f = c.getDeclaredField("val");
  	    f.setInt(ft, 42);               // IllegalArgumentException

        // production code should handle these exceptions more gracefully
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
 	} catch (IllegalAccessException x) {
 	    x.printStackTrace();
	}
    }
}

```

```

$ **java FieldTrouble**
Exception in thread "main" java.lang.IllegalArgumentException: Can not set
  java.lang.Object field FieldTrouble.val to (long)42
        at sun.reflect.UnsafeFieldAccessorImpl.throwSetIllegalArgumentException
          (UnsafeFieldAccessorImpl.java:146)
        at sun.reflect.UnsafeFieldAccessorImpl.throwSetIllegalArgumentException
          (UnsafeFieldAccessorImpl.java:174)
        at sun.reflect.UnsafeObjectFieldAccessorImpl.setLong
          (UnsafeObjectFieldAccessorImpl.java:102)
        at java.lang.reflect.Field.setLong(Field.java:831)
        at FieldTrouble.main(FieldTrouble.java:11)

```

To eliminate this exception, the problematic line should be replaced by the following invocation of 
[`Field.set(Object obj, Object value)`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#set-java.lang.Object-java.lang.Object-):

```

f.set(ft, new Integer(43));

```

```

Integer.class.isAssignableFrom(int.class) == false

```

Similarly, automatic conversion from primitive to reference type is also impossible in reflection.

```

int.class.isAssignableFrom(Integer.class) == false

```

## NoSuchFieldException for Non-Public Fields

The astute reader may notice that if the 
[`<code>FieldSpy`</code>](example/FieldSpy.java) example shown earlier is used to get information on a non-public field, it will fail:

```

$ **java FieldSpy java.lang.String count**
java.lang.NoSuchFieldException: count
        at java.lang.Class.getField(Class.java:1519)
        at FieldSpy.main(FieldSpy.java:12)

```

## IllegalAccessException when Modifying Final Fields

An 
[`IllegalAccessException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalAccessException.html) may be thrown if an attempt is made to get or set the value of a `private` or otherwise inaccessible field or to set the value of a `final` field (regardless of its access modifiers).

The 
[`<code>FieldTroubleToo`</code>](example/FieldTroubleToo.java) example illustrates the type of stack trace which results from attempting to set a final field.

```


import java.lang.reflect.Field;

public class FieldTroubleToo {
    public final boolean b = true;

    public static void main(String... args) {
	FieldTroubleToo ft = new FieldTroubleToo();
	try {
	    Class&lt;?&gt; c = ft.getClass();
	    Field f = c.getDeclaredField("b");
// 	    f.setAccessible(true);  // solution
	    f.setBoolean(ft, Boolean.FALSE);   // IllegalAccessException

        // production code should handle these exceptions more gracefully
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	} catch (IllegalArgumentException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java FieldTroubleToo**
java.lang.IllegalAccessException: Can not set final boolean field
  FieldTroubleToo.b to (boolean)false
        at sun.reflect.UnsafeFieldAccessorImpl.
          throwFinalFieldIllegalAccessException(UnsafeFieldAccessorImpl.java:55)
        at sun.reflect.UnsafeFieldAccessorImpl.
          throwFinalFieldIllegalAccessException(UnsafeFieldAccessorImpl.java:63)
        at sun.reflect.UnsafeQualifiedBooleanFieldAccessorImpl.setBoolean
          (UnsafeQualifiedBooleanFieldAccessorImpl.java:78)
        at java.lang.reflect.Field.setBoolean(Field.java:686)
        at FieldTroubleToo.main(FieldTroubleToo.java:12)

```
