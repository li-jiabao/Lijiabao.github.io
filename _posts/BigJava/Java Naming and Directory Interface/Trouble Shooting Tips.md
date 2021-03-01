
# Lesson: Advanced Topics for LDAP Users

The lessons in the **LDAP** trail provide details on the mapping between the LDAP and the JNDI. They also give hints and tips for accessing the LDAP service through the JNDI.

## LDAP

X.500, is a CCITT standard for directory services that is part of the OSI suite of services. X.500 standard defines a protocol (among others) for a client application to access the X.500 directory called the **Directory Access Protocol** (DAP). It is layered on top of the Open Systems Interconnection (OSI) protocol stack.

The Internet community, recognizing the need for an X.500-like service but faced with different underlying network infrastructure (TCP/IP instead of OSI), designed a new protocol based on the X.500 DAP protocol, called **Lightweight** DAP, or LDAP. 
[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt) defines what is now called **version 3** of the LDAP (or LDAP v3), an improved version of its predecessor LDAP v2 specified in 
[RFC 1777](http://www.ietf.org/rfc/rfc1777.txt).

The goal of the LDAP was a protocol that could be easily implemented, with special focus on being able to build small and simple clients. One way that it attempted to achieve simplification was to use a lot of strings and to minimize wherever possible the use of structures. DNs, for example, are represented in the protocol as strings, as are attribute type names and many attribute values.

The protocol consists of the client's sending requests to the server, to which the server responds, though not necessarily in the same order in which the requests were sent. Each request is tagged with an ID so that requests and responses can be matched. The protocol works over either TCP or UDP, although the TCP version is most commonly used.

Because of the focus on clients, the LDAP community also defined standards for the string representation of DNs (
[RFC 2553](http://www.ietf.org/rfc/rfc2553.txt)), search filters (
[RFC 1960](http://www.ietf.org/rfc/rfc1960.txt)), and attribute syntaxes (
[RFC 1778](http://www.ietf.org/rfc/rfc1778.txt)), for a C language based API (
[RFC 1823](http://www.ietf.org/rfc/rfc1823.txt)), and for the format of URLs for accessing LDAP services (
[RFC 1959](http://www.ietf.org/rfc/rfc1959.txt)).

LDAP v3 supports internationalization, various authentication mechanisms, referrals, and a generic deployment mechanism. It allows new features to be added to the protocol without also requiring changes to the protocol by using **extensions** and **controls**.
