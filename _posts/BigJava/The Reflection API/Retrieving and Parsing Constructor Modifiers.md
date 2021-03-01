
# Creating New Class Instances

There are two reflective methods for creating instances of classes: 
[`java.lang.reflect.Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) and 
[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--). The former is preferred and is thus used in these examples because:

<li>
[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--) can only invoke the zero-argument constructor, while 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) may invoke any constructor, regardless of the number of parameters.</li>
<li>
[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--) throws any exception thrown by the constructor, regardless of whether it is checked or unchecked. 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) always wraps the thrown exception with an
[`InvocationTargetException`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html).</li>
<li>
[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--) requires that the constructor be visible; 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) may invoke `private` constructors under certain circumstances.</li>

Sometimes it may be desirable to retrieve internal state from an object which is only set after construction. Consider a scenario where it is necessary to obtain the internal character set used by 
[`java.io.Console`](https://docs.oracle.com/javase/8/docs/api/java/io/Console.html). (The `Console` character set is stored in an private field and is not necessarily the same as the Java virtual machine default character set returned by 
[`java.nio.charset.Charset.defaultCharset()`](https://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html#defaultCharset--)). The 
[`<code>ConsoleCharset`</code>](example/ConsoleCharset.java) example shows how this might be achieved:

```


import java.io.Console;
import java.nio.charset.Charset;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import static java.lang.System.out;

public class ConsoleCharset {
    public static void main(String... args) {
	Constructor[] ctors = Console.class.getDeclaredConstructors();
	Constructor ctor = null;
	for (int i = 0; i &lt; ctors.length; i++) {
	    ctor = ctors[i];
	    if (ctor.getGenericParameterTypes().length == 0)
		break;
	}

	try {
	    ctor.setAccessible(true);
 	    Console c = (Console)ctor.newInstance();
	    Field f = c.getClass().getDeclaredField("cs");
	    f.setAccessible(true);
	    out.format("Console charset         :  %s%n", f.get(c));
	    out.format("Charset.defaultCharset():  %s%n",
		       Charset.defaultCharset());

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
 	} catch (InvocationTargetException x) {
 	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	}
    }
}

```


[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--) will only succeed if the constructor is has zero arguments and is already accessible. Otherwise, it is necessary to use 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) as in the above example.

Example output for a UNIX system:

```

$ **java ConsoleCharset**
Console charset          :  ISO-8859-1
Charset.defaultCharset() :  ISO-8859-1

```

Example output for a Windows system:

```

C:\&gt; **java ConsoleCharset**
Console charset          :  IBM437
Charset.defaultCharset() :  windows-1252

```

Another common application of 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) is to invoke constructors which take arguments. The 
[`<code>RestoreAliases`</code>](example/RestoreAliases.java) example finds a specific single-argument constructor and invokes it:

```


import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;
import static java.lang.System.out;

class EmailAliases {
    private Set&lt;String&gt; aliases;
    private EmailAliases(HashMap&lt;String, String&gt; h) {
	aliases = h.keySet();
    }

    public void printKeys() {
	out.format("Mail keys:%n");
	for (String k : aliases)
	    out.format("  %s%n", k);
    }
}

public class RestoreAliases {

    private static Map&lt;String, String&gt; defaultAliases = new HashMap&lt;String, String&gt;();
    static {
	defaultAliases.put("Duke", "duke@i-love-java");
	defaultAliases.put("Fang", "fang@evil-jealous-twin");
    }

    public static void main(String... args) {
	try {
	    Constructor ctor = EmailAliases.class.getDeclaredConstructor(HashMap.class);
	    ctor.setAccessible(true);
	    EmailAliases email = (EmailAliases)ctor.newInstance(defaultAliases);
	    email.printKeys();

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	}
    }
}

```

This example uses 
[`Class.getDeclaredConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructor-java.lang.Class...-) to find the constructor with a single argument of type 
[`java.util.HashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html). Note that it is sufficient to pass `HashMap.class` since the parameter to any `get*Constructor()` method requires a class only for type purposes. Due to 
[type erasure](https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.6), the following expression evaluates to `true`:

```

HashMap.class == defaultAliases.getClass()

```

The example then creates a new instance of the class using this constructor with 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-).

```

$ **java RestoreAliases**
Mail keys:
  Duke
  Fang

```
