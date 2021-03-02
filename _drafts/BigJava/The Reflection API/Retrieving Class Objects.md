
# Examining Class Modifiers and Types

A class may be declared with one or more modifiers which affect its runtime behavior:

- Access modifiers: `public`, `protected`, and `private`
- Modifier requiring override: `abstract`
- Modifier restricting to one instance: `static`
- Modifier prohibiting value modification: `final`
- Modifier forcing strict floating point behavior: `strictfp`
- Annotations

Not all modifiers are allowed on all classes, for example an interface cannot be `final` and an enum cannot be `abstract`. 
[`java.lang.reflect.Modifier`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Modifier.html) contains declarations for all possible modifiers. It also contains methods which may be used to decode the set of modifiers returned by 
[`Class.getModifiers()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getModifiers--).

The 
[`<code>ClassDeclarationSpy`</code>](example/ClassDeclarationSpy.java) example shows how to obtain the declaration components of a class including the modifiers, generic type parameters, implemented interfaces, and the inheritance path. Since 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) implements the 
[`java.lang.reflect.AnnotatedElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html) interface it is also possible to query the runtime annotations.

```


import java.lang.annotation.Annotation;
import java.lang.reflect.Modifier;
import java.lang.reflect.Type;
import java.lang.reflect.TypeVariable;
import java.util.Arrays;
import java.util.ArrayList;
import java.util.List;
import static java.lang.System.out;

public class ClassDeclarationSpy {
    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName(args[0]);
	    out.format("Class:%n  %s%n%n", c.getCanonicalName());
	    out.format("Modifiers:%n  %s%n%n",
		       Modifier.toString(c.getModifiers()));

	    out.format("Type Parameters:%n");
	    TypeVariable[] tv = c.getTypeParameters();
	    if (tv.length != 0) {
		out.format("  ");
		for (TypeVariable t : tv)
		    out.format("%s ", t.getName());
		out.format("%n%n");
	    } else {
		out.format("  -- No Type Parameters --%n%n");
	    }

	    out.format("Implemented Interfaces:%n");
	    Type[] intfs = c.getGenericInterfaces();
	    if (intfs.length != 0) {
		for (Type intf : intfs)
		    out.format("  %s%n", intf.toString());
		out.format("%n");
	    } else {
		out.format("  -- No Implemented Interfaces --%n%n");
	    }

	    out.format("Inheritance Path:%n");
	    List&lt;Class&gt; l = new ArrayList&lt;Class&gt;();
	    printAncestor(c, l);
	    if (l.size() != 0) {
		for (Class&lt;?&gt; cl : l)
		    out.format("  %s%n", cl.getCanonicalName());
		out.format("%n");
	    } else {
		out.format("  -- No Super Classes --%n%n");
	    }

	    out.format("Annotations:%n");
	    Annotation[] ann = c.getAnnotations();
	    if (ann.length != 0) {
		for (Annotation a : ann)
		    out.format("  %s%n", a.toString());
		out.format("%n");
	    } else {
		out.format("  -- No Annotations --%n%n");
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static void printAncestor(Class&lt;?&gt; c, List&lt;Class&gt; l) {
	Class&lt;?&gt; ancestor = c.getSuperclass();
 	if (ancestor != null) {
	    l.add(ancestor);
	    printAncestor(ancestor, l);
 	}
    }
}

```

A few samples of the output follows. User input is in italics.

```

$ **java ClassDeclarationSpy java.util.concurrent.ConcurrentNavigableMap**
Class:
  java.util.concurrent.ConcurrentNavigableMap

Modifiers:
  public abstract interface

Type Parameters:
  K V

Implemented Interfaces:
  java.util.concurrent.ConcurrentMap&lt;K, V&gt;
  java.util.NavigableMap&lt;K, V&gt;

Inheritance Path:
  -- No Super Classes --

Annotations:
  -- No Annotations --

```

This is the actual declaration for 
[`java.util.concurrent.ConcurrentNavigableMap`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentNavigableMap.html) in the source code:

```

public interface ConcurrentNavigableMap&lt;K,V&gt;
    extends ConcurrentMap&lt;K,V&gt;, NavigableMap&lt;K,V&gt;

```

```

$ **java ClassDeclarationSpy "[Ljava.lang.String;"**
Class:
  java.lang.String[]

Modifiers:
  public abstract final

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  interface java.lang.Cloneable
  interface java.io.Serializable

Inheritance Path:
  java.lang.Object

Annotations:
  -- No Annotations --

```

Since arrays are runtime objects, all of the type information is defined by the Java virtual machine. In particular, arrays implement 
[`Cloneable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html) and 
[`java.io.Serializable`](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html) and their direct superclass is always 
[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html).

```

$ **java ClassDeclarationSpy java.io.InterruptedIOException**
Class:
  java.io.InterruptedIOException

Modifiers:
  public

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  -- No Implemented Interfaces --

Inheritance Path:
  java.io.IOException
  java.lang.Exception
  java.lang.Throwable
  java.lang.Object

Annotations:
  -- No Annotations --

```

From the inheritance path, it may be deduced that 
[`java.io.InterruptedIOException`](https://docs.oracle.com/javase/8/docs/api/java/io/InterruptedIOException.html) is a checked exception because 
[`RuntimeException`](https://docs.oracle.com/javase/8/docs/api/java/lang/RuntimeException.html) is not present.

```

$ **java ClassDeclarationSpy java.security.Identity**
Class:
  java.security.Identity

Modifiers:
  public abstract

Type Parameters:
  -- No Type Parameters --

Implemented Interfaces:
  interface java.security.Principal
  interface java.io.Serializable

Inheritance Path:
  java.lang.Object

Annotations:
  @java.lang.Deprecated()

```

This output shows that 
[`java.security.Identity`](https://docs.oracle.com/javase/8/docs/api/java/security/Identity.html), a deprecated API, possesses the annotation 
[`java.lang.Deprecated`](https://docs.oracle.com/javase/8/docs/api/java/lang/Deprecated.html). This may be used by reflective code to detect deprecated APIs.
