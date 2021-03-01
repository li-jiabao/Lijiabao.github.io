
# Observe the Restricted Application

The last part of the 
[Quick Tour of Controlling Applications](../tour2/index.html) lesson shows how an application can be run under a security manager by invoking the interpreter with the new `-Djava.security.manager` command-line argument. But what if the application to be invoked resides inside a JAR file?

One of the interpreter options is the `-cp` (for class path) option, that lets you specify a search path for application classes and resources. Therefore, to execute the `Count` application inside the `sCount.jar` JAR file, specifying the file `C:\TestData\data` as its argument, you can type the following command while in the directory containing `sCount.jar`:

```

java -cp sCount.jar Count C:\TestData\data

```

To execute the application with a security manager, add `-Djava.security.manager`, as shown below:

```

**java -Djava.security.manager -cp sCount.jar Count C:\TestData\data**

```

**Important:**&#160;&#160;When you run this command, your Java interpreter will throw an exception shown below:

```

Exception in thread "main" java.security.AccessControlException:
access denied (java.io.FilePermission C:\TestData\data read)
    at java.security.AccessControlContext.checkPermission(Compiled Code)
    at java.security.AccessController.checkPermission(Compiled Code)
    at java.lang.SecurityManager.checkPermission(Compiled Code)
    at java.lang.SecurityManager.checkRead(Compiled Code)
    at java.io.FileInputStream.&lt;init&gt;(Compiled Code)
    at Count.main(Compiled Code)

```

In this example, `AccessControlException` reported that the `count` application does not have permission to read the file `C:\TestData\data`. Your interpreter raised this exception because it will not allow any application running under a security manager to read a file or to access other resources unless it has explicit permission to do so -- usually specified in a `grant` statement contained in a `policy` file.
