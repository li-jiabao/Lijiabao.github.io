---
title: The JarRunner Class
author: lijiabao
date: 2020-12-06 12:46:11.102926000 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# The JarRunner Class

The JarRunner application is launched with a command of this form:

```

java JarRunner *url [arguments]*

```

In the previous section, we've seen how <tt>JarClassLoader</tt> is able to identify and load the main class of a JAR-bundled application from a given URL. To complete the JarRunner application, therefore, we need to be able to take a URL and any arguments from the command line, and pass them to an instance of <tt>JarClassLoader</tt>. These tasks belong to the <tt>JarRunner</tt> class, the entry point of the JarRunner application.

It begins by creating a <tt>java.net.URL</tt> object from the URL specified on the command line:

```

public static void main(String[] args) {
    if (args.length &lt; 1) {
        usage();
    }
    URL url = null;
    try {
        url = new URL(args[0]);
    } catch (MalformedURLException e) {
        fatal("Invalid URL: " + args[0]);
    }

```

If <tt>args.length&#160;&lt;&#160;1</tt>, that means no URL was specified on the command line, so a usage message is printed. If the first command-line argument is a good URL, a new <tt>URL</tt> object is created to represent it.

Next, JarRunner creates a new instance of <tt>JarClassLoader</tt>, passing to the constructor the URL that was specified on the command-line:

```

JarClassLoader cl = new JarClassLoader(url);

```

As we saw in the previous section, it's through <tt>JarClassLoader</tt> that JarRunner taps into the JAR-handling APIs.

The URL that's passed to the <tt>JarClassLoader</tt> constructor is the URL of the JAR-bundled application that you want to run. JarRunner next calls the class loader's <tt>getMainClassName</tt> method to identify the entry-point class for the application:

```

String name = null;
try {
    **name = cl.getMainClassName();**
} catch (IOException e) {
    System.err.println("I/O error while loading JAR file:");
    e.printStackTrace();
    System.exit(1);
}
if (name == null) {
    fatal("Specified jar file does not contain a 'Main-Class'" +
          " manifest attribute");
}

```

The key statement is highlighted in bold. The other statements are for error handling.

Once <tt>JarRunner</tt> has identified the application's entry-point class, only two steps remain: passing any arguments to the application and actually launching the application. <tt>JarRunner</tt> performs these steps with this code:

```

// Get arguments for the application
String[] newArgs = new String[args.length - 1];
**System.arraycopy(args, 1, newArgs, 0, newArgs.length);**
// Invoke application's main class
try {
    cl.invokeClass(name, newArgs);
} catch (ClassNotFoundException e) {
    fatal("Class not found: " + name);
} catch (NoSuchMethodException e) {
    fatal("Class does not define a 'main' method: " + name);
} catch (InvocationTargetException e) {
    e.getTargetException().printStackTrace();
    System.exit(1);
}

```

Recall that the first command-line argument was the URL of the JAR-bundled application. Any arguments to be passed to that application are therefore in element <tt>1</tt> and beyond in the <tt>args</tt> array. <tt>JarRunner</tt> takes those elements, and creates a new array called <tt>newArgs</tt> to pass to the application (bold line above). <tt>JarRunner</tt> then passes the entry-point's class name and the new argument list to the <tt>invokeClass</tt> method of <tt>JarClassLoader</tt>. As we saw in the previous section, <tt>invokeClass</tt> will load the application's entry-point class, pass it any arguments, and launch the application.
