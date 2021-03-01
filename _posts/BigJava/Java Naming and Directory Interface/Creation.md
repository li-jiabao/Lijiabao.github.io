
#  Closing

Normal garbage collection takes care of removing <tt>Context</tt> instances when they are no longer in use. Connections used by <tt>Context</tt> instances being garbage collected will be closed automatically. Therefore, you do not need to explicitly close connections. Network connections, however, are limited resources and for certain programs, you might want to have control over their proliferation and usage. This section contains information on how to close connections and how to get notified if the server closes the connection.

## Explicit Closures

You invoke 
[<tt>Context.close()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#close--) on a <tt>Context</tt> instance to indicate that you no longer need to use it. If the <tt>Context</tt> instance being closed is using a dedicated connection, the connection is also closed. If the <tt>Context</tt> instance is sharing a connection with other <tt>Context</tt> and unterminated <tt>NamingEnumeration</tt> instances, the connection will not be closed until <tt>close()</tt> has been invoked on all such <tt>Context</tt> and <tt>NamingEnumeration</tt> instances.

In the 

[`example`](examples/Shared.java) from the 
[Connection Creation](create.html#SHARE) example section, all three <tt>Context</tt> instances must be closed before the underlying connection is closed.

```

// Create initial context
DirContext ctx = new InitialDirContext(env);

// Get a copy of the same context
Context ctx2 = (Context)ctx.lookup("");

// Get a child context
Context ctx3 = (Context) ctx.lookup("ou=NewHires");

// do something useful with ctx, ctx2, ctx3

// Close the contexts when we're done
ctx.close();
ctx2.close();
ctx3.close();

```

## Forced Implicit Closures

As mentioned previously, for those <tt>Context</tt> and <tt>NamingEnumeration</tt> instances that are no longer in scope, the Java runtime system will eventually garbage collect them, thus cleaning up the state that a <tt>close()</tt> would have done. To force the garbage collection, you can use the following code.

```

Runtime.getRuntime().gc();
Runtime.getRuntime().runFinalization();

```

Depending on the state of the program, performing this procedure can cause serious (temporary) performance degradation. If you need to ensure that connections are closed, track <tt>Context</tt> instances and close them explicitly.

## Detecting Connection Closures

LDAP servers often have an idle timeout period after which they will close connections no longer being used. When you subsequently invoke a method on a <tt>Context</tt> instance that is using such a connection, the method will throw a 
[<tt>CommunicationException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/CommunicationException.html). To detect when the server closes the connection that a <tt>Context</tt> instance is using, you register an 
[<tt>UnsolicitedNotificationListener</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/UnsolicitedNotificationListener.html) with the <tt>Context</tt> instance. 
[`AN example`](examples/RegUnsol.java) is shown in the LDAP Unsolicited Notifications section. Although the example is designed for receiving unsolicited notification from the server, it can also be used to detect connection closures by the server. After starting the program, stop the LDAP server and observe that the listener's 
[<tt>namingExceptionThrown()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamingListener.html#namingExceptionThrown-javax.naming.event.NamingExceptionEvent-) method gets called.
