
# Simple

**Simple** authentication consists of sending the LDAP server the fully qualified DN of the client (user) and the client's clear-text password (see 
[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt) and 
[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)). This mechanism has security problems because the password can be read from the network. To avoid exposing the password in this way, you can use the simple authentication mechanism within an encrypted channel (such as SSL), provided that this is supported by the LDAP server.

Both the LDAP v2 and v3 support simple authentication.

To use the simple authentication mechanism, you must set the three authentication environment properties as follows.

See the 
[example](authentication.html#SIMPLE) earlier in this section that illustrates how to use simple authentication.

**Note:** If you supply an empty string, an empty <tt>byte</tt>/<tt>char</tt> array, or <tt>null</tt> to the <tt>Context.SECURITY_CREDENTIALS</tt> environment property, then the authentication mechanism will be <tt>"none"</tt>. This is because the LDAP requires the password to be nonempty for simple authentication. The protocol automatically converts the authentication to <tt>"none"</tt> if a password is not supplied.
