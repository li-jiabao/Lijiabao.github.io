
# Obtaining Field Types

A field may be either of primitive or reference type. There are eight primitive types: `boolean`, `byte`, `short`, `int`, `long`, `char`, `float`, and `double`. A reference type is anything that is a direct or indirect subclass of 
[`java.lang.Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) including interfaces, arrays, and enumerated types.

The 
[`<code>FieldSpy`</code>](example/FieldSpy.java) example prints the field's type and generic type given a fully-qualified binary class name and field name.

```


import java.lang.reflect.Field;
import java.util.List;

public class FieldSpy&lt;T&gt; {
    public boolean[][] b = {{ false, false }, { true, true } };
    public String name  = "Alice";
    public List&lt;Integer&gt; list;
    public T val;

    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName(args[0]);
	    Field f = c.getField(args[1]);
	    System.out.format("Type: %s%n", f.getType());
	    System.out.format("GenericType: %s%n", f.getGenericType());

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	}
    }
}

```

Sample output to retrieve the type of the three public fields in this class (`b`, `name`, and the parameterized type `list`), follows. User input is in italics.

```

$ **java FieldSpy FieldSpy b**
Type: class [[Z
GenericType: class [[Z
$ **java FieldSpy FieldSpy name**
Type: class java.lang.String
GenericType: class java.lang.String
$ **java FieldSpy FieldSpy list**
Type: interface java.util.List
GenericType: java.util.List&lt;java.lang.Integer&gt;
$ **java FieldSpy FieldSpy val**
Type: class java.lang.Object
GenericType: T

```

The type for the field `b` is two-dimensional array of boolean. The syntax for the type name is described in 
[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--).

The type for the field `val` is reported as `java.lang.Object` because generics are implemented via **type erasure** which removes all information regarding generic types during compilation. Thus `T` is replaced by the upper bound of the type variable, in this case, `java.lang.Object`.


[`Field.getGenericType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#getGenericType--) will consult the <!-- The specific reference should be JVMS3 section 4.4.4 "Signature" -->
Signature Attribute in the class file if it's present. If the attribute isn't available, it falls back on 
[`Field.getType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#getType--) which was not changed by the introduction of generics. The other methods in reflection with name `getGeneric**Foo**` for some value of **Foo** are implemented similarly.
