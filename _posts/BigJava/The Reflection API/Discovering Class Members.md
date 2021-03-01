
# Troubleshooting

The following examples show typical errors which may be encountered when reflecting on classes.

## Compiler Warning: "Note: ... uses unchecked or unsafe operations"

When a method is invoked, the types of the argument values are checked and possibly converted. 
[`<code>ClassWarning`</code>](example/ClassWarning.java) invokes 
[`getMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class...-) to cause a typical unchecked conversion warning:

```


import java.lang.reflect.Method;

public class ClassWarning {
    void m() {
	try {
	    Class c = ClassWarning.class;
	    Method m = c.getMethod("m");  // warning

        // production code should handle this exception more gracefully
	} catch (NoSuchMethodException x) {
    	    x.printStackTrace();
    	}
    }
}

```

```

$ **javac ClassWarning.java**
Note: ClassWarning.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
$ **javac -Xlint:unchecked ClassWarning.java**
ClassWarning.java:6: warning: [unchecked] unchecked call to getMethod
  (String,Class&lt;?&gt;...) as a member of the raw type Class
Method m = c.getMethod("m");  // warning
                      ^
1 warning

```

Many library methods have been retrofitted with generic declarations including several in 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html). Since `c` is declared as a **raw** type (has no type parameters) and the corresponding parameter of 
[`getMethod()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class...-) is a parameterized type, an unchecked conversion occurs. The compiler is required to generate a warning. (See [**The Java Language Specification, Java SE 7 Edition**](https://docs.oracle.com/javase/specs/jls/se7/html/index.html), sections [Unchecked Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.9) and [Method Invocation Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.3).)

There are two possible solutions. The more preferable it to modify the declaration of `c` to include an appropriate generic type. In this case, the declaration should be:

```

Class&lt;?&gt; c = warn.getClass();

```

Alternatively, the warning could be explicitly suppressed using the predefined annotation 
[`@SuppressWarnings`](https://docs.oracle.com/javase/8/docs/api/java/lang/SuppressWarnings.html) preceding the problematic statement.

```

Class c = ClassWarning.class;
@SuppressWarnings("unchecked")
Method m = c.getMethod("m");  
// warning gone

```

## InstantiationException when the Constructor is Not Accessible


[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--) will throw an 
[`InstantiationException`](https://docs.oracle.com/javase/8/docs/api/java/lang/InstantiationException.html) if an attempt is made to create a new instance of the class and the zero-argument constructor is not visible. The 
[`<code>ClassTrouble`</code>](example/ClassTrouble.java) example illustrates the resulting stack trace.

```


class Cls {
    private Cls() {}
}

public class ClassTrouble {
    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName("Cls");
	    c.newInstance();  // InstantiationException

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java ClassTrouble**
java.lang.IllegalAccessException: Class ClassTrouble can not access a member of
  class Cls with modifiers "private"
        at sun.reflect.Reflection.ensureMemberAccess(Reflection.java:65)
        at java.lang.Class.newInstance0(Class.java:349)
        at java.lang.Class.newInstance(Class.java:308)
        at ClassTrouble.main(ClassTrouble.java:9)

```


[`Class.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance--) behaves very much like the `new` keyword and will fail for the same reasons `new` would fail. The typical solution in reflection is to take advantage of the 
[`java.lang.reflect.AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html) class which provides the ability to suppress access control checks; however, this approach will not work because 
[`java.lang.Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) does not extend 
[`AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html). The only solution is to modify the code to use 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) which does extend 
[`AccessibleObject`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AccessibleObject.html).

Additional examples of potential problems using 
[`Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-) may be found in the [Constructor Troubleshooting](../member/ctorTrouble.html) section of the [Members](../member/index.html) lesson.
