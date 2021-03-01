
# Troubleshooting

The following problems are sometimes encountered by developers when trying to invoke constructors via reflection.

## InstantiationException Due to Missing Zero-Argument Constructor

The 
[`ConstructorTrouble`](example/ConstructorTrouble.java) example illustrates what happens when code attempts to create a new instance of a class using 
[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--) and there is no accessible zero-argument constructor:

```


public class ConstructorTrouble {
    private ConstructorTrouble(int i) {}

    public static void main(String... args){
	try {
	    Class&lt;?&gt; c = Class.forName("ConstructorTrouble");
	    Object o = c.newInstance();  // InstantiationException

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java ConstructorTrouble**
java.lang.InstantiationException: ConstructorTrouble
        at java.lang.Class.newInstance0(Class.java:340)
        at java.lang.Class.newInstance(Class.java:308)
        at ConstructorTrouble.main(ConstructorTrouble.java:7)

```

## Class.newInstance() Throws Unexpected Exception

The 
[`ConstructorTroubleToo`](example/ConstructorTroubleToo.java) example shows an unresolvable problem in 
[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--). Namely, it propagates any exception &#8212; checked or unchecked &#8212; thrown by the constructor.

```


import java.lang.reflect.InvocationTargetException;
import static java.lang.System.err;

public class ConstructorTroubleToo {
    public ConstructorTroubleToo() {
 	throw new RuntimeException("exception in constructor");
    }

    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName("ConstructorTroubleToo");
	    // Method propagetes any exception thrown by the constructor
	    // (including checked exceptions).
	    if (args.length &gt; 0 &amp;&amp; args[0].equals("class")) {
		Object o = c.newInstance();
	    } else {
		Object o = c.getConstructor().newInstance();
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	    err.format("%n%nCaught exception: %s%n", x.getCause());
	}
    }
}

```

```

$ **java ConstructorTroubleToo class**
Exception in thread "main" java.lang.RuntimeException: exception in constructor
        at ConstructorTroubleToo.&lt;init&gt;(ConstructorTroubleToo.java:6)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance
          (NativeConstructorAccessorImpl.java:39)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance
          (DelegatingConstructorAccessorImpl.java:27)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:513)
        at java.lang.Class.newInstance0(Class.java:355)
        at java.lang.Class.newInstance(Class.java:308)
        at ConstructorTroubleToo.main(ConstructorTroubleToo.java:15)

```

This situation is unique to reflection. Normally, it is impossible to write code which ignores a checked exception because it would not compile. It is possible to wrap any exception thrown by a constructor by using 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) rather than 
[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--).

```

$ **java ConstructorTroubleToo**
java.lang.reflect.InvocationTargetException
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance
          (NativeConstructorAccessorImpl.java:39)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance
          (DelegatingConstructorAccessorImpl.java:27)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:513)
        at ConstructorTroubleToo.main(ConstructorTroubleToo.java:17)
Caused by: java.lang.RuntimeException: exception in constructor
        at ConstructorTroubleToo.&lt;init&gt;(ConstructorTroubleToo.java:6)
        ... 5 more


Caught exception: java.lang.RuntimeException: exception in constructor

```

If an 
[`InvocationTargetException`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html) is thrown, the method was invoked. Diagnosis of the problem would be the same as if the constructor was called directly and threw the exception that is retrieved by 
[`InvocationTargetException.getCause()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/InvocationTargetException.html#getCause--). This exception does not indicate a problem with the reflection package or its usage.

## Problems Locating or Invoking the Correct Constructor

The 
[`ConstructorTroubleAgain`](example/ConstructorTroubleAgain.java) class illustrates various ways in which incorrect code can fail to locate or invoke the expected constructor.

```


import java.lang.reflect.InvocationTargetException;
import static java.lang.System.out;

public class ConstructorTroubleAgain {
    public ConstructorTroubleAgain() {}

    public ConstructorTroubleAgain(Integer i) {}

    public ConstructorTroubleAgain(Object o) {
	out.format("Constructor passed Object%n");
    }

    public ConstructorTroubleAgain(String s) {
	out.format("Constructor passed String%n");
    }

    public static void main(String... args){
	String argType = (args.length == 0 ? "" : args[0]);
	try {
	    Class&lt;?&gt; c = Class.forName("ConstructorTroubleAgain");
	    if ("".equals(argType)) {
		// IllegalArgumentException: wrong number of arguments
		Object o = c.getConstructor().newInstance("foo");
	    } else if ("int".equals(argType)) {
		// NoSuchMethodException - looking for int, have Integer
		Object o = c.getConstructor(int.class);
	    } else if ("Object".equals(argType)) {
		// newInstance() does not perform method resolution
		Object o = c.getConstructor(Object.class).newInstance("foo");
	    } else {
		assert false;
	    }

        // production code should handle these exceptions more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java ConstructorTroubleAgain**
Exception in thread "main" java.lang.IllegalArgumentException: wrong number of
  arguments
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance
          (NativeConstructorAccessorImpl.java:39)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance
          (DelegatingConstructorAccessorImpl.java:27)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:513)
        at ConstructorTroubleAgain.main(ConstructorTroubleAgain.java:23)

```

An 
[`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html) is thrown because the zero-argument constructor was requested and an attempt was made to pass an argument. The same exception would be thrown if the constructor was passed an argument of the wrong type.

```

$ **java ConstructorTroubleAgain int**
java.lang.NoSuchMethodException: ConstructorTroubleAgain.&lt;init&gt;(int)
        at java.lang.Class.getConstructor0(Class.java:2706)
        at java.lang.Class.getConstructor(Class.java:1657)
        at ConstructorTroubleAgain.main(ConstructorTroubleAgain.java:26)

```

This exception may occur if the developer mistakenly believes that reflection will autobox or unbox types. Boxing (conversion of a primitive to a reference type) occurs only during compilation. There is no opportunity in reflection for this operation to occur, so the specific type must be used when locating a constructor.

```

$ **java ConstructorTroubleAgain Object**
Constructor passed Object

```

Here, it might be expected that the constructor taking a 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) argument would be invoked since `newInstance()` was invoked with the more specific `String` type. However it is too late! The constructor which was found is already the constructor with an 
[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) argument. `newInstance()` makes no attempt to do method resolution; it simply operates on the existing constructor object.

## IllegalAccessException When Attempting to Invoke an Inaccessible Constructor

An 
[`IllegalAccessException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalAccessException.html) may be thrown if an attempt is made to invoke a private or otherwise inaccessible constructor. The 
[`ConstructorTroubleAccess`](example/ConstructorTroubleAccess.java) example illustrates the resulting stack trace.

```


import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

class Deny {
    private Deny() {
	System.out.format("Deny constructor%n");
    }
}

public class ConstructorTroubleAccess {
    public static void main(String... args) {
	try {
	    Constructor c = Deny.class.getDeclaredConstructor();
//  	    c.setAccessible(true);   // solution
	    c.newInstance();

        // production code should handle these exceptions more gracefully
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	} catch (NoSuchMethodException x) {
	    x.printStackTrace();
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java ConstructorTroubleAccess**
java.lang.IllegalAccessException: Class ConstructorTroubleAccess can not access
  a member of class Deny with modifiers "private"
        at sun.reflect.Reflection.ensureMemberAccess(Reflection.java:65)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:505)
        at ConstructorTroubleAccess.main(ConstructorTroubleAccess.java:15)

```
