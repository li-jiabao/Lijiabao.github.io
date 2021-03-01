
# Creating New Arrays

Just as in non-reflective code, reflection supports the ability to dynamically create arrays of arbitrary type and dimensions via 
[`java.lang.reflect.Array.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Array.html#newInstance-java.lang.Class-int-). Consider 
[`<code>ArrayCreator`</code>](example/ArrayCreator.java), a basic interpreter capable of dynamically creating arrays. The syntax that will be parsed is as follows:

```

fully_qualified_class_name variable_name[] = 
     { val1, val2, val3, ... }

```

Assume that the `fully_qualified_class_name` represents a class that has a constructor with a single 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) argument. The dimensions of the array are determined by the number of values provided. The following example will construct an instance of an array of `fully_qualified_class_name` and populate its values with instances given by `val1`, `val2`, etc. (This example assumes familiarity with 
[`Class.getConstructor()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class...-) and 
[`java.lang.reflect.Constructor.newInstance()`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html#newInstance-java.lang.Object...-). For a discussion of the reflection APIs for 
[`Constructor`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html) see the [Creating New Class Instances](../member/ctorInstance.html) section of this trail.)

```


import java.lang.reflect.Array;
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.util.regex.Pattern;
import java.util.regex.Matcher;
import java.util.Arrays;
import static java.lang.System.out;

public class ArrayCreator {
    private static String s = "java.math.BigInteger bi[] = { 123, 234, 345 }";
    private static Pattern p = Pattern.compile("^\\s*(\\S+)\\s*\\w+\\[\\].*\\{\\s*([^}]+)\\s*\\}");

    public static void main(String... args) {
        Matcher m = p.matcher(s);

        if (m.find()) {
            String cName = m.group(1);
            String[] cVals = m.group(2).split("[\\s,]+");
            int n = cVals.length;

            try {
                Class&lt;?&gt; c = Class.forName(cName);
                Object o = Array.newInstance(c, n);
                for (int i = 0; i &lt; n; i++) {
                    String v = cVals[i];
                    Constructor ctor = c.getConstructor(String.class);
                    Object val = ctor.newInstance(v);
                    Array.set(o, i, val);
                }

                Object[] oo = (Object[])o;
                out.format("%s[] = %s%n", cName, Arrays.toString(oo));

            // production code should handle these exceptions more gracefully
            } catch (ClassNotFoundException x) {
                x.printStackTrace();
            } catch (NoSuchMethodException x) {
                x.printStackTrace();
            } catch (IllegalAccessException x) {
                x.printStackTrace();
            } catch (InstantiationException x) {
                x.printStackTrace();
            } catch (InvocationTargetException x) {
                x.printStackTrace();
            }
        }
    }
}

```

```

$ **java ArrayCreator**
java.math.BigInteger [] = [123, 234, 345]

```

The above example shows one case where it may be desirable to create an array via reflection; namely if the component type is not known until runtime. In this case, the code uses 
[`Class.forName()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName--) to get a class for the desired component type and then calls a specific constructor to initialize each component of the array before setting the corresponding array value.
