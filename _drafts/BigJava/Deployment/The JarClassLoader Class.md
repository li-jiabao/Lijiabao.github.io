
# The JarClassLoader Class

The <tt>JarClassLoader</tt> class extends <tt>java.net.URLClassLoader</tt>. As its name implies, <tt>URLClassLoader</tt> is designed to be used for loading classes and resources that are accessed by searching a set of URLs. The URLs can refer either to directories or to JAR files.

In addition to subclassing <tt>URLClassLoader</tt>, <tt>JarClassLoader</tt> also makes use of features in two other new JAR-related APIs, the <tt>java.util.jar</tt> package and the <tt>java.net.JarURLConnection</tt> class. In this section, we'll look in detail at the constructor and two methods of <tt>JarClassLoader</tt>.

## The JarClassLoader Constructor

The constructor takes an instance of <tt>java.net.URL</tt> as an argument. The URL passed to this constructor will be used elsewhere in <tt>JarClassLoader</tt> to find the JAR file from which classes are to be loaded.

```

public JarClassLoader(URL url) {
    super(new URL[] { url });
    this.url = url;
}

```

The <tt>URL</tt> object is passed to the constructor of the superclass, <tt>URLClassLoader</tt>, which takes a <tt>URL[]</tt> array, rather than a single <tt>URL</tt> instance, as an argument.

## The getMainClassName Method

Once a <tt>JarClassLoader</tt> object is constructed with the URL of a JAR-bundled application, it's going to need a way to determine which class in the JAR file is the application's entry point. That's the job of the <tt>getMainClassName</tt> method:

```

public String getMainClassName() throws IOException {
    URL u = new URL("jar", "", url + "!/");
    JarURLConnection uc = (JarURLConnection)u.openConnection();
    Attributes attr = uc.getMainAttributes();
    return attr != null
                   ? attr.getValue(Attributes.Name.MAIN_CLASS)
                   : null;
}

```

You may recall from a [previous lesson](run.html) that a JAR-bundled application's entry point is specified by the <tt>Main-Class</tt> header of the JAR file's manifest. To understand how <tt>getMainClassName</tt> accesses the <tt>Main-Class</tt> header value, let's look at the method in detail, paying special attention to the new JAR-handling features that it uses:

## The JarURLConnection class and JAR URLs

The <tt>getMainClassName</tt> method uses the JAR URL format specified by the <tt>java.net.JarURLConnection</tt> class. The syntax for the URL of a JAR file is as in this example:

```

jar:http://www.example.com/jarfile.jar!/

```

The terminating <tt>!/</tt> separator indicates that the URL refers to an entire JAR file. Anything following the separator refers to specific JAR-file contents, as in this example:

```

jar:http://www.example.com/jarfile.jar!/mypackage/myclass.class

```

The first line in the <tt>getMainClassName</tt> method is:

```

URL u = new URL("jar", "", url + "!/");

```

This statement constructs a new <tt>URL</tt> object representing a JAR URL, appending the <tt>!/</tt> separator to the URL that was used in creating the <tt>JarClassLoader</tt> instance.

## The java.net.JarURLConnection class

This class represents a communications link between an application and a JAR file. It has methods for accessing the JAR file's manifest. The second line of <tt>getMainClassName</tt> is:

```

JarURLConnection uc = (JarURLConnection)u.openConnection();

```

In this statement, <tt>URL</tt> instance created in the first line opens a <tt>URLConnection</tt>. The <tt>URLConnection</tt> instance is then cast to <tt>JarURLConnection</tt> so it can take advantage of <tt>JarURLConnection</tt>'s JAR-handling features.

## Fetching Manifest Attributes: java.util.jar.Attributes

With a <tt>JarURLConnection</tt> open to a JAR file, you can access the header information in the JAR file's manifest by using the <tt>getMainAttributes</tt> method of <tt>JarURLConnection</tt>. This method returns an instance of <tt>java.util.jar.Attributes</tt>, a class that maps header names in JAR-file manifests with their associated string values. The third line in <tt>getMainClassName</tt> creates an <tt>Attributes</tt> object:

```

Attributes attr = uc.getMainAttributes();

```

To get the value of the manifest's <tt>Main-Class</tt> header, the fourth line of <tt>getMainClassName</tt> invokes the <tt>Attributes.getValue</tt> method:

```

return attr != null
               ? attr.getValue(Attributes.Name.MAIN_CLASS)
               : null;

```

The method's argument, <tt>Attributes.Name.MAIN_CLASS</tt>, specifies that it's the value of the <tt>Main-Class</tt> header that you want. (The <tt>Attributes.Name</tt> class also provides static fields such as <tt>MANIFEST_VERSION</tt>, <tt>CLASS_PATH</tt>, and <tt>SEALED</tt> for specifying other standard manifest headers.)

## The invokeClass Method

We've seen how <tt>JarURLClassLoader</tt> can identify the main class in a JAR-bundled application. The last method to consider, <tt>JarURLClassLoader.invokeClass</tt>, enables that main class to be invoked to launch the JAR-bundled application:

```

public void invokeClass(String name, String[] args)
    throws ClassNotFoundException,
           NoSuchMethodException,
           InvocationTargetException
{
    Class c = loadClass(name);
    Method m = c.getMethod("main", new Class[] { args.getClass() });
    m.setAccessible(true);
    int mods = m.getModifiers();
    if (m.getReturnType() != void.class || !Modifier.isStatic(mods) ||
        !Modifier.isPublic(mods)) {
        throw new NoSuchMethodException("main");
    }
    try {
        m.invoke(null, new Object[] { args });
    } catch (IllegalAccessException e) {
        // This should not happen, as we have disabled access checks
    }
}

```

The <tt>invokeClass</tt> method takes two arguments: the name of the application's entry-point class and an array of string arguments to pass to the entry-point class's <tt>main</tt> method. First, the main class is loaded:

```

Class c = loadClass(name);

```

The <tt>loadClass</tt> method is inherited from <tt>java.lang.ClassLoader</tt>.

Once the main class is loaded, the reflection API of the <tt>java.lang.reflect</tt> package is used to pass the arguments to the class and launch it. You can refer to the tutorial on [The Reflection API](../../reflect/index.html) for a review of reflection.
