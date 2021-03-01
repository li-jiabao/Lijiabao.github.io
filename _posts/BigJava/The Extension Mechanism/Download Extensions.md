
# Understanding Extension Class Loading

The extension framework makes use of the class-loading delegation mechanism. When the runtime environment needs to load a new class for an application, it looks for the class in the following locations, in order:

1. **Bootstrap classes**: the runtime classes in <tt>rt.jar</tt>, internationalization classes in <tt>i18n.jar</tt>, and others.
1. **Installed extensions**: classes in JAR files in the <tt>lib/ext</tt> directory of the JRE, and in the system-wide, platform-specific extension directory (such as <tt>/usr/jdk/packages/lib/ext</tt> on the Solaris&#8482; Operating System, but note that use of this directory applies only to Java&#8482; 6 and later).
1. **The class path**: classes, including classes in JAR files, on paths specified by the system property <tt>java.class.path</tt>. If a JAR file on the class path has a manifest with the `Class-Path` attribute, JAR files specified by the `Class-Path` attribute will be searched also. By default, the `java.class.path` property's value is `.`, the current directory. You can change the value by using the <tt>-classpath</tt> or <tt>-cp</tt> command-line options, or setting the `CLASSPATH` environment variable. The command-line options override the setting of the `CLASSPATH` environment variable.

The precedence list tells you, for example, that the class path is searched only if a class to be loaded hasn't been found among the classes in <tt>rt.jar</tt>, <tt>i18n.jar</tt> or the installed extensions.

Unless your software instantiates its own class loaders for special purposes, you don't really need to know much more than to keep this precedence list in mind. In particular, you should be aware of any class name conflicts that might be present. For example, if you list a class on the class path, you'll get unexpected results if the runtime environment instead loads another class of the same name that it found in an installed extension.

## The Java Class Loading Mechanism

The Java platform uses a delegation model for loading classes. The basic idea is that every class loader has a "parent" class loader. When loading a class, a class loader first "delegates" the search for the class to its parent class loader before attempting to find the class itself.

Here are some highlights of the class-loading API:

- Constructors in <tt>java.lang.ClassLoader</tt> and its subclasses allow you to specify a parent when you instantiate a new class loader. If you don't explicitly specify a parent, the virtual machine's system class loader will be assigned as the default parent.
<li>The <tt>loadClass</tt> method in <tt>ClassLoader</tt> performs these tasks, in order, when called to load a class:
<ol>
- If a class has already been loaded, it returns it.
- Otherwise, it delegates the search for the new class to the parent class loader.
- If the parent class loader does not find the class, <tt>loadClass</tt> calls the method <tt>findClass</tt> to find and load the class.
</ol>
</li>
- The <tt>findClass</tt> method of <tt>ClassLoader</tt> searches for the class in the current class loader if the class wasn't found by the parent class loader. You will probably want to override this method when you instantiate a class loader subclass in your application.
- The class <tt>java.net.URLClassLoader</tt> serves as the basic class loader for extensions and other JAR files, overriding the <tt>findClass</tt> method of <tt>java.lang.ClassLoader</tt> to search one or more specified URLs for classes and resources.

To see a sample application that uses some of the API as it relates to JAR files, see the 

[Using JAR-related APIs](../../deployment/jar/apiindex.html)
 lesson in this tutorial.

## Class Loading and the <tt>java</tt> Command

The Java platform's class-loading mechanism is reflected in the <tt>java</tt> command.

- In the <tt>java</tt> tool, the <tt>-classpath</tt> option is a shorthand way to set the <tt>java.class.path</tt> property.
- The <tt>-cp</tt> and <tt>-classpath</tt> options are equivalent.
<li>The <tt>-jar</tt> option runs applications that are packaged in JAR files. For a description and examples of this option, see the 

[Running JAR-Packaged Software](../../deployment/jar/run.html) lesson in this tutorial.
</li>
