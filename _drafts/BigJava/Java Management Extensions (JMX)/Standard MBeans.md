
# Standard MBeans

This section presents an example of a straightforward, standard MBean.

A standard MBean is defined by writing a Java interface called `SomethingMBean` and a Java class called `Something` that implements that interface. Every method in the interface defines either an attribute or an operation in the MBean. By default, every method defines an operation. Attributes and operations are methods that follow certain design patterns. A standard MBean is composed of an MBean interface and a class. The MBean interface lists the methods for all exposed attributes and operations. The class implements this interface and provides the functionality of the instrumented resource.

The following sections examine an example of a standard MBean and a simple JMX technology-enabled agent (JMX agent) that manages the MBean.

## MBean Interface

An example of a basic MBean interface, 

[`HelloMBean`](../examples/HelloMBean.java)
, follows:

```


package com.example; 
 
public interface HelloMBean { 
 
    public void sayHello(); 
    public int add(int x, int y); 
    
    public String getName(); 
     
    public int getCacheSize(); 
    public void setCacheSize(int size); 
} 


```

By convention, an MBean interface takes the name of the Java class that implements it, with the suffix *`MBean`* added. In this case, the interface is called `HelloMBean`. The `Hello` class that implements this interface is described in the next section.

According to the JMX specification, an MBean interface consists of named and typed attributes that are readable and possibly writable, in addition to the named and typed operations that can be invoked by the applications that are managed by the MBean. The `HelloMBean` interface declares two operations: the Java methods `add()` and `sayHello()`.

`HelloMBean` declares two attributes: `Name` is a read-only string, and `CacheSize` is an integer that can be both read and written. Getter and setter methods are declared to allow the managed application to access and possibly change the attribute values. As defined by the JMX specification, a *getter* is any public method that does not return void and whose name begins with `get`. A getter enables a manager to read the value of the attribute, whose type is that of the returned object. A *setter* is any public method that takes a single parameter and whose name begins with `set`. A setter enables a manager to write a new value in the attribute, whose type is the same as that of the parameter.

The implementation of these operations and attributes is shown in the following section.

## MBean Implementation

The 

[`Hello`](../examples/Hello.java)
 Java class that follows implements the `HelloMBean` MBean interface:

```

package com.example; 
 
public class Hello ... 
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
        ...
    
        this.cacheSize = size; 
        System.out.println("Cache size now " + this.cacheSize); 
    } 
    ...
     
    private final String name = "Reginald"; 
    private int cacheSize = DEFAULT_CACHE_SIZE; 
    private static final int 
        DEFAULT_CACHE_SIZE = 200; 
}

```

The straightforward `Hello` class provides the definitions of the operations and attributes that are declared by `HelloMBean`. The `sayHello()` and `add()` operations are extremely simple, but real-life operations can be as simple or as sophisticated as needed.

The methods to get the `Name` attribute and to get and set the `CacheSize` attribute are also defined. In this example, the `Name` attribute value never changes. However, in a real scenario this attribute might change as the managed resource runs. For example, the attribute might represent statistics such as uptime or memory usage. Here, the attribute is merely the name `Reginald`.

Calling the `setCacheSize` method enables you to alter the `CacheSize` attribute from its declared default value of 200. In a real scenario, changing the `CacheSize` attribute could require other operations to be performed, such as discarding entries or allocating new entries. This example merely prints a message to confirm that the cache size has changed. However, more sophisticated operations could be defined instead of the simple call to `println()`.

With the `Hello` MBean and its interface thus defined, they can now be used to manage the resource they represent, as shown in the following section.

## Creating a JMX Agent to Manage a Resource

Once a resource has been instrumented by MBeans, the management of that resource is performed by a JMX agent.

The core component of a JMX agent is the MBean server. An MBean server is a managed object server in which MBeans are registered. A JMX agent also includes a set of services to manage MBeans. See the API documentation for the 
[`MBeanServer`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanServer.html) interface for details of the MBean server implementation.

The 

[`Main`](../examples/Main.java)
 class that follows represents a basic JMX agent:

