
# Common Java Web Start Problems

This section covers some common problems that you might encounter when developing and deploying Java Web Start applications. After each problem is a list of possible reasons and solutions.

**Problem:** My browser shows the Java Network Launch Protocol (JNLP) file for my application as plain text.

Most likely, your web server is not aware of the proper MIME type for JNLP files. See the 
[Setting up the web server](settingUpWebServerMimeType.html) section for more information.

Furthermore, if you are using a proxy server, ensure that the update versions of the files are returned, by updating the time stamp of the resources on the web server such that the proxies will update their caches.

**Problem:** When I try to launch my JNLP file, I get the following error:

```

MissingFieldException[ The following required field is missing from the launch
  file: (&lt;application-desc&gt;|&lt;applet-desc&gt;|&lt;installer-desc&gt;|&lt;component-desc&gt;)]
        at com.sun.javaws.jnl.XMLFormat.parse(Unknown Source)
        at com.sun.javaws.jnl.LaunchDescFactory.buildDescriptor(Unknown Source)
        at com.sun.javaws.jnl.LaunchDescFactory.buildDescriptor(Unknown Source)
        at com.sun.javaws.jnl.LaunchDescFactory.buildDescriptor(Unknown Source)
        at com.sun.javaws.Main.launchApp(Unknown Source)
        at com.sun.javaws.Main.continueInSecureThread(Unknown Source)
        at com.sun.javaws.Main.run(Unknown Source)
        at java.lang.Thread.run(Unknown Source)

```

Often this error occurs when your XML is malformed. You can stare at the code until you figure it out but it is easier to run an XML syntax checker over the file. (NetBeans IDE and jEdit both provide XML syntax checkers.)

However, this error can occur in a other situations and the above was caused by the following line in an otherwise well-formed XML file:

```

&lt;description kind="short"&gt;Demonstrates choosing the drop location in the target &lt;code&gt;TransferHandler&lt;/code&gt;&lt;/description&gt;

```

The error was caused by the illegal embedded `code` tags.
