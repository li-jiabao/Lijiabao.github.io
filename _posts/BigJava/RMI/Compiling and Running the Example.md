
# Compiling the Example Programs

In a real-world scenario in which a service such as the compute engine is deployed, a developer would likely create a Java Archive (JAR) file that contains the `Compute` and `Task` interfaces for server classes to implement and client programs to use. Next, a developer, perhaps the same developer of the interface JAR file, would write an implementation of the `Compute` interface and deploy that service on a machine available to clients. Developers of client programs can use the `Compute` and the `Task` interfaces, contained in the JAR file, and independently develop a task and client program that uses a `Compute` service.

In this section, you learn how to set up the JAR file, server classes, and client classes. You will see that the client's `Pi` class will be downloaded to the server at runtime. Also, the `Compute` and `Task` interfaces will be downloaded from the server to the registry at runtime.

This example separates the interfaces, remote object implementation, and client code into three packages:

<li>`compute` &#8211; 
[`Compute`](examples/compute/Compute.java) and 
[`Task`](examples/compute/Task.java) interfaces</li>
<li>`engine` &#8211; 
[`ComputeEngine`](examples/engine/ComputeEngine.java) implementation class</li>
<li>`client` &#8211; 
[`ComputePi`](examples/client/ComputePi.java) client code and 
[`Pi`](examples/client/Pi.java) task implementation</li>

First, you need to build the interface JAR file to provide to server and client developers.

## Building a JAR File of Interface Classes

First, you need to compile the interface source files in the `compute` package and then build a JAR file that contains their class files. Assume that user `waldo` has written these interfaces and placed the source files in the directory `c:\home\waldo\src\compute` on Windows or the directory `/home/waldo/src/compute` on Solaris OS or Linux. Given these paths, you can use the following commands to compile the interfaces and create the JAR file:

**Microsoft Windows**:

```

cd c:\home\waldo\src
javac compute\Compute.java compute\Task.java
jar cvf compute.jar compute\*.class

```

**Solaris OS or Linux**:

```

cd /home/waldo/src
javac compute/Compute.java compute/Task.java
jar cvf compute.jar compute/*.class

```
