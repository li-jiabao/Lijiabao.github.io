
# SASL

The LDAP v3 protocol uses the 
[SASL](http://www.ietf.org/rfc/rfc2222.txt) to support **pluggable** authentication. This means that the LDAP client and server can be configured to negotiate and use possibly nonstandard and/or customized mechanisms for authentication, depending on the level of protection desired by the client and the server. The LDAP v2 protocol does not support the SASL.

Several SASL mechanisms are currently defined:

<li>Anonymous (
[RFC 2245](http://www.ietf.org/rfc/rfc2245.txt))</li>
<li>CRAM-MD5 (
[RFC 2195](http://www.ietf.org/rfc/rfc2195.txt)
)</li>
<li>Digest-MD5 (
[RFC 2831](http://www.ietf.org/rfc/rfc2831.txt))</li>
<li>External (
[RFC 2222](http://www.ietf.org/rfc/rfc2830.txt))</li>
<li>Kerberos V4 (
[RFC 2222](http://www.ietf.org/rfc/rfc2830.txt))</li>
<li>Kerberos V5 (
[RFC 2222](http://www.ietf.org/rfc/rfc2830.txt))</li>
<li>SecurID (
[RFC 2808](http://www.ietf.org/rfc/rfc2808.txt))</li>
<li>S/Key (
[RFC 2222](http://www.ietf.org/rfc/rfc2830.txt))</li>

## SASL Mechanisms Supported by LDAP Servers

Of the mechanisms on the previous list, popular LDAP servers (such as those from Oracle, OpenLDAP, and Microsoft) support External, Digest-MD5, and Kerberos V5. 
[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt) proposes the use of Digest-MD5 as the mandatory default mechanism for LDAP v3 servers.

Here is a 
[`simple program`](examples/ServerSasl.java) for finding out the list of SASL mechanisms that an LDAP server supports.

```

// Create initial context
DirContext ctx = new InitialDirContext();

// Read supportedSASLMechanisms from root DSE
Attributes attrs = ctx.getAttributes(
    "ldap://localhost:389", new String[]{"supportedSASLMechanisms"});

```

Here is the output produced by running this program against a server that supports the External SASL mechanism.

```

{supportedsaslmechanisms=supportedSASLMechanisms: 
                         EXTERNAL, GSSAPI, DIGEST-MD5}

```

## Specifying the Authentication Mechanism

To use a particular SASL mechanism, you specify its Internet Assigned Numbers Authority (IANA)-registered mechanism name in the 
[<tt>Context.SECURITY_AUTHENTICATION</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION) environment property. You can also specify a list of mechanisms for the LDAP provider to try. This is done by specifying an ordered list of space-separated mechanism names. The LDAP provider will use the first mechanism for which it finds an implementation.

Here's an example that asks the LDAP provider to try to get the implementation for the DIGEST-MD5 mechanism and if that's not available, use the one for GSSAPI.

```

env.put(Context.SECURITY_AUTHENTICATION, "DIGEST-MD5 GSSAPI");

```

You might get this list of authentication mechanisms from the user of your application. Or you might get it by asking the LDAP server, via a call similar to that shown previously. The LDAP provider itself does not consult the server for this information. It simply attempts to locate and use the implementation of the specified mechanisms.

The LDAP provider in the platform has built-in support for the External, Digest-MD5, and GSSAPI (Kerberos v5) SASL mechanisms. You can add support for additional mechanisms.

## Specifying Input for the Authentication Mechanism

Some mechanisms, such as External, require no additional input--the mechanism name alone is sufficient for the authentication to proceed. The 
[External example](ssl.html#EXTERNAL) shows how to use the External SASL mechanism.

Most other mechanisms require some additional input. Depending on the mechanism, the type of input might vary. Following are some common inputs required by mechanisms.

- **Authentication id**. The identity of the entity performing the authentication.
- **Authorization id**. The identity of the entity for which access control checks should be made if the authentication succeeds.
- **Authentication credentials**. For example, a password or a key.

The authentication and authorization ids might differ if the program (such as a proxy server) is authenticating on behalf of another entity. The authentication id is specified by using the 
[<tt>Context.SECURITY_PRINCIPAL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL) environment property. It is of type <tt>java.lang.String</tt>.

The password/key of the authentication id is specified by using the 
[<tt>Context.SECURITY_CREDENTIALS</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_CREDENTIALS) environment property. It is of type <tt>java.lang.String</tt>, <tt>char</tt> array (<tt>char[]</tt>), or <tt>byte</tt> array (<tt>byte[]</tt>). If the password is a <tt>byte</tt> array, then it is transformed into a <tt>char</tt> array by using an UTF-8 encoding. <a name="authzid" id="authzid"></a>

If the <tt>"java.naming.security.sasl.authorizationId"</tt> property has been set, then its value is used as the authorization ID. Its value must be of type <tt>java.lang.String</tt>. By default, the empty string is used as the authorization ID, which directs the server to derive an authorization ID from the client's authentication credentials.

The 
[Digest-MD5 example](digest.html) shows how to use the <tt>Context.SECURITY_PRINCIPAL</tt> and <tt>Context.SECURITY_CREDENTIALS</tt> properties for Digest-MD5 authentication.

If a mechanism requires input other than those already described, then you need to define a **callback** object for the mechanism to use, you can check out the callback example in the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/callback.html). The next part of this lesson discusses how to use SASL Digest-MD5 authentication mechanism. The 
[SASL Policies](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/sasl.html), 
[GSS API (Kerberos v5)](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/gssapi.html) and 
[CRAM-MD5](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/crammd5.html) mechanisms are covered in the JNDI Tutorial.
