
# Creating a Custom JMX Client

The previous lessons in this trail have shown you how to create JMX technology MBeans and MXBeans, and register them with a JMX agent. However, all the previous examples have used an existing JMX client, JConsole. This lesson will demonstrate how to create your own custom JMX client.

An example of a custom JMX client,
 
[`Client`](../examples/Client.java)
 is included in 
[`jmx_examples.zip`](../examples/jmx_examples.zip). This JMX client interacts with the same MBean, MXBean and JMX agent as were seen in the previous lessons. Due to the size of the `Client` class, it will be examined in chunks, in the following sections.

## Importing the JMX Remote API Classes

To be able to create connections to JMX agents that are running remotely from the JMX client, you need to use the classes from the 
[`javax.management.remote`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/package-summary.html).

```

package com.example;
...

import javax.management.remote.JMXConnector;
import javax.management.remote.JMXConnectorFactory;
import javax.management.remote.JMXServiceURL;

public class Client {
...

```

The `Client` class will be creating 
[`JMXConnector`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnector.html) instances, for which it will need a 
[`JMXConnectorFactory`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnectorFactory.html) and a 
[`JMXServiceURL`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXServiceURL.html).

## Creating a Notification Listener

The JMX client needs a notification handler, to listen for and to process any notifications that might be sent by the MBeans that are registered in the JMX agent's MBean server. The JMX client's notification handler is an instance of the 
[`NotificationListener`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationListener.html) interface, as shown below.

```

... 

public static class ClientListener implements NotificationListener {

    public void handleNotification(Notification notification,
            Object handback) {
        echo("\nReceived notification:");
        echo("\tClassName: " + notification.getClass().getName());
        echo("\tSource: " + notification.getSource());
        echo("\tType: " + notification.getType());
        echo("\tMessage: " + notification.getMessage());
        if (notification instanceof AttributeChangeNotification) {
            AttributeChangeNotification acn =
                (AttributeChangeNotification) notification;
            echo("\tAttributeName: " + acn.getAttributeName());
            echo("\tAttributeType: " + acn.getAttributeType());
            echo("\tNewValue: " + acn.getNewValue());
            echo("\tOldValue: " + acn.getOldValue());
        }
    }
}    
...       

```

