
# Obtaining Method Type Information

A method declaration includes the name, modifiers, parameters, return type, and list of throwable exceptions. The 
[`java.lang.reflect.Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html) class provides a way to obtain this information.

The 
[`<code>MethodSpy`</code>](example/MethodSpy.java) example illustrates how to enumerate all of the declared methods in a given class and retrieve the return, parameter, and exception types for all the methods of the given name.

```


import java.lang.reflect.Method;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class MethodSpy {
    private static final String  fmt = "%24s: %s%n";

    // for the morbidly curious
    &lt;E extends RuntimeException&gt; void genericThrow() throws E {}

    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName(args[0]);
	    Method[] allMethods = c.getDeclaredMethods();
	    for (Method m : allMethods) {
		if (!m.getName().equals(args[1])) {
		    continue;
		}
		out.format("%s%n", m.toGenericString());

		out.format(fmt, "ReturnType", m.getReturnType());
		out.format(fmt, "GenericReturnType", m.getGenericReturnType());

		Class&lt;?&gt;[] pType  = m.getParameterTypes();
		Type[] gpType = m.getGenericParameterTypes();
		for (int i = 0; i &lt; pType.length; i++) {
		    out.format(fmt,"ParameterType", pType[i]);
		    out.format(fmt,"GenericParameterType", gpType[i]);
		}

		Class&lt;?&gt;[] xType  = m.getExceptionTypes();
		Type[] gxType = m.getGenericExceptionTypes();
		for (int i = 0; i &lt; xType.length; i++) {
		    out.format(fmt,"ExceptionType", xType[i]);
		    out.format(fmt,"GenericExceptionType", gxType[i]);
		}
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

Here is the output for 
[`Class.getConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-) which is an example of a method with parameterized types and a variable number of parameters.

```

$ **java MethodSpy java.lang.Class getConstructor**
public java.lang.reflect.Constructor&lt;T&gt; java.lang.Class.getConstructor
  (java.lang.Class&lt;?&gt;[]) throws java.lang.NoSuchMethodException,
  java.lang.SecurityException
              ReturnType: class java.lang.reflect.Constructor
       GenericReturnType: java.lang.reflect.Constructor&lt;T&gt;
           ParameterType: class [Ljava.lang.Class;
    GenericParameterType: java.lang.Class&lt;?&gt;[]
           ExceptionType: class java.lang.NoSuchMethodException
    GenericExceptionType: class java.lang.NoSuchMethodException
           ExceptionType: class java.lang.SecurityException
    GenericExceptionType: class java.lang.SecurityException

```

This is the actual declaration of the method in source code:

```

public Constructor&lt;T&gt; getConstructor(Class&lt;?&gt;... parameterTypes)

```

First note that the return and parameter types are generic. 
[`Method.getGenericReturnType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getGenericReturnType--) will consult the <!-- The specific reference should be JVMS3 section 4.4.4 "Signature" -->
Signature Attribute in the class file if it's present. If the attribute isn't available, it falls back on 
[`Method.getReturnType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getReturnType--) which was not changed by the introduction of generics. The other methods with name `getGeneric**Foo**()` for some value of **Foo** in reflection are implemented similarly.

Next, notice that the last (and only) parameter, `parameterType`, is of variable arity (has a variable number of parameters) of type `java.lang.Class`. It is represented as a single-dimension array of type `java.lang.Class`. This can be distinguished from a parameter that is explicitly an array of `java.lang.Class` by invoking 
[`Method.isVarArgs()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#isVarArgs--). The syntax for the returned values of `Method.get*Types()` is described in 
[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--).

The following example illustrates a method with a generic return type.

```

$ **java MethodSpy java.lang.Class cast**
public T java.lang.Class.cast(java.lang.Object)
              ReturnType: class java.lang.Object
       GenericReturnType: T
           ParameterType: class java.lang.Object
    GenericParameterType: class java.lang.Object

```

The generic return type for the method 
[`Class.cast()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#cast-java.lang.Object-) is reported as `java.lang.Object` because generics are implemented via **type erasure** which removes all information regarding generic types during compilation. The erasure of `T` is defined by the declaration of 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html):

```

public final class Class&lt;T&gt; implements ...

```

Thus `T` is replaced by the upper bound of the type variable, in this case, `java.lang.Object`.

The last example illustrates the output for a method with multiple overloads.

```

$ **java MethodSpy java.io.PrintStream format**
public java.io.PrintStream java.io.PrintStream.format
  (java.util.Locale,java.lang.String,java.lang.Object[])
              ReturnType: class java.io.PrintStream
       GenericReturnType: class java.io.PrintStream
           ParameterType: class java.util.Locale
    GenericParameterType: class java.util.Locale
           ParameterType: class java.lang.String
    GenericParameterType: class java.lang.String
           ParameterType: class [Ljava.lang.Object;
    GenericParameterType: class [Ljava.lang.Object;
public java.io.PrintStream java.io.PrintStream.format
  (java.lang.String,java.lang.Object[])
              ReturnType: class java.io.PrintStream
       GenericReturnType: class java.io.PrintStream
           ParameterType: class java.lang.String
    GenericParameterType: class java.lang.String
           ParameterType: class [Ljava.lang.Object;
    GenericParameterType: class [Ljava.lang.Object;

```

If multiple overloads of the same method name are discovered, they are all returned by 
[`Class.getDeclaredMethods()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethods--). Since `format()` has two overloads (with with a 
[`Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html) and one without), both are shown by `MethodSpy`.
