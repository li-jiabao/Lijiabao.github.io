
# LDAP v3

## Internationalization

Internationalization is addressed via an international character set (ISO 10646) to represent protocol elements that are strings (such as DNs). Version 3 also differs from version 2 in that it uses UTF-8 to encode its strings.

## Authentication

In addition to anonymous, simple (clear-text password) authentication, LDAP v3 uses the Simple Authentication and Security Layer (SASL) authentication framework (
[RFC 2222](http://www.ietf.org/rfc/rfc2222.txt)) to allow different authentication mechanisms to be used with the LDAP. SASL specifies a challenge-response protocol in which data is exchanged between the client and the server for the purposes of authentication.

Several SASL mechanisms are currently defined: 
[DIGEST-MD5](http://www.ietf.org/rfc/rfc2831.txt), 
[CRAM-MD5](http://www.ietf.org/rfc/rfc2195.txt), 
[Anonymous](http://www.ietf.org/rfc/rfc2245.txt), 
[External](http://www.ietf.org/rfc/rfc2222.txt), 
[S/Key](http://www.ietf.org/rfc/rfc2222.txt), 
[GSSAPI](http://www.ietf.org/rfc/rfc2222.txt), and 
[Kerberos v4](http://www.ietf.org/rfc/rfc2222.txt). An LDAP v3 client can use any of these SASL mechanisms, provided that the LDAP v3 server supports them. Moreover, new (yet-to-be defined) SASL mechanisms can be used without changes to the LDAP having to be made.

## Referrals

A **referral** is information that a server sends back to the client indicating that the requested information can be found at another location (possibly at another server). In the LDAP v2, servers are supposed to handle referrals and not return them to the client. This is because handling referrals can be very complicated and therefore would result in more-complicated clients. As servers were built and deployed, referrals were found to be useful, but not many servers supported server-side referral handling. So a way was found to retrofit the protocol to allow it to return referrals. This was done by placing the referral inside of the error message of a "partial result" error response.

The LDAP v3 has explicit support for referrals and allows servers to return the referrals directly to the client. Referrals won't be covered in this lesson but you can always refer to 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/index.html) for managing referrals in your application.

## Deployment

A common protocol such as the LDAP is useful for ensuring that all of the directory clients and servers "speak the same language." When many different directory client applications and directory servers are deployed in a network, it also is very useful for all of these entities to talk about the same objects.

A **directory schema** specifies, among other things, the types of objects that a directory may have and the mandatory and optional attributes that each type of object may have. The LDAP v3 defines a schema (
[RFC 2252](http://www.ietf.org/rfc/rfc2252.txt) and 
[RFC 2256](http://www.ietf.org/rfc/rfc2256.txt)) based on the X.500 standard for common objects found in the network, such as countries, localities, organizations, users/persons, groups, and devices. It also defines a way for a client application to access the server's schema so that it can find out the types of objects and attributes that a particular server supports.

The LDAP v3 further defines a set of syntaxes for representing attribute values (
[RFC 2252](http://www.ietf.org/rfc/rfc2252.txt)). For writing Java Applications that need to access schema please refer to the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/schema/index.html).

## Extensions

In addition to the repertoire of predefined operations, such as "search" and "modify," the LDAP v3 defines an **"extended" operation**. The "extended" operation takes a request as an argument and returns a response. The request contains an identifier that identifies the request and the arguments of the request. The response contains the results of performing the request. The pair of "extended" operation request/response is called an **extension**. For example, there can be an extension for Start TLS, which is a request for the client to the server to activate the Start TLS protocol.

These extensions can be standard (defined by the LDAP community) or proprietary (defined by a particular directory vendor). For writing applications that need to use Extensions refer to the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/index.html).

## Controls

Another way to add new features is by using a **control**. The LDAP v3 allows the behavior of any operation to be modified through the use of controls. Any number of controls may be sent along with an operation, and any number of controls may be returned with its results. For example, you can send a Sort control along with a "search" operation that tells the server to sort the results of the search according to the <tt>"name"</tt> attribute.

Like extensions, such controls can be standard or proprietary. The standard controls are provided in the 
[platform](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Control.html). For writing applications that need to use controls refer to the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/index.html).
