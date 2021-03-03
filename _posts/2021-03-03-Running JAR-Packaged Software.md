---
title: Running JAR-Packaged Software
author: lijiabao
date: 2020-12-06 12:45:30.662375400 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Running JAR-Packaged Software

Now that you have learned how to create JAR files, how do you actually run the code you packaged? Consider these scenarios:

- Your JAR file contains an applet that is to be run inside a browser.
- Your JAR file contains an application that is to be started from the command line.
- Your JAR file contains code that you want to use as an extension.

This section will cover the first two situations. A separate trail in the tutorial on the 
[extension mechanism](../../ext/index.html) covers the use of JAR files as extensions.

## Applets Packaged in JAR Files

To start any applet from an HTML file for running inside a browser, you use the <tt>applet</tt> tag. For more information, see the 
[Java Applets](../applet/index.html) lesson. If the applet is bundled as a JAR file, the only thing you need to do differently is to use the **archive** parameter to specify the relative path to the JAR file.

As an example, use the TicTacToe demo applet. The <tt>applet</tt> tag in the HTML file that displays the applet can be marked up like this:

```

&lt;applet code=TicTacToe.class 
        width="120" height="120"&gt;
&lt;/applet&gt;

```

If the TicTacToe demo was packaged in a JAR file named <tt>TicTacToe.jar</tt>, you can modify the <tt>applet</tt> tag with the addition of an <tt>archive</tt> parameter:

```

&lt;applet code=TicTacToe.class 
        archive="TicTacToe.jar"
        width="120" height="120"&gt;
&lt;/applet&gt;

```

The <tt>archive</tt> parameter specifies the relative path to the JAR file that contains <tt>TicTacToe.class</tt>. For this example it is assumed that the JAR file and the HTML file are in the same directory. If they are not, you must include the JAR file's relative path in the <tt>archive</tt> parameter's value. For example, if the JAR file was one directory below the HTML file in a directory called <tt>applets</tt>, the <tt>applet</tt> tag would look like this:

```

&lt;applet code=TicTacToe.class 
        archive="applets/TicTacToe.jar"
        width="120" height="120"&gt;
&lt;/applet&gt;

```

## JAR Files as Applications

You can run JAR packaged applications with the Java launcher (<tt>java</tt> command). The basic command is:

```

java -jar *jar-file*

```

The <tt>-jar</tt> flag tells the launcher that the application is packaged in the JAR file format. You can only specify one JAR file, which must contain all of the application-specific code.

Before you execute this command, make sure that the runtime environment has information about which class within the JAR file is the application's entry point.

To indicate which class is the application's entry point, you must add a <tt>Main-Class</tt> header to the JAR file's manifest. The header takes the form:

```

Main-Class: *classname*

```

The header's value, <tt>classname</tt>, is the name of the class that is the application's entry point.

For more information, see the 
[Setting an Application's Entry Point](appman.html) section.

When the <tt>Main-Class</tt> is set in the manifest file, you can run the application from the command line:

```

java -jar app.jar

```

To run the application from the JAR file that is in another directory, you must specify the path of that directory:
`java -jar path/app.jar`
