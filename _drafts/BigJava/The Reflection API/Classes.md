
# Retrieving Class Objects

The entry point for all reflection operations is 
[`java.lang.Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html). With the exception of 
[`java.lang.reflect.ReflectPermission`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/ReflectPermission.html), none of the classes in 
[`java.lang.reflect`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/package-summary.html) have public constructors. To get to these classes, it is necessary to invoke appropriate methods on 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html). There are several ways to get a 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) depending on whether the code has access to an object, the name of class, a type, or an existing 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html).

## Object.getClass()

If an instance of an object is available, then the simplest way to get its 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) is to invoke 
[`Object.getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--). Of course, this only works for reference types which all inherit from 
[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html). Some examples follow.

```

Class c = "foo".getClass();

```

Returns the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) for 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)

```

Class c = System.console().getClass();

```

There is a unique console associated with the virtual machine which is returned by the `static` method 
[`System.console()`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#console--). The value returned by 
[`getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--) is the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) corresponding to 
[`java.io.Console`](https://docs.oracle.com/javase/8/docs/api/java/io/Console.html).

```

enum E { A, B }
Class c = A.getClass();

```

`A` is is an instance of the enum `E`; thus 
[`getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--) returns the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) corresponding to the enumeration type `E`.

```

byte[] bytes = new byte[1024];
Class c = bytes.getClass();

```

Since arrays are 
[`Objects`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html), it is also possible to invoke 
[`getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--) on an instance of an array. The returned 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) corresponds to an array with component type `byte`.

```

import java.util.HashSet;
import java.util.Set;

Set&lt;String&gt; s = new HashSet&lt;String&gt;();
Class c = s.getClass();

```

In this case, 
[`java.util.Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html) is an interface to an object of type 
[`java.util.HashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html). The value returned by 
[`getClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--) is the class corresponding to 
[`java.util.HashSet`](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html).

## The .class Syntax

If the type is available but there is no instance then it is possible to obtain a 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) by appending `".class"` to the name of the type. This is also the easiest way to obtain the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) for a primitive type.

```

boolean b;
Class c = b.getClass();   // compile-time error

Class c = boolean.class;  // correct

```

Note that the statement `boolean.getClass()` would produce a compile-time error because a `boolean` is a primitive type and cannot be dereferenced. The `.class` syntax returns the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) corresponding to the type `boolean`.

```

Class c = java.io.PrintStream.class;

```

The variable `c` will be the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) corresponding to the type 
[`java.io.PrintStream`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html).

```

Class c = int[][][].class;

```

The `.class` syntax may be used to retrieve a 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) corresponding to a multi-dimensional array of a given type.

## Class.forName()

If the fully-qualified name of a class is available, it is possible to get the corresponding 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) using the static method 
[`Class.forName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName-java.lang.String-). This cannot be used for primitive types. The syntax for names of array classes is described by 
[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--). This syntax is applicable to references and primitive types.

```

Class c = Class.forName("com.duke.MyLocaleServiceProvider");

```

This statement will create a class from the given fully-qualified name.

```

Class cDoubleArray = Class.forName("[D");

Class cStringArray = Class.forName("[[Ljava.lang.String;");

```

The variable `cDoubleArray` will contain the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) corresponding to an array of primitive type `double` (i.e. the same as `double[].class`). The `cStringArray` variable will contain the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) corresponding to a two-dimensional array of 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) (i.e. identical to `String[][].class`).

## TYPE Field for Primitive Type Wrappers

The `.class` syntax is a more convenient and the preferred way to obtain the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) for a primitive type; however there is another way to acquire the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html). Each of the primitive types and `void` has a wrapper class in 
[`java.lang`](https://docs.oracle.com/javase/8/docs/api/java/lang/package-summary.html) that is used for boxing of primitive types to reference types. Each wrapper class contains a field named `TYPE` which is equal to the 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) for the primitive type being wrapped.

```

Class c = Double.TYPE;

```

There is a class 
[`java.lang.Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html) which is used to wrap the primitive type `double` whenever an 
[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) is required. The value of 
[`Double.TYPE`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html#TYPE) is identical to that of `double.class`.

```

Class c = Void.TYPE;

```


[`Void.TYPE`](https://docs.oracle.com/javase/8/docs/api/java/lang/Void.html#TYPE) is identical to `void.class`.

## Methods that Return Classes

There are several Reflection APIs which return classes but these may only be accessed if a 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) has already been obtained either directly or indirectly.

```

Class c = javax.swing.JButton.class.getSuperclass();

```

```

Class&lt;?&gt;[] c = Character.class.getClasses();

```

```

Class&lt;?&gt;[] c = Character.class.getDeclaredClasses();

```

```

import java.lang.reflect.Field;

Field f = System.class.getField("out");
Class c = f.getDeclaringClass();

```

```

public class MyClass {
    static Object o = new Object() {
        public void m() {} 
    };
    static Class&lt;c&gt; = o.getClass().getEnclosingClass();
}

```

```

Class c = Thread.State.class().getEnclosingClass();

```

```

public class MyClass {
    static Object o = new Object() { 
        public void m() {} 
    };
    static Class&lt;c&gt; = o.getClass().getEnclosingClass();
}

```
