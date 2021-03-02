
# Examining Enums

Reflection provides three enum-specific APIs:

Sometimes it is necessary to dynamically retrieve the list of enum constants; in non-reflective code this is accomplished by invoking the implicitly declared static method 
[`values()`](https://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html) on the enum. If an instance of an enum type is not available the only way to get a list of the possible values is to invoke 
[`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--) since it is impossible to instantiate an enum type.

Given a fully qualified name, the 
[`<code>EnumConstants`</code>](example/EnumConstants.java) example shows how to retrieve an ordered list of constants in an enum using 
[`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--).

```


import java.util.Arrays;
import static java.lang.System.out;

enum Eon { HADEAN, ARCHAEAN, PROTEROZOIC, PHANEROZOIC }

public class EnumConstants {
    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = (args.length == 0 ? Eon.class : Class.forName(args[0]));
	    out.format("Enum name:  %s%nEnum constants:  %s%n",
		       c.getName(), Arrays.asList(c.getEnumConstants()));
	    if (c == Eon.class)
		out.format("  Eon.values():  %s%n",
			   Arrays.asList(Eon.values()));

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }
}

```

Samples of the output follows. User input is in italics.

```

$ **java EnumConstants java.lang.annotation.RetentionPolicy**
Enum name:  java.lang.annotation.RetentionPolicy
Enum constants:  [SOURCE, CLASS, RUNTIME]

```

```

$ **java EnumConstants java.util.concurrent.TimeUnit**
Enum name:  java.util.concurrent.TimeUnit
Enum constants:  [NANOSECONDS, MICROSECONDS, 
                  MILLISECONDS, SECONDS, 
                  MINUTES, HOURS, DAYS]

```

This example also shows that value returned by 
[`Class.getEnumConstants()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants--) is identical to the value returned by invoking `values()` on an enum type.

```

$ **java EnumConstants**
Enum name:  Eon
Enum constants:  [HADEAN, ARCHAEAN, 
                  PROTEROZOIC, PHANEROZOIC]
Eon.values():  [HADEAN, ARCHAEAN, 
                PROTEROZOIC, PHANEROZOIC]

```

Since enums are classes, other information may be obtained using the same Reflection APIs described in the [Fields](../member/field.html), [Methods](../member/method.html), and [Constructors](../member/ctor.html) sections of this trail. The 
[`<code>EnumSpy`</code>](example/EnumSpy.java) code illustrates how to use these APIs to get additional information about the enum's declaration. The example uses 
[`Class.isEnum()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isEnum--) to restrict the set of classes examined. It also uses 
[`Field.isEnumConstant()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html#isEnumConstant--) to distinguish enum constants from other fields in the enum declaration (not all fields are enum constants).

```


import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;
import java.lang.reflect.Member;
import java.util.List;
import java.util.ArrayList;
import static java.lang.System.out;

public class EnumSpy {
    private static final String fmt = "  %11s:  %s %s%n";

    public static void main(String... args) {
	try {
	    Class&lt;?&gt; c = Class.forName(args[0]);
	    if (!c.isEnum()) {
		out.format("%s is not an enum type%n", c);
		return;
	    }
	    out.format("Class:  %s%n", c);

	    Field[] flds = c.getDeclaredFields();
	    List&lt;Field&gt; cst = new ArrayList&lt;Field&gt;();  // enum constants
	    List&lt;Field&gt; mbr = new ArrayList&lt;Field&gt;();  // member fields
	    for (Field f : flds) {
		if (f.isEnumConstant())
		    cst.add(f);
		else
		    mbr.add(f);
	    }
	    if (!cst.isEmpty())
		print(cst, "Constant");
	    if (!mbr.isEmpty())
		print(mbr, "Field");

	    Constructor[] ctors = c.getDeclaredConstructors();
	    for (Constructor ctor : ctors) {
		out.format(fmt, "Constructor", ctor.toGenericString(),
			   synthetic(ctor));
	    }

	    Method[] mths = c.getDeclaredMethods();
	    for (Method m : mths) {
		out.format(fmt, "Method", m.toGenericString(),
			   synthetic(m));
	    }

        // production code should handle this exception more gracefully
	} catch (ClassNotFoundException x) {
	    x.printStackTrace();
	}
    }

    private static void print(List&lt;Field&gt; lst, String s) {
	for (Field f : lst) {
 	    out.format(fmt, s, f.toGenericString(), synthetic(f));
	}
    }

    private static String synthetic(Member m) {
	return (m.isSynthetic() ? "[ synthetic ]" : "");
    }
}

```

```

$ **java EnumSpy java.lang.annotation.RetentionPolicy**
Class:  class java.lang.annotation.RetentionPolicy
     Constant:  public static final java.lang.annotation.RetentionPolicy
                  java.lang.annotation.RetentionPolicy.SOURCE 
     Constant:  public static final java.lang.annotation.RetentionPolicy
                  java.lang.annotation.RetentionPolicy.CLASS 
     Constant:  public static final java.lang.annotation.RetentionPolicy 
                  java.lang.annotation.RetentionPolicy.RUNTIME 
        Field:  private static final java.lang.annotation.RetentionPolicy[] 
                  java.lang.annotation.RetentionPolicy. [ synthetic ]
  Constructor:  private java.lang.annotation.RetentionPolicy() 
       Method:  public static java.lang.annotation.RetentionPolicy[]
                  java.lang.annotation.RetentionPolicy.values() 
       Method:  public static java.lang.annotation.RetentionPolicy
                  java.lang.annotation.RetentionPolicy.valueOf(java.lang.String) 

```

The output shows that declaration of 
[`java.lang.annotation.RetentionPolicy`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html) only contains the three enum constants. The enum constants are exposed as `public static final` fields. The field, constructor, and methods are compiler generated. The `$VALUES` field is related to the implementation of the `values()` method.

The output for 
[`java.util.concurrent.TimeUnit`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/TimeUnit.html) shows that much more complicated enums are possible. This class includes several methods as well as additional fields declared `static final` which are not enum constants.

```

$ java EnumSpy java.util.concurrent.TimeUnit
Class:  class java.util.concurrent.TimeUnit
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.NANOSECONDS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.MICROSECONDS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.MILLISECONDS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.SECONDS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.MINUTES
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.HOURS
     Constant:  public static final java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.DAYS
        Field:  static final long java.util.concurrent.TimeUnit.C0
        Field:  static final long java.util.concurrent.TimeUnit.C1
        Field:  static final long java.util.concurrent.TimeUnit.C2
        Field:  static final long java.util.concurrent.TimeUnit.C3
        Field:  static final long java.util.concurrent.TimeUnit.C4
        Field:  static final long java.util.concurrent.TimeUnit.C5
        Field:  static final long java.util.concurrent.TimeUnit.C6
        Field:  static final long java.util.concurrent.TimeUnit.MAX
        Field:  private static final java.util.concurrent.TimeUnit[] 
                  java.util.concurrent.TimeUnit. [ synthetic ]
  Constructor:  private java.util.concurrent.TimeUnit()
  Constructor:  java.util.concurrent.TimeUnit
                  (java.lang.String,int,java.util.concurrent.TimeUnit)
                  [ synthetic ]
       Method:  public static java.util.concurrent.TimeUnit
                  java.util.concurrent.TimeUnit.valueOf(java.lang.String)
       Method:  public static java.util.concurrent.TimeUnit[] 
                  java.util.concurrent.TimeUnit.values()
       Method:  public void java.util.concurrent.TimeUnit.sleep(long) 
                  throws java.lang.InterruptedException
       Method:  public long java.util.concurrent.TimeUnit.toNanos(long)
       Method:  public long java.util.concurrent.TimeUnit.convert
                  (long,java.util.concurrent.TimeUnit)
       Method:  abstract int java.util.concurrent.TimeUnit.excessNanos
                  (long,long)
       Method:  public void java.util.concurrent.TimeUnit.timedJoin
                  (java.lang.Thread,long) throws java.lang.InterruptedException
       Method:  public void java.util.concurrent.TimeUnit.timedWait
                  (java.lang.Object,long) throws java.lang.InterruptedException
       Method:  public long java.util.concurrent.TimeUnit.toDays(long)
       Method:  public long java.util.concurrent.TimeUnit.toHours(long)
       Method:  public long java.util.concurrent.TimeUnit.toMicros(long)
       Method:  public long java.util.concurrent.TimeUnit.toMillis(long)
       Method:  public long java.util.concurrent.TimeUnit.toMinutes(long)
       Method:  public long java.util.concurrent.TimeUnit.toSeconds(long)
       Method:  static long java.util.concurrent.TimeUnit.x(long,long,long)

```
