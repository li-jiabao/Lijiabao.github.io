
# Architecture of the JMX Technology

The JMX technology can be divided into three levels, as follows:

- Instrumentation
- JMX agent
- Remote management

## Instrumentation

To manage resources using the JMX technology, you must first instrument the resources in the Java programming language. You use Java objects known as *MBeans* to implement the access to the resources' instrumentation. MBeans must follow the design patterns and interfaces defined in the JMX specification. Doing so ensures that all MBeans provide managed resource instrumentation in a standardized way. In addition to standard MBeans, the JMX specification also defines a special type of MBean called an *MXBean*. An MXBean is an MBean that references only a pre-defined set of data types. Other types of MBean exist, but this trail will concentrate on standard MBeans and MXBeans.

Once a resource has been instrumented by MBeans, it can be managed through a JMX agent. MBeans do not require knowledge of the JMX agent with which they will operate.

MBeans are designed to be flexible, simple, and easy to implement. Developers of applications, systems, and networks can make their products manageable in a standard way without having to understand or invest in complex management systems. Existing resources can be made manageable with minimum effort.

In addition, the instrumentation level of the JMX specification provides a notification mechanism. This mechanism enables MBeans to generate and propagate notification events to components of the other levels.

## JMX Agent

A JMX technology-based agent (JMX agent) is a standard management agent that directly controls resources and makes them available to remote management applications. JMX agents are usually located on the same machine as the resources they control, but this arrangement is not a requirement.

The core component of a JMX agent is the **MBean server**, a managed object server in which MBeans are registered. A JMX agent also includes a set of services to manage MBeans, and at least one communications adaptor or connector to allow access by a management application.

When you implement a JMX agent, you do not need to know the semantics or functions of the resources that it will manage. In fact, a JMX agent does not even need to know which resources it will serve because any resource instrumented in compliance with the JMX specification can use any JMX agent that offers the services that the resource requires. Similarly, the JMX agent does not need to know the functions of the management applications that will access it.

## Remote Management

JMX technology instrumentation can be accessed in many different ways, either through existing management protocols such as the Simple Network Management Protocol (SNMP) or through proprietary protocols. The MBean server relies on protocol adaptors and connectors to make a JMX agent accessible from management applications outside the agent's Java Virtual Machine (Java VM).

Each adaptor provides a view through a specific protocol of all MBeans that are registered in the MBean server. For example, an HTML adaptor could display an MBean in a browser.

Connectors provide a manager-side interface that handles the communication between manager and JMX agent. Each connector provides the same remote management interface through a different protocol. When a remote management application uses this interface, it can connect to a JMX agent transparently through the network, regardless of the protocol. The JMX technology provides a standard solution for exporting JMX technology instrumentation to remote applications based on Java Remote Method Invocation (Java RMI).
