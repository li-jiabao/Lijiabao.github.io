
# Modes of Authenticating to LDAP

In the LDAP, authentication information is supplied in the "bind" operation. In LDAP v2, a client initiates a connection with the LDAP server by sending the server a "bind" operation that contains the authentication information.

In the LDAP v3, this operation serves the same purpose, but it is optional. A client that sends an LDAP request without doing a "bind" is treated as an **anonymous** client (see the 

[Anonymous](anonymous.html) section for details). In the LDAP v3, the "bind" operation may be sent at any time, possibly more than once, during the connection. A client can send a "bind" request in the middle of a connection to change its identity. If the request is successful, then all outstanding requests that use the old identity on the connection are discarded and the connection is associated with the new identity.

The authentication information supplied in the "bind" operation depends on the **authentication mechanism** that the client chooses. See 
[Authentication Mechanisms](auth_mechs.html) for a discussion of the authentication mechanism.

## Authenticating to the LDAP by Using the JNDI

In the JNDI, authentication information is specified in environment properties. When you create an initial context by using the 
[<tt>InitialDirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html) class (or its superclass or subclass), you supply a set of environment properties, some of which might contain authentication information. You can use the following environment properties to specify the authentication information.

<li><br />
[<tt>Context.SECURITY_AUTHENTICATION</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION) (<tt>"java.naming.security.authentication"</tt>).<br />
Specifies the authentication mechanism to use. For the LDAP service provider in the JDK, this can be one of the following strings: <tt>"none"</tt>, <tt>"simple"</tt>, **sasl_mech**, where **sasl_mech** is a space-separated list of SASL mechanism names. See the 
[Authentication Mechanisms](auth_mechs.html) for a description of these strings.</li>
<li><br />
[<tt>Context.SECURITY_PRINCIPAL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL) (<tt>"java.naming.security.principal"</tt>).<br />
Specifies the name of the user/program doing the authentication and depends on the value of the <tt>Context.SECURITY_AUTHENTICATION</tt> property. See the next few sections in this lesson for details and examples.</li>
<li><br />
[<tt>Context.SECURITY_CREDENTIALS</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_CREDENTIALS) (<tt>"java.naming.security.credentials"</tt>).<br />
Specifies the credentials of the user/program doing the authentication and depends on the value of the <tt>Context.SECURITY_AUTHENTICATION</tt> property. See the next few sections in this lesson for details and examples.</li>

When the initial context is created, the underlying LDAP service provider extracts the authentication information from these environment properties and uses the LDAP "bind" operation to pass them to the server.

<a name="SIMPLE" id="SIMPLE"></a> The 
[`following example`](examples/Simple.java) shows how, by using a simple clear-text password, a client authenticates to an LDAP server.

```

// Set up the environment for creating the initial context
Hashtable&lt;String, Object&gt; env = new Hashtable&lt;String, Object&gt;();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Authenticate as S. User and password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

## Using Different Authentication Information for a Context

If you want to use different authentication information for an existing context, then you can use 
[<tt>Context.addToEnvironment()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#addToEnvironment-java.lang.String-java.lang.Object-) and 
[<tt>Context.removeFromEnvironment()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#removeFromEnvironment-java.lang.String-) to update the environment properties that contain the authentication information. Subsequent invocations of methods on the context will use the new authentication information to communicate with the server.

The 
[`following example`](examples/UseDiff.java) shows how the authentication information of a context is changed to <tt>"none"</tt> after the context has been created.

```

// Authenticate as S. User and the password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

// Change to using no authentication
ctx.addToEnvironment(Context.SECURITY_AUTHENTICATION, "none");

// ... do something useful with ctx

```

## Authentication Failures

Authentication can fail for a number of reasons. For example, if you supply incorrect authentication information, such as an incorrect password or principal name, then an 
[<tt>AuthenticationException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationException.html) is thrown.

Here is 
[`an example`](examples/BadPasswd.java) that is a variation of the previous example. This time, an incorrect password causes the authentication to fail.

```

// Authenticate as S. User and give an incorrect password
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "notmysecret");

```

This produces the following output.

```

javax.naming.AuthenticationException: [LDAP: error code 49 - Invalid Credentials]
        ...

```

Because different servers support different authentication mechanisms, you might request an authentication mechanism that the server does not support. In this case, an 
[<tt>AuthenticationNotSupportedException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationNotSupportedException.html) will be thrown.

Here is 
[`an example`](examples/BadAuth.java) that is a variation of the previous example. This time, an unsupported authentication mechanism (<tt>"custom"</tt>) causes the authentication to fail.

```

// Authenticate as S. User and the password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "custom");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

```

This produces the following output.

```

javax.naming.AuthenticationNotSupportedException: custom
        ...

```
