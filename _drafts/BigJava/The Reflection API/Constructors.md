
# Finding Constructors

A constructor declaration includes the name, modifiers, parameters, and list of throwable exceptions. The 
[`java.lang.reflect.Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html) class provides a way to obtain this information.

The 
[`<code>ConstructorSift`</code>](example/ConstructorSift.java) example illustrates how to search a class's declared constructors for one which has a parameter of a given type.

```


import java.lang.reflect.Constructor;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ConstructorSift {
    public static void main(String... args) {
	try {
	    Class&lt;?&gt; cArg = Class.forName(args[1]);

	    Class&lt;?&gt; c = Class.forName(args[0]);
	    Constructor[] allConstructors = c.getDeclaredConstructors();
	    for (Constructor ctor : allConstructors) {
		Class&lt;?&gt;[] pType  = ctor.getParameterTypes();
		for (int i = 0; i &lt; pType.length; i++) {
		    if (pType[i].equals(cArg)) {
			out.format("%s%n", ctor.toGenericString());

			Type[] gpType = ctor.getGenericParameterTypes();
			for (int j = 0; j &lt; gpType.length; j++) {
			    char ch = (pType[j].equals(cArg) ? '*' : ' ');
			    out.format("%7c%s[%d]: %s%n", ch,
				       "GenericParameterType", j, gpType[j]);
			}
			break;
		    }
		}
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```


[`Method.getGenericParameterTypes()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getGenericParameterTypes--) will consult the <!-- The specific reference should be JVMS3 section 4.4.4 "Signature" -->
Signature Attribute in the class file if it's present. If the attribute isn't available, it falls back on 
[`Method.getParameterType()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#getParameterType--) which was not changed by the introduction of generics. The other methods with name `getGeneric**Foo**()` for some value of **Foo** in reflection are implemented similarly. The syntax for the returned values of `Method.get*Types()` is described in 
[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--).

Here is the output for all constructors in 
[`java.util.Formatter`](https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html) which have a 
[`Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html) argument.

```

$ **java ConstructorSift java.util.Formatter java.util.Locale**
public
java.util.Formatter(java.io.OutputStream,java.lang.String,java.util.Locale)
throws java.io.UnsupportedEncodingException
       GenericParameterType[0]: class java.io.OutputStream
       GenericParameterType[1]: class java.lang.String
      *GenericParameterType[2]: class java.util.Locale
public java.util.Formatter(java.lang.String,java.lang.String,java.util.Locale)
throws java.io.FileNotFoundException,java.io.UnsupportedEncodingException
       GenericParameterType[0]: class java.lang.String
       GenericParameterType[1]: class java.lang.String
      *GenericParameterType[2]: class java.util.Locale
public java.util.Formatter(java.lang.Appendable,java.util.Locale)
       GenericParameterType[0]: interface java.lang.Appendable
      *GenericParameterType[1]: class java.util.Locale
public java.util.Formatter(java.util.Locale)
      *GenericParameterType[0]: class java.util.Locale
public java.util.Formatter(java.io.File,java.lang.String,java.util.Locale)
throws java.io.FileNotFoundException,java.io.UnsupportedEncodingException
       GenericParameterType[0]: class java.io.File
       GenericParameterType[1]: class java.lang.String
      *GenericParameterType[2]: class java.util.Locale

```

The next example output illustrates how to search for a parameter of type `char[]` in 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html).

```

$ **java ConstructorSift java.lang.String "[C"**
java.lang.String(int,int,char[])
       GenericParameterType[0]: int
       GenericParameterType[1]: int
      *GenericParameterType[2]: class [C
public java.lang.String(char[],int,int)
      *GenericParameterType[0]: class [C
       GenericParameterType[1]: int
       GenericParameterType[2]: int
public java.lang.String(char[])
      *GenericParameterType[0]: class [C

```

The syntax for expressing arrays of reference and primitive types acceptable to 
[`Class.forName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName-java.lang.String-) is described in 
[`Class.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName--). Note that the first listed constructor is `package-private`, not `public`. It is returned because the example code uses 
[`Class.getDeclaredConstructors()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructors--) rather than 
[`Class.getConstructors()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructors--), which returns only `public` constructors.

This example shows that searching for arguments of variable arity (which have a variable number of parameters) requires use of array syntax:

```

$ **java ConstructorSift java.lang.ProcessBuilder "[Ljava.lang.String;"**
public java.lang.ProcessBuilder(java.lang.String[])
      *GenericParameterType[0]: class [Ljava.lang.String;

```

This is the actual declaration of the 
[`ProcessBuilder`](https://docs.oracle.com/javase/8/docs/api/java/lang/ProcessBuilder.html#ProcessBuilder-java.lang.String...-) constructor in source code:

```

public ProcessBuilder(String... command)

```

The parameter is represented as a single-dimension array of type `java.lang.String`. This can be distinguished from a parameter that is explicitly an array of `java.lang.String` by invoking 
[`Constructor.isVarArgs()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#isVarArgs--).

The final example reports the output for a constructor which has been declared with a generic parameter type:

```

$ **java ConstructorSift java.util.HashMap java.util.Map**
public java.util.HashMap(java.util.Map&lt;? extends K, ? extends V&gt;)
      *GenericParameterType[0]: java.util.Map&lt;? extends K, ? extends V&gt;

```

Exception types may be retrieved for constructors in a similar way as for methods. See the 
[`<code>MethodSpy`</code>](example/MethodSpy.java) example described in [Obtaining Method Type Information](methodType.html) section for further details.
