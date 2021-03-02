
# Event and Service Provider Packages

## Event Package

The 
[<tt>javax.naming.event</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/package-summary.html) package contains classes and interfaces for supporting event notification in naming and directory services. Event notification is described in detail in the 
[Event Notification]() trail.

To receive event notifications, a listener must be registered with either an 
[<tt>EventContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventContext.html) or an 
[<tt>EventDirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/event/EventDirContext.html). Once registered, the listener will receive event notifications when the corresponding changes occur in the naming/directory service. The details about Event Notification can be found in the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/beyond/event/index.html).

## Service Provider Package

The 
[<tt>javax.naming.spi</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/package-summary.html) package provides the means by which developers of different naming/directory service providers can develop and hook up their implementations so that the corresponding services are accessible from applications that use the JNDI.

This package also provides support for doing the reverse. That is, implementors of 
[<tt>Context.bind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#bind-javax.naming.Name-java.lang.Object-) and related methods can accept Java objects and store the objects in a format acceptable to the underlying naming/directory service. This support is provided in the form of 
[state factories](../objects/index.html#STATEFAC).

The details about the Service Provider mechanism can be found in the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/provider/index.html).
