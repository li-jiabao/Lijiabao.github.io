
# Discovering Class Members

There are two categories of methods provided in 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) for accessing fields, methods, and constructors: methods which enumerate these members and methods which search for particular members. Also there are distinct methods for accessing members declared directly on the class versus methods which search the superinterfaces and superclasses for inherited members. The following tables provide a summary of all the member-locating methods and their characteristics.

<br />
<th id="h1">[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) API</th><th id="h2">List of members?</th><th id="h3">Inherited members?</th><th id="h4">Private members? <!-- Field =====================--></th>
<td headers="h1">[`getDeclaredField()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredField-java.lang.String-)</td><td headers="h2" align="center">no</td><td headers="h3" align="center">no</td><td headers="h4" align="center">yes</td>
<td headers="h1">[`getField()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getField-java.lang.String-)</td><td headers="h2" align="center">no</td><td headers="h3" align="center">yes</td><td headers="h4" align="center">no</td>
<td headers="h1">[`getDeclaredFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredFields--)</td><td headers="h2" align="center">yes</td><td headers="h3" align="center">no</td><td headers="h4" align="center">yes</td>
<td headers="h1">[`getFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--)</td><td headers="h2" align="center">yes</td><td headers="h3" align="center">yes</td><td headers="h4" align="center">no</td>

<br />
<!-- Method =========================================-->
<th id="h101">[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) API</th><th id="h102">List of members?</th><th id="h103">Inherited members?</th><th id="h104">Private members?</th>
<td headers="h101">[`getDeclaredMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethod-java.lang.String-java.lang.Class...-)</td><td headers="h102" align="center">no</td><td headers="h103" align="center">no</td><td headers="h104" align="center">yes</td>
<td headers="h101">[`getMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class...-)</td><td headers="h102" align="center">no</td><td headers="h103" align="center">yes</td><td headers="h104" align="center">no</td>
<td headers="h101">[`getDeclaredMethods()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethods--)</td><td headers="h102" align="center">yes</td><td headers="h103" align="center">no</td><td headers="h104" align="center">yes</td>
<td headers="h101">[`getMethods()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethods--)</td><td headers="h102" align="center">yes</td><td headers="h103" align="center">yes</td><td headers="h104" align="center">no</td>

<br />
<!-- Constructor ========================================-->
<th id="h201">[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) API</th><th id="h202">List of members?</th><th id="h203">Inherited members?</th><th id="h204">Private members?</th>
<td headers="h201">[`getDeclaredConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructor-java.lang.Class...-)</td><td headers="h202" align="center">no</td><td headers="h203" align="center">N/A<sup>1</sup></td><td headers="h204" align="center">yes</td>
<td headers="h201">[`getConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-)</td><td headers="h202" align="center">no</td><td headers="h203" align="center">N/A<sup>1</sup></td><td headers="h204" align="center">no</td>
<td headers="h201">[`getDeclaredConstructors()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructors--)</td><td headers="h202" align="center">yes</td><td headers="h203" align="center">N/A<sup>1</sup></td><td headers="h204" align="center">yes</td>
<td headers="h201">[`getConstructors()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructors--)</td><td headers="h202" align="center">yes</td><td headers="h203" align="center">N/A<sup>1</sup></td><td headers="h204" align="center">no</td>

<sup>1</sup> Constructors are not inherited.

Given a class name and an indication of which members are of interest, the 
[`<code>ClassSpy`</code>](example/ClassSpy.java) example uses the `get*s()` methods to determine the list of all public elements, including any which are inherited.

```


import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.lang.reflect.Member;
import static java.lang.System.out;

enum ClassMember { CONSTRUCTOR, FIELD, METHOD, CLASS, ALL }

public class ClassSpy {
    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName(args[0]);
	    out.format("Class:%n  %s%n%n", c.getCanonicalName());

	    Package p = c.getPackage();
	    out.format("Package:%n  %s%n%n",
		       (p != null ? p.getName() : "-- No Package --"));

	    for (int i = 1; i &lt; args.length; i++) {
		switch (ClassMember.valueOf(args[i])) {
		case CONSTRUCTOR:
		    printMembers(c.getConstructors(), "Constructor");
		    break;
		case FIELD:
		    printMembers(c.getFields(), "Fields");
		    break;
		case METHOD:
		    printMembers(c.getMethods(), "Methods");
		    break;
		case CLASS:
		    printClasses(c);
		    break;
		case ALL:
		    printMembers(c.getConstructors(), "Constuctors");
		    printMembers(c.getFields(), "Fields");
		    printMembers(c.getMethods(), "Methods");
		    printClasses(c);
		    break;
		default:
		    assert false;
		}
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static void printMembers(Member[] mbrs, String s) {
	out.format("%s:%n", s);
	for (Member mbr : mbrs) {
	    if (mbr instanceof Field)
		out.format("  %s%n", ((Field)mbr).toGenericString());
	    else if (mbr instanceof Constructor)
		out.format("  %s%n", ((Constructor)mbr).toGenericString());
	    else if (mbr instanceof Method)
		out.format("  %s%n", ((Method)mbr).toGenericString());
	}
	if (mbrs.length == 0)
	    out.format("  -- No %s --%n", s);
	out.format("%n");
    }

    private static void printClasses(Class&lt;?&gt; c) {
	out.format("Classes:%n");
	Class&lt;?&gt;[] clss = c.getClasses();
	for (Class&lt;?&gt; cls : clss)
	    out.format("  %s%n", cls.getCanonicalName());
	if (clss.length == 0)
	    out.format("  -- No member interfaces, classes, or enums --%n");
	out.format("%n");
    }
}

```