```

package com.example; 
 
import java.lang.management.*; 
import javax.management.*; 
 
public class Main { 
 
    public static void main(String[] args) 
        throws Exception { 
     
        MBeanServer mbs = ManagementFactory.getPlatformMBeanServer(); 
        ObjectName name = new ObjectName("com.example:type=Hello"); 
        Hello mbean = new Hello(); 
        mbs.registerMBean(mbean, name); 
          
        ...
     
        System.out.println("Waiting forever..."); 
        Thread.sleep(Long.MAX_VALUE); 
    } 
} 

```

The JMX agent `Main` begins by obtaining an MBean server that has been created and initialized by the platform, by calling the `getPlatformMBeanServer()` method of the 
[`java.lang.management.ManagementFactory`](https://docs.oracle.com/javase/8/docs/api/java/lang/management/ManagementFactory.html) class. If no MBean server has been created by the platform already, then `getPlatformMBeanServer()` creates an MBean server automatically by calling the JMX method 
[`MBeanServerFactory.createMBeanServer()`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanServerFactory.html#createMBeanServer--). The `MBeanServer` instance obtained by `Main` is named `mbs`.

Next, `Main` defines an object name for the MBean instance that it will create. Every JMX MBean must have an object name. The object name is an instance of the JMX class 
[`ObjectName`](https://docs.oracle.com/javase/8/docs/api/javax/management/ObjectName.html) and must conform to the syntax defined by the JMX specification. Namely, the object name must contain a domain and a list of key-properties. In the object name defined by `Main`, the domain is `com.example` (the package in which the example MBean is contained). In addition, the key-property declares that this object is of the type `Hello`.

An instance of a `Hello` object, named `mbean`, is created. The `Hello` object named `mbean` is then registered as an MBean in the MBean server `mbs` with the object name `name`, by passing the object and the object name into a call to the JMX method 
[`MBeanServer.registerMBean()`](https://docs.oracle.com/javase/8/docs/api/javax/management/MBeanServer.html#registerMBean-java.lang.Object-javax.management.ObjectName-).

With the `Hello` MBean registered in the MBean server, `Main` simply waits for management operations to be performed on `Hello`. In this example, these management operations are invoking `sayHello()` and `add()`, and getting and setting the attribute values.

## Running the Standard MBean Example

Having examined the example classes, you can now run the example. In this example, JConsole is used to interact with the MBean.

To run the example, follow these steps:

<li>Save the bundle of JMX API sample classes, 
[`jmx_examples.zip`](../examples/jmx_examples.zip), to your working directory, `work_dir`.</li>
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
<li>If you are running the Java Development Kit (JDK) version 6, start the `Main` application with the following command.
<pre><code>
java com.example.Main
</code></pre>
If you are running a JDK version that is older than version 6, you will need to start the `Main` application with the following option specified, to expose the application for monitoring and management.
<pre><code>
java -Dcom.sun.management.jmxremote example.Main
</code></pre>
A confirmation that `Main` is waiting for something to happen is displayed.
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
You see the example MBean `Hello` that was created and registered by `Main`. If you click `Hello`, you see its associated Attributes and Operations nodes in the MBean tree.
</li>
<li>Expand the Attributes node of the `Hello` MBean in the MBean tree.
The MBean attributes that were defined by the `Hello` class are displayed.
</li>
<li>Change the value of the `CacheSize` attribute to 150.
In the terminal window in which you started `Main`, a confirmation of this attribute change is generated.
</li>
<li>Expand the Operations node of the `Hello` MBean in the MBean tree.
The two operations declared by the `Hello` MBean, `sayHello()` and `add()`, are visible.
</li>
<li>Invoke the `sayHello()` operation by clicking the `sayHello` button.
A JConsole dialog box informs you that the method was invoked successfully. The message *"hello, world"* is generated in the terminal window in which `Main` is running.
</li>
<li>Provide two integers for the `add()` operation to add and click the `add` button.
The answer is displayed in a JConsole dialog box.
</li>
1. To close JConsole, select Connection -&gt; Exit.
