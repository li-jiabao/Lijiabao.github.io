
# Lesson: Overview of JNDI

The Java Naming and Directory Interface&#8482; (JNDI) is an application programming interface (API) that provides [naming](naming.html) and [directory](dir.html) functionality to applications written using the Java&#8482; programming language. It is defined to be independent of any specific directory service implementation. Thus a variety of directories -new, emerging, and already deployed can be accessed in a common way.

## Architecture

The JNDI architecture consists of an API and a service provider interface (SPI). Java applications use the JNDI API to access a variety of naming and directory services. The SPI enables a variety of naming and directory services to be plugged in transparently, thereby allowing the Java application using the JNDI API to access their services. See the following figure:

## Packaging

JNDI is included in the Java SE Platform. To use the JNDI, you must have the JNDI classes and one or more service providers. The JDK includes service providers for the following naming/directory services:

- Lightweight Directory Access Protocol (LDAP)
- Common Object Request Broker Architecture (CORBA) Common Object Services (COS) name service
- Java Remote Method Invocation (RMI) Registry
- Domain Name Service (DNS)

Other service providers can be downloaded from the 
[JNDI page](http://www.oracle.com/technetwork/java/jndi/index.html) or obtained from other vendors.

The JNDI is divided into five packages:

- [javax.naming](naming.html)
- [javax.naming.directory](dir.html)
- [javax.naming.ldap](dir.html)
- [javax.naming.event](event.html)
- [javax.naming.spi](event.html)

The next part of the lesson has a brief description of the JNDI packages.
