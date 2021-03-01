
# Retrieving and Parsing Method Modifiers

There a several modifiers that may be part of a method declaration:

- Access modifiers: `public`, `protected`, and `private`
- Modifier restricting to one instance: `static`
- Modifier prohibiting value modification: `final`
- Modifier requiring override: `abstract`
- Modifier preventing reentrancy: `synchronized`
- Modifier indicating implementation in another programming language: `native`
- Modifier forcing strict floating point behavior: `strictfp`
- Annotations

The 
[`<code>MethodModifierSpy`</code>](example/MethodModifierSpy.java) example lists the modifiers of a method with a given name. It also displays whether the method is synthetic (compiler-generated), of variable arity, or a bridge method (compiler-generated to support generic interfaces).

```


import java.lang.reflect.Method;
import java.lang.reflect.Modifier;
import static java.lang.System.out;

public class MethodModifierSpy {

    private static int count;
    private static synchronized void inc() { count++; }
    private static synchronized int cnt() { return count; }

    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName(args[0]);
	    Method[] allMethods = c.getDeclaredMethods();
	    for (Method m : allMethods) {
		if (!m.getName().equals(args[1])) {
		    continue;
		}
		out.format("%s%n", m.toGenericString());
		out.format("  Modifiers:  %s%n",
			   Modifier.toString(m.getModifiers()));
		out.format("  [ synthetic=%-5b var_args=%-5b bridge=%-5b ]%n",
			   m.isSynthetic(), m.isVarArgs(), m.isBridge());
		inc();
	    }
	    out.format("%d matching overload%s found%n", cnt(),
		       (cnt() == 1 ? "" : "s"));

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

A few examples of the output 
[`<code>MethodModifierSpy`</code>](example/MethodModifierSpy.java) produces follow.

```

$ **java MethodModifierSpy java.lang.Object wait**
public final void java.lang.Object.wait() throws java.lang.InterruptedException
  Modifiers:  public final
  [ synthetic=false var_args=false bridge=false ]
public final void java.lang.Object.wait(long,int)
  throws java.lang.InterruptedException
  Modifiers:  public final
  [ synthetic=false var_args=false bridge=false ]
public final native void java.lang.Object.wait(long)
  throws java.lang.InterruptedException
  Modifiers:  public final native
  [ synthetic=false var_args=false bridge=false ]
3 matching overloads found

```

```

$ **java MethodModifierSpy java.lang.StrictMath toRadians**
public static double java.lang.StrictMath.toRadians(double)
  Modifiers:  public static strictfp
  [ synthetic=false var_args=false bridge=false ]
1 matching overload found

```

```

$ **java MethodModifierSpy MethodModifierSpy inc**
private synchronized void MethodModifierSpy.inc()
  Modifiers: private synchronized
  [ synthetic=false var_args=false bridge=false ]
1 matching overload found

```

```

$ **java MethodModifierSpy java.lang.Class getConstructor**
public java.lang.reflect.Constructor&lt;T&gt; java.lang.Class.getConstructor
  (java.lang.Class&lt;T&gt;[]) throws java.lang.NoSuchMethodException,
  java.lang.SecurityException
  Modifiers: public transient
  [ synthetic=false var_args=true bridge=false ]
1 matching overload found

```

```

$ **java MethodModifierSpy java.lang.String compareTo**
public int java.lang.String.compareTo(java.lang.String)
  Modifiers: public
  [ synthetic=false var_args=false bridge=false ]
public int java.lang.String.compareTo(java.lang.Object)
  Modifiers: public volatile
  [ synthetic=true  var_args=false bridge=true  ]
2 matching overloads found

```

Note that 
[`Method.isVarArgs()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html#isVarArgs--) returns `true` for 
[`Class.getConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-). This indicates that the method declaration looks like this:

```

public Constructor&lt;T&gt; getConstructor(Class&lt;?&gt;... parameterTypes)

```

not like this:

```

public Constructor&lt;T&gt; getConstructor(Class&lt;?&gt; [] parameterTypes)

```

Notice that the output for 
[`String.compareTo()`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#compareTo-java.lang.String-) contains two methods. The method declared in `String.java`:

```

public int compareTo(String anotherString);

```

and a second **synthetic** or compiler-generated **bridge** method. This occurs because 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) implements the parameterized interface 
[`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html). During type erasure, the argument type of the inherited method 
[`Comparable.compareTo()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#compareTo-T-) is changed from `java.lang.Object` to `java.lang.String`. Since the parameter types for the `compareTo` methods in `Comparable` and `String` no longer match after erasure, overriding can not occur. In all other circumstances, this would produce a compile-time error because the interface is not implemented. The addition of the bridge method avoids this problem.


[`Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html) implements 
[`java.lang.reflect.AnnotatedElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html). Thus any runtime annotations with 
[`java.lang.annotation.RetentionPolicy.RUNTIME`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html#RUNTIME) may be retrieved. For an example of obtaining annotations see the section [Examining Class Modifiers and Types](../class/classModifiers.html).
