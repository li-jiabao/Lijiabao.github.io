
# Retrieving and Parsing Constructor Modifiers

Because of the role of constructors in the language, fewer modifiers are meaningful than for methods:

- Access modifiers: `public`, `protected`, and `private`
- Annotations

The 
[`<code>ConstructorAccess`</code>](example/ConstructorAccess.java) example searches for constructors in a given class with the specified access modifier. It also displays whether the constructor is synthetic (compiler-generated) or of variable arity.

```


import java.lang.reflect.Constructor;
import java.lang.reflect.Modifier;
import java.lang.reflect.Type;
import static java.lang.System.out;

public class ConstructorAccess {
    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName(args[0]);
	    Constructor[] allConstructors = c.getDeclaredConstructors();
	    for (Constructor ctor : allConstructors) {
		int searchMod = modifierFromString(args[1]);
		int mods = accessModifiers(ctor.getModifiers());
		if (searchMod == mods) {
		    out.format("%s%n", ctor.toGenericString());
		    out.format("  [ synthetic=%-5b var_args=%-5b ]%n",
			       ctor.isSynthetic(), ctor.isVarArgs());
		}
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static int accessModifiers(int m) {
	return m &amp; (Modifier.PUBLIC | Modifier.PRIVATE | Modifier.PROTECTED);
    }

    private static int modifierFromString(String s) {
	if ("public".equals(s))               return Modifier.PUBLIC;
	else if ("protected".equals(s))       return Modifier.PROTECTED;
	else if ("private".equals(s))         return Modifier.PRIVATE;
	else if ("package-private".equals(s)) return 0;
	else return -1;
    }
}

```

There is not an explicit 
[`Modifier`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Modifier.html) constant which corresponds to "package-private" access, so it is necessary to check for the absence of all three access modifiers to identify a package-private constructor.

This output shows the private constructors in 
[`java.io.File`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html):

```

$ **java ConstructorAccess java.io.File private**
private java.io.File(java.lang.String,int)
  [ synthetic=false var_args=false ]
private java.io.File(java.lang.String,java.io.File)
  [ synthetic=false var_args=false ]

```

Synthetic constructors are rare; however the 
[`<code>SyntheticConstructor`</code>](example/SyntheticConstructor.java) example illustrates a typical situation where this may occur:

```


public class SyntheticConstructor {
    private SyntheticConstructor() {}
    class Inner {
	// Compiler will generate a synthetic constructor since
	// SyntheticConstructor() is private.
	Inner() { new SyntheticConstructor(); }
    }
}

```

```

$ **java ConstructorAccess SyntheticConstructor package-private**
SyntheticConstructor(SyntheticConstructor$1)
  [ synthetic=true  var_args=false ]

```

Since the inner class's constructor references the private constructor of the enclosing class, the compiler must generate a package-private constructor. The parameter type `SyntheticConstructor$1` is arbitrary and dependent on the compiler implementation. Code which depends on the presence of any synthetic or non-public class members may not be portable.

Constructors implement 
[`java.lang.reflect.AnnotatedElement`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html), which provides methods to retrieve runtime annotations with 
[`java.lang.annotation.RetentionPolicy.RUNTIME`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html#RUNTIME). For an example of obtaining annotations see the [Examining Class Modifiers and Types](../class/classModifiers.html) section.
