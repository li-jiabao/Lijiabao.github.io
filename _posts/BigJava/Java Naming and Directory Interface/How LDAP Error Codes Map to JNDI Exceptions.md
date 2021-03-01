
# Security

An LDAP service provides a generic directory service. It can be used to store information of all sorts. All LDAP servers have some system in place for controlling who can read and update the information in the directory.

To access the LDAP service, the LDAP client first must **authenticate** itself to the service. That is, it must tell the LDAP server who is going to be accessing the data so that the server can decide what the client is allowed to see and do. If the client authenticates successfully to the LDAP server, then when the server subsequently receives a request from the client, it will check whether the client is allowed to perform the request. This process is called **access control**.

The LDAP standard has proposed ways in which LDAP clients can authenticate to LDAP servers (
[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)
 and 
[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)). These are discussed in general in the 

[LDAP Authentication section](authentication.html) and 
[Authentication Mechanisms section](auth_mechs.html). This lesson also contains descriptions of how to use the 
[anonymous](anonymous.html), 
[simple](simple.html) and 
[SASL](sasl.html) authentication mechanisms.

Access control is supported in different ways by different LDAP server implementations. It is not discussed in this lesson.

Another security aspect of the LDAP service is support the use of secure channels to communicate with clients, for example to send and receive attributes that contain secrets, such as passwords and keys. LDAP servers use SSL for this purpose. This lesson also shows how to 
[use SSL](ssl.html) with the LDAP service provider.
