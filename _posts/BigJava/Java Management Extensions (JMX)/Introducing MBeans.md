
# Lesson: Introducing MBeans

This lesson introduces the fundamental concept of the JMX API, namely managed beans, or *MBeans*.

An MBean is a managed Java object, similar to a JavaBeans component, that follows the design patterns set forth in the JMX specification. An MBean can represent a device, an application, or any resource that needs to be managed. MBeans expose a management interface that consists of the following:

- A set of readable or writable attributes, or both.
- A set of invokable operations.
- A self-description.

The management interface does not change throughout the life of an MBean instance. MBeans can also emit notifications when certain predefined events occur.

The JMX specification defines five types of MBean:

- Standard MBeans
- Dynamic MBeans
- Open MBeans
- Model MBeans
- MXBeans

The examples in this trail demonstrate only the simplest types of MBean, namely standard MBeans and MXBeans.