This notification listener determines the origin of any notifications it receives, and retrieves the information stored in the notification. It then performs different actions with the notification information according to the type of notification received. In this case, when the listener receives notifications of the type 
[`AttributeChangeNotification`](https://docs.oracle.com/javase/8/docs/api/javax/management/AttributeChangeNotification.html) it will obtain the name and type of the MBean attribute that has changed, as well as its old and new values, by calling the `AttributeChangeNotification` methods `getAttributeName`, `getAttributeType`, `getNewValue` and `getOldValue`.

A new `ClientListener` instance is created by later in the code.

```

 
ClientListener listener = new ClientListener();


```

## Creating an RMI Connector Client

The `Client` class creates an RMI connector client that is configured to connect to an RMI connector server that you will launch when you start the JMX agent, `Main`. This will allow the JMX client to interact with the JMX agent as if they were running on the same machine.

```

...
    
public static void main(String[] args) throws Exception {

echo("\nCreate an RMI connector client and " +
    "connect it to the RMI connector server");
JMXServiceURL url = 
    new JMXServiceURL("service:jmx:rmi:///jndi/rmi://:9999/jmxrmi");
JMXConnector jmxc = JMXConnectorFactory.connect(url, null);
...        

```

As you can see, the `Client` defines a 
[`JMXServiceURL`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXServiceURL.html) named `url`, that represents the location at which the connector client expects to find the connector server. This URL allows the connector client to retrieve the RMI connector server stub `jmxrmi` from the RMI registry running on port 9999 of the local host, and to connect to the RMI connector server.

With the RMI registry thus identified, the connector client can be created. The connector client, `jmxc`, is an instance of the interface 
[`JMXConnector`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnector.html), created by the `connect()` method of 
[`JMXConnectorFactory`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnectorFactory.html). The `connect()` method is passed the parameters `url` and a null environment map when it is called.

## Connecting to the Remote MBean Server

With the RMI connection in place, the JMX client must connect to the remote MBean server, so that it can interact with the various MBeans registered in it by the remote JMX agent.

```

...
        
MBeanServerConnection mbsc = 
    jmxc.getMBeanServerConnection();
                
...     

```

An instance of 
[`MBeanServerConnection`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanServerConnection.html), named mbsc, is then created by calling the `getMBeanServerConnection()` method of the `JMXConnector` instance `jmxc`.

The connector client is now connected to the MBean server created by the JMX agent, and can register MBeans and perform operations on them with the connection remaining completely transparent to both ends.

To start with, the client defines some simple operations to discover information about the MBeans found in the agent's MBean server.

```

...
        
echo("\nDomains:");
String domains[] = mbsc.getDomains();
Arrays.sort(domains);
for (String domain : domains) {
    echo("\tDomain = " + domain);
}
        
...
        
echo("\nMBeanServer default domain = " + mbsc.getDefaultDomain());

echo("\nMBean count = " +  mbsc.getMBeanCount());
echo("\nQuery MBeanServer MBeans:");
Set&lt;ObjectName&gt; names = 
    new TreeSet&lt;ObjectName&gt;(mbsc.queryNames(null, null));
for (ObjectName name : names) {
    echo("\tObjectName = " + name);
}
      
...
        

```

The client calls various methods of `MBeanServerConnection` in order to obtain the domains in which the different MBeans are operating, the number of MBeans registered in the MBean server, and the object names for each of the MBeans it discovers.

## Performing Operations on Remote MBeans via Proxies

The client accesses the `Hello` MBean in the MBean server through the MBean server connection by creating an MBean **proxy**. This MBean proxy is local to the client, and emulates the remote MBean.

```


...

ObjectName mbeanName = new ObjectName("com.example:type=Hello");
HelloMBean mbeanProxy = JMX.newMBeanProxy(mbsc, mbeanName, 
                                          HelloMBean.class, true);

echo("\nAdd notification listener...");
mbsc.addNotificationListener(mbeanName, listener, null, null);

echo("\nCacheSize = " + mbeanProxy.getCacheSize());

mbeanProxy.setCacheSize(150);

echo("\nWaiting for notification...");
sleep(2000);
echo("\nCacheSize = " + mbeanProxy.getCacheSize());
echo("\nInvoke sayHello() in Hello MBean...");
mbeanProxy.sayHello();

echo("\nInvoke add(2, 3) in Hello MBean...");
echo("\nadd(2, 3) = " + mbeanProxy.add(2, 3));

waitForEnterPressed();
        
...
        

```

MBean proxies allow you to access an MBean through a Java interface, allowing you to make calls on the proxy rather than having to write lengthy code to access a remote MBean. An MBean proxy for `Hello` is created here by calling the method `newMBeanProxy()` in the 
[`javax.management.JMX`](https://docs.oracle.com/javase/8/docs/api/javax/management/JMX.html) class, passing it the MBean's `MBeanServerConnection`, object name, the class name of the MBean interface and `true`, to signify that the proxy must behave as a 
[`NotificationBroadcaster`](https://docs.oracle.com/javase/8/docs/api/javax/management/NotificationBroadcaster.html). The JMX client can now perform the operations defined by `Hello` as if they were the operations of a locally registered MBean. The JMX client also adds a notification listener and changes the MBean's `CacheSize` attribute, to make it send a notification.

## Performing Operations on Remote MXBeans via Proxies

You can create proxies for MXBeans in exactly the same way as you create MBean proxies.

```

...
        
ObjectName mxbeanName = new ObjectName ("com.example:type=QueueSampler");
QueueSamplerMXBean mxbeanProxy = JMX.newMXBeanProxy(mbsc, 
    mxbeanName,  QueueSamplerMXBean.class);
QueueSample queue1 = mxbeanProxy.getQueueSample();
echo("\nQueueSample.Date = " + queue1.getDate());
echo("QueueSample.Head = " + queue1.getHead());
echo("QueueSample.Size = " + queue1.getSize());
echo("\nInvoke clearQueue() in QueueSampler MXBean...");
mxbeanProxy.clearQueue();

QueueSample queue2 = mxbeanProxy.getQueueSample();
echo("\nQueueSample.Date = " +  queue2.getDate());
echo("QueueSample.Head = " + queue2.getHead());
echo("QueueSample.Size = " + queue2.getSize());

...

```

As shown above, to create a proxy for an MXBean, all you have to do is call 
[`JMX.newMXBeanProxy`](https://docs.oracle.com/javase/8/docs/api/javax/management/JMX.html#newMXBeanProxy-javax.management.MBeanServerConnection-javax.management.ObjectName-java.lang.Class-) instead of `newMBeanProxy`. The MXBean proxy `mxbeanProxy` allows the client to invoke the `QueueSample` MXBean's operations as if they were the operations of a locally registered MXBean.

## Closing the Connection

Once the JMX client has obtained all the information it needs and performed all the required operations on the MBeans in the remote JMX agent's MBean server, the connection must be closed down.

```


jmxc.close();


```

The connection is closed with a call to the 
[`JMXConnector.close`](https://docs.oracle.com/javase/8/docs/api/javax/management/remote/JMXConnector.html#close--) method.

### To Run the Custom JMX Client Example

This example requires version 6 of the Java SE platform. To monitor the `Main` JMX agent remotely using a custom JMX client 
[`Client`](../examples/Client.java), follow these steps:

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
<li>Start the `Main` application, specifying the properties that expose `Main` for remote management:
<pre><code>
java -Dcom.sun.management.jmxremote.port=9999 \
     -Dcom.sun.management.jmxremote.authenticate=false \
     -Dcom.sun.management.jmxremote.ssl=false \ 
     com.example.Main
</code></pre>     
A confirmation that `Main` is waiting for something to happen is generated.
</li>
<li>Start the `Client` application in a different terminal window:
<pre><code>
java com.example.Client
</code></pre>
A confirmation that an `MBeanServerConnection` has been obtained is displayed.
</li>
<li>Press Enter.
The domains in which all the MBeans that are registered in the MBean server started by `Main` are displayed.
</li>
<li>Press Enter again.
The number of MBeans that are registered in the MBean server is displayed, as well as the object names of all these MBeans. The MBeans displayed include all the standard platform MXBeans running in the Java VM, as well as the `Hello` MBean and the `QueueSampler` MXBean that were registered in the MBean server by `Main`.
</li>
<li>Press Enter again.
The `Hello` MBean's operations are invoked by `Client`, with the following results:
<ul>
1. A notification listener is added to `Client` to listen for notifications from `Main`.
1. The value of the `CacheSize` attribute is changed from 200 to 150.
1. In the terminal window in which you started `Main`, confirmation of the `CacheSize` attribute change is displayed.
1. In the terminal window in which you started `Client`, a notification from `Main` is displayed, informing `Client` of the `CacheSize` attribute change.
1. The `Hello` MBean's `sayHello` operation is invoked.
1. In the terminal window in which you started `Main`, the message "Hello world" is displayed.
1. The `Hello` MBean's `add` operation is invoked, with the values 2 and 3 as parameters. The result is displayed by `Client`.
</ul>
</li>
<li>Press Enter again.
The `QueueSampler` MXBean's operations are invoked by `Client`, with the following results:
<ul>
1. The `QueueSample` values `date`, `head`, and `size` are displayed.
1. The `clearQueue` operation is invoked.
</ul>
</li>
<li>Press Enter again.
The `Client` closes the connection to the MBean server and a confirmation is displayed.
</li>

- The `QueueSample` values `date`, `head`, and `size` are displayed.
- The `clearQueue` operation is invoked.
