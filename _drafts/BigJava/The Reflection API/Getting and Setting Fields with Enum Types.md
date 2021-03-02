
# Troubleshooting

The following examples show problems which may be encountered when using enumerated types.

## IllegalArgumentException When Attempting to Instantiate an Enum Type

As has been mentioned, instantiation of enum types is forbidden. The 
[`<code>EnumTrouble`</code>](example/EnumTrouble.java) example attempts this.

```


import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import static java.lang.System.out;

enum Charge {
    POSITIVE, NEGATIVE, NEUTRAL;
    Charge() {
	out.format("under construction%n");
    }
}

public class EnumTrouble {

    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Charge.class;

 	    Constructor[] ctors = c.getDeclaredConstructors();
 	    for (Constructor ctor : ctors) {
		out.format("Constructor: %s%n",  ctor.toGenericString());
 		ctor.setAccessible(true);
 		ctor.newInstance();
 	    }

        // production code should handle these exceptions more gracefully
	} catch (InstantiationException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	} catch (InvocationTargetException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java EnumTrouble**
Constructor: private Charge()
Exception in thread "main" java.lang.IllegalArgumentException: Cannot
  reflectively create enum objects
        at java.lang.reflect.Constructor.newInstance(Constructor.java:511)
        at EnumTrouble.main(EnumTrouble.java:22)

```

## IllegalArgumentException when Setting a Field with an Incompatible Enum Type

Fields storing enums set with the appropriate enum type. (Actually, fields of **any** type must be set with compatible types.) The 
[`<code>EnumTroubleToo`</code>](example/EnumTroubleToo.java) example produces the expected error.

```


import java.lang.reflect.Field;

enum E0 { A, B }
enum E1 { A, B }

class ETest {
    private E0 fld = E0.A;
}

public class EnumTroubleToo {
    public static void main(String... args) {
	try {
	    ETest test = new ETest();
	    Field f = test.getClass().getDeclaredField("fld");
	    f.setAccessible(true);
 	    f.set(test, E1.A);  // IllegalArgumentException

        // production code should handle these exceptions more gracefully
	} catch (NoSuchFieldException x) {
	    x.printStackTrace();
	} catch (IllegalAccessException x) {
	    x.printStackTrace();
	}
    }
}

```

```

$ **java EnumTroubleToo**
Exception in thread "main" java.lang.IllegalArgumentException: Can not set E0
  field ETest.fld to E1
        at sun.reflect.UnsafeFieldAccessorImpl.throwSetIllegalArgumentException
          (UnsafeFieldAccessorImpl.java:146)
        at sun.reflect.UnsafeFieldAccessorImpl.throwSetIllegalArgumentException
          (UnsafeFieldAccessorImpl.java:150)
        at sun.reflect.UnsafeObjectFieldAccessorImpl.set
          (UnsafeObjectFieldAccessorImpl.java:63)
        at java.lang.reflect.Field.set(Field.java:657)
        at EnumTroubleToo.main(EnumTroubleToo.java:16)

```

```

X.class.isAssignableFrom(Y.class) == true

```

```

if (f.getType().isAssignableFrom(E0.class))
    // compatible
else
    // expect IllegalArgumentException

```
