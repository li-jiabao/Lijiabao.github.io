
# Lesson: Notifications

The JMX API defines a mechanism to enable MBeans to generate notifications, for example, to signal a state change, a detected event, or a problem.

To generate notifications, an MBean must implement the interface 
[`NotificationEmitter`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationEmitter.html) or extend 
[`NotificationBroadcasterSupport`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationBroadcasterSupport.html). To send a notification, you need to construct an instance of the class 
[`javax.management.Notification`](https://docs.oracle.com/javase/8/docs/api/javax/management/Notification.html) or a subclass (such as 
[`AttributeChangedNotification`](https://docs.oracle.com/javase/8/docs/api/javax/management/AttributeChangeNotification.html)), and pass the instance to 
[`NotificationBroadcasterSupport.sendNotification`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationBroadcasterSupport.html#sendNotification-javax.management.Notification).

Every notification has a source. The source is the object name of the MBean that generated the notification.

Every notification has a sequence number. This number can be used to order notifications coming from the same source when order matters and there is a risk of the notifications being handled in the wrong order. The sequence number can be zero, but preferably the number increments for each notification from a given MBean.

The 

[`Hello`](../examples/Hello.java)
 MBean implementation in 
[Standard MBeans](../mbeans/standard.html)
 actually implements the notification mechanism. However, this code was omitted in that lesson for the sake of simplicity. The complete code for `Hello` follows:

```

package com.example;

import javax.management.*;

public class Hello
        extends NotificationBroadcasterSupport
        implements HelloMBean {

    public void sayHello() {
        System.out.println("hello, world");
    }

    public int add(int x, int y) {
        return x + y;
    }

    public String getName() {
        return this.name;
    }

    public int getCacheSize() {
        return this.cacheSize;
    }

    public synchronized void setCacheSize(int size) {
        int oldSize = this.cacheSize;
        this.cacheSize = size;

        System.out.println("Cache size now " + this.cacheSize);

        Notification n = new AttributeChangeNotification(this,
                                sequenceNumber++, System.currentTimeMillis(),
                                "CacheSize changed", "CacheSize", "int",
                                oldSize, this.cacheSize);

        sendNotification(n);
    }

    @Override
    public MBeanNotificationInfo[] getNotificationInfo() {
        String[] types = new String[]{
            AttributeChangeNotification.ATTRIBUTE_CHANGE
        };

        String name = AttributeChangeNotification.class.getName();
        String description = "An attribute of this MBean has changed";
        MBeanNotificationInfo info = 
                new MBeanNotificationInfo(types, name, description);
        return new MBeanNotificationInfo[]{info};
    }
    
    private final String name = "Reginald";
    private int cacheSize = DEFAULT_CACHE_SIZE;
    private static final int DEFAULT_CACHE_SIZE = 200;
    private long sequenceNumber = 1;
}

```

This `Hello` MBean implementation extends the `NotificationBroadcasterSupport` class. `NotificationBroadcasterSupport` implements the `NotificationEmitter` interface.

The operations and attributes are set in the same way as in the standard MBean example, with the exception that the `CacheSize` attribute's setter method now defines a value of `oldSize`. This value records the `CacheSize` attribute's value prior to the set operation.

The notification is constructed from an instance, `n`, of the JMX class `AttributeChangeNotification`, which extends `javax.management.Notification`. The notification is constructed within the definition of the `setCacheSize()` method from the following information. This information is passed to `AttributeChangeNotification` as parameters.

- The object name of the source of the notification, namely the `Hello` MBean, represented by `this`
- A sequence number, namely `sequenceNumber`, that is set to 1 and that increases incrementally
- A timestamp
- The content of the notification message
- The name of the attribute that has changed, in this case, `CacheSize`
- The type of attribute that has changed
- The old attribute value, in this case, `oldSize`
- The new attribute value, in this case, `this.cacheSize`

The notification `n` is then passed to the `NotificationBroadcasterSupport.sendNotification()` method.

Finally, the 
[`MBeanNotificationInfo`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanNotificationInfo.html) instance is defined to describe the characteristics of the different notification instances generated by the MBean for a given type of notification. In this case the type of notifications sent is `AttributeChangeNotification` notifications.

## Running the MBean Notification Example

Once again, you will use JConsole to interact with the `Hello` MBean, this time to send and receive notifications. This example requires version 6 of the Java SE platform.

<li>If you have not done so already, save 
[`jmx_examples.zip`](../examples/jmx_examples.zip) into your `work_dir` directory.</li>
<li>Unzip the bundle of sample classes by using the following command in a terminal window.
<pre><code>
unzip jmx_examples.zip
</code></pre>
</li>
<li>Compile the example Java classes from within the `work_dir` directory.
<pre><code>
javac com/example/*.java
</code></pre>
</li>
<li>Start the `Main` application.
<pre><code>
java com.example.Main
</code></pre>
A confirmation that `Main` is waiting for something to happen is generated.
</li>
<li>Start JConsole in a different terminal window on the same machine.
<pre><code>
jconsole
</code></pre>
The New Connection dialog box is displayed, presenting a list of running JMX agents that you can connect to.
</li>
<li>In the New Connection dialog box, select `com.example.Main` from the list and click Connect.
A summary of your platform's current activity is displayed.
</li>
<li>Click the MBeans tab.
This panel shows all the MBeans that are currently registered in the MBean server.
</li>
<li>In the left frame, expand the `com.example` node in the MBean tree.
You see the example MBean `Hello` that was created and registered by `Hello`. If you click `Hello`, you see its Notifications node in the MBean tree.
</li>
<li>Expand the Notifications node of the `Hello` MBean in the MBean tree.
Note that the panel is blank.
</li>
<li>Click the Subscribe button.
The current number of notifications received (0) is displayed in the Notifications node label.
</li>
<li>Expand the Attributes node of the `Hello` MBean in the MBean tree, and change the value of the `CacheSize` attribute to 150.
In the terminal window in which you started `Main`, a confirmation of this attribute change is displayed. Note that the number of received notifications displayed in the Notifications node has changed to 1.
</li>
<li>Expand again the Notifications node of the `Hello` MBean in the MBean tree.
The details of the notification are displayed.
</li>
1. To close JConsole, select Connection -&gt; Exit.
