
# Digest-MD5

**Digest-MD5 authentication** is the required authentication mechanism for LDAP v3 servers (
[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt)). Because the use of SASL is part of the LDAP v3 (
[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)), servers that support only the LDAP v2 do not support Digest-MD5.

The Digest-MD5 mechanism is described in 
[RFC 2831](http://www.ietf.org/rfc/rfc2831.txt). It is based on the HTTP Digest Authentication (
[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)). In Digest-MD5, the LDAP server sends data that includes various authentication options that it is willing to support plus a special token to the LDAP client. The client responds by sending an encrypted response that indicates the authentication options that it has selected. The response is encrypted in such a way that proves that the client knows its password. The LDAP server then decrypts and verifies the client's response.

To use the Digest-MD5 authentication mechanism, you must set the authentication environment properties as follows.

The 
[`following example`](examples/Digest.java) shows how a client performs authentication using Digest-MD5 to an LDAP server.

```

// Set up the environment for creating the initial context
Hashtable&lt;String, Object&gt; env = new Hashtable&lt;String, Object&gt;();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Authenticate as C. User and password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "DIGEST-MD5");
env.put(Context.SECURITY_PRINCIPAL, 
        "dn:cn=C. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

## Specifying the Realm

A **realm** defines the namespace from which the authentication entity (the value of the <tt>Context.SECURITY_PRINCIPAL</tt> property) is selected. A server might have multiple realms. For example, a server for a university might be configured to have two realms, one for its student users and another for faculty users. Realm configuration is done by the directory administrator. Some directories have a default single realm. For example, the Oracle Directory Server, v5.2, uses the fully qualified hostname of the machine as the default realm.

In Digest-MD5 authentication, you must authenticate to a specific realm. You may use the following authentication environment property to specify the realm. If you do not specify a realm, then any one of the realms offered by the server will be used.

The 
[`following example`](examples/DigestRealm.java) shows how to set the environment properties for performing authentication using Digest-MD5 and a specified realm. To make this example work in your environment, you must change the source code so that the realm value reflects what has been configured on your directory server.

```

// Authenticate as C. User and password "mysecret" in realm "JNDITutorial"
env.put(Context.SECURITY_AUTHENTICATION, "DIGEST-MD5");
env.put(Context.SECURITY_PRINCIPAL, 
        "dn:cn=C. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");
env.put("java.naming.security.sasl.realm", "JNDITutorial");

```

If you need to use 
[privacy protection](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/digest.html) and other SASL properties, these are discussed in the JNDI Tutorial.
