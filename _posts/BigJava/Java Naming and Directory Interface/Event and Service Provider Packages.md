
# Lesson: Software Setup

## Required Software

Following is a list of the software/systems that you need:

- [Java platform software](#JDK)
- [Service provider software](#PROVIDER)
- [Naming and directory server software](#SERVER)

### <a name="JDK" id="JDK">Java Platform Software</a>

JNDI is included in the Java SE Platform.

To run the applets, you can use any Java-compatible Web browser, such as Firefox, or Internet Explorer v5 or later. To ensure that your applets take full advantage of the latest features of the Java platform software, you can use the Java Plug-in with your Web browser.

### <a name="PROVIDER" id="PROVIDER">Service Provider Software</a>

The JNDI API is a generic API for accessing any naming or directory service. Actual access to a naming or directory service is enabled by plugging in a service provider under the JNDI. An overview of the JNDI architecture and the role of service providers is given in the 
[JNDI Overview](../overview/index.html) lesson.

A **service provider** is software that maps the JNDI API to actual calls to the naming or directory server. Typically, the roles of the service provider and that of the naming/directory server differ. In the terminology of client/server software, the JNDI and the service provider are the **client** (called the **JNDI client**) and the naming/directory server is the **server**.

Clients and servers may interact in many ways. In one common way, they use a network protocol so that the client and server can exist autonomously in a networked environment. The server typically supports many different clients, not only JNDI clients, provided that the clients conform to the specified protocol. The JNDI does not dictate any particular style of interaction between JNDI clients and servers. For example, at one extreme the client and server could be the same entity.

You need to obtain the classes for the service providers that you will be using. For example, if you plan to use the JNDI to access an LDAP directory server, then you would need software for an LDAP service provider.

The JDK comes with service providers for:

- Light Weight Directory Protocol (LDAP)
- CORBA Common Object Services naming (COS naming)
- RMI registry
- Domain Name Service (DNS)

If you are interested in other providers please check the 
[JNDI page](http://www.oracle.com/technetwork/java/jndi/index.html) for download information.

This tutorial uses only the LDAP Service provider. When using the LDAP service provider, you need either to set up your own server or to have access to an existing server, as explained next.

### <a name="SERVER" id="SERVER">Naming and Directory Server Software</a>

Once you have obtained the service provider software, you then need to set up or have access to a corresponding naming/directory server. Setting up a naming/directory server is typically the job of a network system administrator. Different vendors have different installation procedures for their naming/directory servers. Some require special machine privileges before the server can be installed. You should consult the naming/directory server software's installation instructions.

For the directory examples in this tutorial, you need access to an LDAP server. If you would like to take a quick tour of what LDAP is check out 
[here](http://en.wikipedia.org/wiki/LDAP). You can use any LDAP-compliant server of your choice. The Oracle Directory Server, which runs on many platforms, including Windows, is available for evaluation at: 
[Oracle Directory Server](http://www.oracle.com/technetwork/testcontent/index-085178.html).

You can also download free LDAP servers below:

<li>
[OpenDS](http://opends.java.net/)</li>
<li>
[OpenLDAP](http://www.OpenLDAP.org/)</li>
<li>
[389 Directory Server](http://directory.fedoraproject.org/)</li>
<li>
[Apache Directory Server](http://directory.apache.org)</li>

A publicly accessible server is available at: ldap://ldap.openldap.org Naming Context: dc=OpenLDAP,dc=org
