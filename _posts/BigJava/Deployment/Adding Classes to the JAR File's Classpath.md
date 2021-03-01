
# Adding Classes to the JAR File's Classpath

You may need to reference classes in other JAR files from within a JAR file.

For example, in a typical situation an applet is bundled in a JAR file whose manifest references a different JAR file (or several different JAR files) that serves as utilities for the purposes of that applet.

You specify classes to include in the <tt>Class-Path</tt> header field in the manifest file of an applet or application. The <tt>Class-Path</tt> header takes the following form:

```

Class-Path: **jar1-name jar2-name directory-name/jar3-name**

```

By using the <tt>Class-Path</tt> header in the manifest, you can avoid having to specify a long <tt>-classpath</tt> flag when invoking Java to run the your application.

## An Example

We want to load classes in <tt>MyUtils.jar</tt> into the class path for use in <tt>MyJar.jar</tt>. These two JAR files are in the same directory.

We first create a text file named <tt>Manifest.txt</tt> with the following contents:

```

Class-Path: MyUtils.jar

```

We then create a JAR file named <tt>MyJar.jar</tt> by entering the following command:

```

jar cfm MyJar.jar Manifest.txt MyPackage/*.class

```

This creates the JAR file with a manifest with the following contents:

```

Manifest-Version: 1.0
Class-Path: MyUtils.jar
Created-By: 1.7.0_06 (Oracle Corporation)

```

The classes in <tt>MyUtils.jar</tt> are now loaded into the class path when you run <tt>MyJar.jar</tt>.
