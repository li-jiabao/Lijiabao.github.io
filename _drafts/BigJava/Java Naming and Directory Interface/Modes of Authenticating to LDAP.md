
# Authentication Mechanisms

Different versions of the LDAP support different types of authentication. The LDAP v2 defines three types of authentication: anonymous, simple (clear-text password), and Kerberos v4.

The LDAP v3 supports anonymous, simple, and SASL authentication. SASL is the Simple Authentication and Security Layer (
[RFC 2222](http://www.ietf.org/rfc/rfc2222.txt)). It specifies a challenge-response protocol in which data is exchanged between the client and the server for the purposes of authentication and establishment of a security layer on which to carry out subsequent communication. By using SASL, the LDAP can support any type of authentication agreed upon by the LDAP client and server.

This lesson contains descriptions of how to authenticate by using 
[Anonymous](anonymous.html), 
[Simple](simple.html), and 
[SASL](sasl.html) authentication.

## Specifying the Authentication Mechanism

The authentication mechanism is specified by using the 
[<tt>Context.SECURITY_AUTHENTICATION</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION) environment property. The property may have one of the following values.

<th id="h1">Property Name</th><th id="h2">Property Value</th>
<td headers="h1">**sasl_mech**</td><td headers="h2">A space-separated list of SASL mechanism names. Use one of the SASL mechanisms listed (e.g., <tt>"CRAM-MD5"</tt> means to use the CRAM-MD5 SASL mechanism described in [RFC 2195](http://www.ietf.org/rfc/rfc2195.txt)).</td>
<td headers="h1"><tt>none</tt></td><td headers="h2">Use no authentication (anonymous)</td>
<td headers="h1"><tt>simple</tt></td><td headers="h2">Use weak authentication (clear-text password)</td>

## The Default Mechanism

If the client does not specify any authentication environment properties, then the default authentication mechanism is <tt>"none"</tt>. The client will then be treated as an anonymous client.

If the client specifies authentication information without explicitly specifying the <tt>Context.SECURITY_AUTHENTICATION</tt> property, then the default authentication mechanism is <tt>"simple"</tt>.
