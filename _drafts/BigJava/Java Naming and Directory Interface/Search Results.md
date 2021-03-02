
#  LDAP Unsolicited Notifications

The LDAP v3 (
[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)) defines an **unsolicited notification**, a message that is sent by an LDAP server to the client without any provocation from the client. An unsolicited notification is represented in the JNDI by the 
[<tt>UnsolicitedNotification</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/UnsolicitedNotification.html) interface.

Because unsolicited notifications are sent asynchronously by the server, you can use the same 
[event model](https://docs.oracle.com/javase/jndi/tutorial/beyond/event/index.html) used for receiving notifications about namespace changes and object content changes. You register interest in receiving unsolicited notifications by registering an 
[<tt>UnsolicitedNotificationListener</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/UnsolicitedNotificationListener.html) with an 
[<tt>EventContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventContext.html) or 
[<tt>EventDirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventDirContext.html).

Here is 
[`an example`](examples/RegUnsol.java) of an <tt>UnsolicitedNotificationListener</tt>.

```

public class UnsolListener implements UnsolicitedNotificationListener {
    public void notificationReceived(UnsolicitedNotificationEvent evt) {
        System.out.println("received: " + evt);
    }

    public void namingExceptionThrown(NamingExceptionEvent evt) {
        System.out.println("&gt;&gt;&gt; UnsolListener got an exception");
            evt.getException().printStackTrace();
    }
}

```

Following is 
[`an example`](examples/RegUnsol.java) that registers an implementation of <tt>UnsolicitedNotificationListener</tt> with an event source. Note that only the listener argument to 
[<tt>EventContext.addNamingListener()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventContext.html#addNamingListener-javax.naming.Name-int-javax.naming.event.NamingListener-) is relevant. The name and scope parameters are not relevant to unsolicited notifications.

```

// Get the event context for registering the listener
EventContext ctx = (EventContext)
    (new InitialContext(env).lookup("ou=People"));

// Create the listener
NamingListener listener = new UnsolListener();

// Register the listener with the context (all targets equivalent)
ctx.addNamingListener("", EventContext.ONELEVEL_SCOPE, listener);

```

When running this program, you need to point it at an LDAP server that can generate unsolicited notifications and prod the server to emit the notification. Otherwise, after one minute the program will exit silently.

A listener that implements <tt>UnsolicitedNotificationListener</tt> can also implement other 
[<tt>NamingListener</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamingListener.html) interfaces, such as 
[<tt>NamespaceChangeListener</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/NamespaceChangeListener.html) and 
[<tt>ObjectChangeListener</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/ObjectChangeListener.html).
