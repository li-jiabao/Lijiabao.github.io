
# Exposing a Resource for Remote Management By JConsole

Exposing your Java applications for remote management by using the JMX API can be extremely simple, if you use the out-of-the-box remote management agent and an existing monitoring and management tool such as JConsole.

To expose your application for remote management, you need to start it with the correct properties. This example shows how to expose the 

[`Main`](../examples/Main.java)
 JMX agent for remote management.

For the sake of simplicity, the authentication and encryption security mechanisms are disabled in this example. However, you should implement these security mechanisms when implementing remote management in real-world environments. 
[What Next?](../end.html) provides pointers to other JMX technology documentation that shows how to activate security.

This example requires version 6 of the Java SE platform. To monitor the `Main` JMX agent remotely, follow these steps:

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
<li>Start the `Main` application, specifying the properties that expose `Main` for remote management. (For Windows, use the carat (`^`) instead of the backslash (`\`) to break up a long command into multiple lines):
<pre><code>
java -Dcom.sun.management.jmxremote.port=9999 \
     -Dcom.sun.management.jmxremote.authenticate=false \
     -Dcom.sun.management.jmxremote.ssl=false \
     com.example.Main
</code></pre>
A confirmation that `Main` is waiting for something to happen is generated.
</li>
<li>Start JConsole in a different terminal window on a **different machine**:
<pre><code>
jconsole
</code></pre>
The New Connection dialog box is displayed, presenting a list of running JMX agents that you can connect to locally.
</li>
<li>Select Remote Process, and type the following in the Remote Process field:
<pre><code>
*hostname*:9999
</code></pre>
In this address, `hostname` is the name of the remote machine on which the `Main` application is running and 9999 is the number of the port on which the out-of-the-box JMX connector will be connected.
</li>
<li>Click Connect.
A summary of the current activity of the Java Virtual Machine (Java VM) in which `Main` is running is displayed.
</li>
<li>Click the MBeans tab.
This panel shows all the MBeans that are currently registered in the remote MBean server.
</li>
<li>In the left-hand frame, expand the `com.example` node in the MBean tree.
You see the example MBean `Hello` that was created and registered by `Main`. If you click `Hello`, you see its associated Attributes and Operations nodes in the MBean tree, even though it is running on a different machine.
</li>
1. To close JConsole, select Connection -&gt; Exit.
