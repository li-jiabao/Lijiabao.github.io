
# MXBeans

This section explains a special type of MBean, called *MXBeans*.

An *MXBean* is a type of MBean that references only a predefined set of data types. In this way, you can be sure that your MBean will be usable by any client, including remote clients, without any requirement that the client have access to model-specific classes representing the types of your MBeans. MXBeans provide a convenient way to bundle related values together, without requiring clients to be specially configured to handle the bundles.

In the same way as for standard MBeans, an MXBean is defined by writing a Java interface called `SomethingMXBean` and a Java class that implements that interface. However, unlike standard MBeans, MXBeans do not require the Java class to be called `Something`. Every method in the interface defines either an attribute or an operation in the MXBean. The annotation `@MXBean` can be also used to annotate the Java interface, instead of requiring the interface's name to be followed by the MXBean suffix.

MXBeans existed in the Java 2 Platform, Standard Edition (J2SE) 5.0 software, in the package 
[`java.lang.management`](https://docs.oracle.com/javase/8/docs/api/java/lang/management/package-summary.html). However, users can now define their own MXBeans, in addition to the standard set that is defined in `java.lang.management`.

The main idea behind MXBeans is that types such as 
[`java.lang.management.MemoryUsage`](https://docs.oracle.com/javase/8/docs/api/java/lang/management/MemoryUsage.html) that are referenced in the MXBean interface, 
[`java.lang.management.MemoryMXBean`](https://docs.oracle.com/javase/8/docs/api/java/lang/management/MemoryMXBean.html) in this case, are mapped into a standard set of types, the so-called *Open Types* that are defined in the package 
[`javax.management.openmbean`](https://docs.oracle.com/javase/8/docs/api/javax/management/openmbean/package-summary.html). The exact mapping rules appear in the MXBean specification. However, the general principle is for simple types such as int or String to remain unchanged, while complex types such as `MemoryUsage` get mapped to the standard type 
[`CompositeDataSupport`](https://docs.oracle.com/javase/8/docs/api/javax/management/openmbean/CompositeDataSupport.html).

The MXBean example consists of the following files, which are found in 

[`jmx_examples.zip`](../examples/jmx_examples.zip):

- `QueueSamplerMXBean` interface
- `QueueSampler` class that implements the MXBean interface
- `QueueSample` Java type returned by the `getQueueSample()` method in the MXBean interface
- `Main`, the program that sets up and runs the example

The MXBean example uses these classes to perform the following actions:

- Defines a simple MXBean that manages a resource of type `Queue&lt;String&gt;`
<li>Declares a getter, `getQueueSample`, in the MXBean that takes a snapshot of the queue when invoked and returns a Java class `QueueSample` that bundles the following values together:
<ul>
- The time the snapshot was taken
- The queue size
- The head of the queue at that given time

## MXBean Interface

The following code shows the example 

[`QueueSamplerMXBean`](../examples/QueueSamplerMXBean.java)
 MXBean interface:

```

package com.example; 
 
public interface QueueSamplerMXBean { 
    public QueueSample getQueueSample(); 
    public void clearQueue(); 
} 


```

Note that you declare an MXBean interface in exactly the same way as you declare a standard MBean interface. The `QueueSamplerMXBean` interface declares a getter, `getQueueSample` and an operation, `clearQueue`.

## Defining MXBean Operations

The MXBean operations are declared in the 

[`QueueSampler`](../examples/QueueSampler.java)
 example class, as follows:

```

package com.example; 
 
import java.util.Date; 
import java.util.Queue; 
 
public class QueueSampler 
                implements QueueSamplerMXBean { 
     
    private Queue&lt;String&gt; queue; 
         
    public QueueSampler (Queue&lt;String&gt; queue) { 
        this.queue = queue; 
    } 
         
    public QueueSample getQueueSample() { 
        synchronized (queue) { 
            return new QueueSample(new Date(), 
                           queue.size(), queue.peek()); 
        } 
    } 
         
    public void clearQueue() { 
        synchronized (queue) { 
            queue.clear(); 
        } 
    } 
} 

```

`QueueSampler` defines the `getQueueSample()` getter and `clearQueue()` operation that were declared by the MXBean interface. The `getQueueSample()` operation returns an instance of the `QueueSample` Java type which was created with the values returned by the 
[`java.util.Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html) methods `peek()` and `size()`, and an instance of 
[`java.util.Date`](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html).

## Defining the Java Type Returned by the MXBean Interface

The `QueueSample` instance returned by `QueueSampler` is defined in the 

[`QueueSample`](../examples/QueueSample.java)
 class, as follows:

```

package com.example; 
 
import java.beans.ConstructorProperties; 
import java.util.Date; 
 
public class QueueSample { 
     
    private final Date date; 
    private final int size; 
    private final String head; 
         
    @ConstructorProperties({"date", "size", "head"}) 
    public QueueSample(Date date, int size, 
                        String head) { 
        this.date = date; 
        this.size = size; 
        this.head = head; 
    } 
         
    public Date getDate() { 
        return date; 
    } 
         
    public int getSize() { 
        return size; 
    } 
         
    public String getHead() { 
        return head; 
    } 
}   

```

In the `QueueSample` class, the MXBean framework calls all the getters in `QueueSample` to convert the given instance into a 
[`CompositeData`](https://docs.oracle.com/javase/8/docs/api/javax/management/openmbean/CompositeData.html) instance and uses the `@ConstructorProperties` annotation to reconstruct a `QueueSample` instance from a `CompositeData` instance.

## Creating and Registering the MXBean in the MBean Server

So far, the following have been defined: an MXBean interface and the class that implements it, as well as the Java type that is returned. Next, the MXBean must be created and registered in an MBean server. These actions are performed by the same 

[`Main`](../examples/Main.java)
 example JMX agent that was used in the standard MBean example, but the relevant code was not shown in the 
[Standard MBean](standard.html) lesson.

```


package com.example; 
 
import java.lang.management.ManagementFactory; 
import java.util.Queue; 
import java.util.concurrent.ArrayBlockingQueue; 
import javax.management.MBeanServer; 
import javax.management.ObjectName; 
 
public class Main { 
 
    public static void main(String[] args) throws Exception { 
        MBeanServer mbs = 
            ManagementFactory.getPlatformMBeanServer(); 
                
        ...  
        ObjectName mxbeanName = new ObjectName("com.example:type=QueueSampler");
        
        Queue&lt;String&gt; queue = new ArrayBlockingQueue&lt;String&gt;(10);
        queue.add("Request-1");
        queue.add("Request-2");
        queue.add("Request-3");
        QueueSampler mxbean = new QueueSampler(queue);
        
        mbs.registerMBean(mxbean, mxbeanName);
                 
        System.out.println("Waiting..."); 
        Thread.sleep(Long.MAX_VALUE); 
    } 
} 
 


```

The `Main` class performs the following actions:

- Gets the platform MBean server.
- Creates an object name for the MXBean `QueueSampler.`
- Creates a `Queue` instance for the `QueueSampler` MXBean to process.
- Feeds the `Queue` instance to a newly created `QueueSampler` MXBean.
- Registers the MXBean in the MBean server in exactly the same way as a standard MBean.

## Running the MXBean Example

The MXBean example uses classes from the 
[`jmx_examples.zip`](../examples/jmx_examples.zip) bundle that you used in the 
[Standard MBeans](standard.html) section. This example requires version 6 of the Java SE platform. To run the MXBeans example follow these steps:

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
<li>Start the `Main` application. A confirmation that `Main` is waiting for something to happen is generated.
<pre><code>
java com.example.Main
</code></pre>
</li>
<li>Start JConsole in a different terminal window on the same machine. The New Connection dialog box is displayed, presenting a list of running JMX agents that you can connect to.
<pre><code>
jconsole
</code></pre>
</li>
<li>In the New Connection dialog box, select `com.example.Main` from the list and click Connect.
A summary of your platform's current activity is displayed.
</li>
<li>Click the MBeans tab.
This panel shows all the MBeans that are currently registered in the MBean server.
</li>
<li>In the left frame, expand the `com.example` node in the MBean tree.
You see the example MBean `QueueSampler` that was created and registered by `Main`. If you click `QueueSampler`, you see its associated Attributes and Operations nodes in the MBean tree.
</li>
<li>Expand the Attributes node.
You see the `QueueSample` attribute appear in the right pane, with its value of `javax.management.openmbean.CompositeDataSupport`.
</li>
<li>Double-click the `CompositeDataSupport` value.
You see the `QueueSample` values `date`, `head`, and `size` because the MXBean framework has converted the `QueueSample` instance into `CompositeData`. If you had defined `QueueSampler` as a standard MBean rather than as an MXBean, JConsole would not have found the `QueueSample` class because it would not be in its class path. If `QueueSampler` had been a standard MBean, you would have received a `ClassNotFoundException` message when retrieving the `QueueSample` attribute value. The fact that JConsole finds `QueueSampler` demonstrates the usefulness of using MXBeans when connecting to JMX agents through generic JMX clients such as JConsole.
</li>
<li>Expand the Operations node.
A button to invoke the `clearQueue` operation is displayed.
</li>
<li>Click the `clearQueue` button.
A confirmation that the method was invoked successfully is displayed.
</li>
<li>Expand the Attributes node again, and double click on the `CompositeDataSupport` value.
The `head` and `size` values have been reset.
</li>
1. To close JConsole, select Connection -&gt; Exit.