This example is relatively compact; however the `printMembers()` method is slightly awkward due to the fact that the 
[`java.lang.reflect.Member`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Member.html) interface has existed since the earliest implementations of reflection and it could not be modified to include the more useful `getGenericString()` method when generics were introduced. The only alternatives are to test and cast as shown, replace this method with `printConstructors()`, `printFields()`, and `printMethods()`, or to be satisfied with the relatively spare results of 
[`Member.getName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Member.html#getName--).

Samples of the output and their interpretation follows. User input is in italics.

```

$ **java ClassSpy java.lang.ClassCastException CONSTRUCTOR**
Class:
  java.lang.ClassCastException

Package:
  java.lang

Constructor:
  public java.lang.ClassCastException()
  public java.lang.ClassCastException(java.lang.String)

```

Since constructors are not inherited, the exception chaining mechanism constructors (those with a 
[`Throwable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html) parameter) which are defined in the immediate super class 
[`RuntimeException`](https://docs.oracle.com/javase/8/docs/api/java/lang/RuntimeException.html) and other super classes are not found.

```

$ **java ClassSpy java.nio.channels.ReadableByteChannel METHOD**
Class:
  java.nio.channels.ReadableByteChannel

Package:
  java.nio.channels

Methods:
  public abstract int java.nio.channels.ReadableByteChannel.read
    (java.nio.ByteBuffer) throws java.io.IOException
  public abstract void java.nio.channels.Channel.close() throws
    java.io.IOException
  public abstract boolean java.nio.channels.Channel.isOpen()

```

The interface 
[`java.nio.channels.ReadableByteChannel`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/ReadableByteChannel.html) defines 
[`read()`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/ReadableByteChannel.html#read-java.nio.ByteBuffer-). The remaining methods are inherited from a super interface. This code could easily be modified to list only those methods that are actually declared in the class by replacing `get*s()` with `getDeclared*s()`.

```

$ **java ClassSpy ClassMember FIELD METHOD**
Class:
  ClassMember

Package:
  -- No Package --

Fields:
  public static final ClassMember ClassMember.CONSTRUCTOR
  public static final ClassMember ClassMember.FIELD
  public static final ClassMember ClassMember.METHOD
  public static final ClassMember ClassMember.CLASS
  public static final ClassMember ClassMember.ALL

Methods:
  public static ClassMember ClassMember.valueOf(java.lang.String)
  public static ClassMember[] ClassMember.values()
  public final int java.lang.Enum.hashCode()
  public final int java.lang.Enum.compareTo(E)
  public int java.lang.Enum.compareTo(java.lang.Object)
  public final java.lang.String java.lang.Enum.name()
  public final boolean java.lang.Enum.equals(java.lang.Object)
  public java.lang.String java.lang.Enum.toString()
  public static &lt;T&gt; T java.lang.Enum.valueOf
    (java.lang.Class&lt;T&gt;,java.lang.String)
  public final java.lang.Class&lt;E&gt; java.lang.Enum.getDeclaringClass()
  public final int java.lang.Enum.ordinal()
  public final native java.lang.Class&lt;?&gt; java.lang.Object.getClass()
  public final native void java.lang.Object.wait(long) throws
    java.lang.InterruptedException
  public final void java.lang.Object.wait(long,int) throws
    java.lang.InterruptedException
  public final void java.lang.Object.wait() hrows java.lang.InterruptedException
  public final native void java.lang.Object.notify()
  public final native void java.lang.Object.notifyAll()

```

In the fields portion of these results, enum constants are listed. While these are technically fields, it might be useful to distinguish them from other fields. This example could be modified to use 
[`java.lang.reflect.Field.isEnumConstant()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isEnumConstant--) for this purpose. The 
[`<code>EnumSpy`</code>](../special/example/EnumSpy.java) example in a later section of this trail, [Examining Enums](../special/enumMembers.html), contains a possible implementation.

In the methods section of the output, observe that the method name includes the name of the declaring class. Thus, the `toString()` method is implemented by 
[`Enum`](https://docs.oracle.com/javase/8/docs/api/java/lang/Enum.html#toString--), not inherited from 
[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html). The code could be amended to make this more obvious by using 
[`Field.getDeclaringClass()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#getDeclaringClass--). The following fragment illustrates part of a potential solution.

```

if (mbr instanceof Field) {
    Field f = (Field)mbr;
    out.format("  %s%n", f.toGenericString());
    out.format("  -- declared in: %s%n", f.getDeclaringClass());
}

```
