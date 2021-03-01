
# Getting and Setting Fields with Enum Types

Fields which store enums are set and retrieved as any other reference type, using 
[`Field.set()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#set-java.lang.Object-java.lang.Object-) and 
[`Field.get()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#get-java.lang.Object-). For more information on accessing fields, see the [Fields](../member/field.html) section of this trail.

Consider application which needs to dynamically modify the trace level in a server application which normally does not allow this change during runtime. Assume the instance of the server object is available. The 
[`<code>SetTrace`</code>](example/SetTrace.java) example shows how code can translate the 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) representation of an enum into an enum type and retrieve and set the value of a field storing an enum.

```


import java.lang.reflect.Field;
import static java.lang.System.out;

enum TraceLevel { OFF, LOW, MEDIUM, HIGH, DEBUG }

class MyServer {
    private TraceLevel level = TraceLevel.OFF;
}

public class SetTrace {
    public static void main(String... args) {
	TraceLevel newLevel = TraceLevel.valueOf(args[0]);

	try {
	    MyServer svr = new MyServer();
	    Class&lt;?&gt; c = svr.getClass();
	    Field f = c.getDeclaredField("level");
	    f.setAccessible(true);
	    TraceLevel oldLevel = (TraceLevel)f.get(svr);
	    out.format("Original trace level:  %s%n", oldLevel);

	    if (oldLevel != newLevel) {
 		f.set(svr, newLevel);
		out.format("    New  trace level:  %s%n", f.get(svr));
	    }

        // production code should handle these exceptions more gracefully
	} catch (IllegalArgumentException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	}
    }
}

```

Since the enum constants are singletons, the `==` and `!=` operators may be used to compare enum constants of the same type.

```

$ **java SetTrace OFF**
Original trace level:  OFF
$ **java SetTrace DEBUG**
Original trace level:  OFF
    New  trace level:  DEBUG

```
