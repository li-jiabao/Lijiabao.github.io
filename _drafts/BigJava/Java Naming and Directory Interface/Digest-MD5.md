
# SSL and Custom Sockets

In addition to SASL authentication, most LDAP servers allow their services to be accessed through SSL. SSL is especially useful for LDAP v2 servers because the v2 protocol does not support SASL authentication.

An SSL-enabled server often supports SSL in two ways. In the most basic way, the server supports SSL ports in addition to normal (unprotected) ports. <!--
To use this service, the client needs to specify the port number of the SSL port
in the 
[<tt>Context.PROVIDER_URL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#PROVIDER_URL)   property and use SSL sockets when communicating with the server.
-->
The other way in which a server supports SSL is via the use of the Start TLS Extension (
[RFC 2830](http://www.ietf.org/rfc/rfc2830.txt)). This option is available only to LDAP v3 servers and is described in detail in the 
 section. <a name="SSLPROP" id="SSLPROP"></a>

## Using the SSL Socket Property

By default, the LDAP service provider in the JDK uses plain sockets when communicating with the LDAP server. To request that SSL sockets be use, set the 
[<tt>Context.SECURITY_PROTOCOL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PROTOCOL) property to <tt>"ssl"</tt>.

In the 
[`following example`](examples/Ssl.java), the LDAP server is offering SSL at port 636. To run this program, you must enable SSL on port 636 on your LDAP server. This procedure is typically carried out by the directory's administrator.

<a name="SERVER" id="SERVER"></a> **Server Requirements:** The LDAP server must be set up with an X.509 SSL server certificate and have SSL enabled. Typically, you must first obtain a signed certificate for the server from a certificate authority (CA). Then, follow the instructions from your directory vendor on how to enable SSL. Different vendors have different tools for doing this.

For the 
[Oracle Directory Server, v5.2](http://www.oracle.com/technetwork/testcontent/index-085178.html)
, use the Manage Certificates tool in the Administration Console to generate a Certificate Signing Request (CSR). Submit the CSR to a CA to obtain an X.509 SSL server certificate. Using the Administration Console, add the certificate to the server's list of certificates. Also install the CA's certificate if it is not already in the server's list of trusted CAs. Enable SSL by using the Configuration tab in the Administration Console. Select the server in the left pane. Select the Encryption tab in the right pane. Click the checkboxes for "Enable SSL for this server" and "Use this cipher family: RSA", ensuring that the server certificate you have added is in the list of certificates.

<a name="CLIENT" id="CLIENT"></a> **Client Requirements:** You need to ensure that the client trusts the LDAP server that you'll be using. You must install the server's certificate (or its CA's certificate) in your JRE's database of trusted certificates. Here is an example.

```

# cd **JAVA_HOME**/lib/security
# keytool -import -file server_cert.cer -keystore jssecacerts

```

For information on how to use the security tools, see the 
[Security](../../security/index.html) trail. For information on the JSSE, see the 
[Java Secure Socket Extension (JSSE) Reference Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html).

```

// Set up the environment for creating the initial context
Hashtable&lt;String, Object&gt; env = new Hashtable&lt;String, Object&gt;();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:636/o=JNDITutorial");

// Specify SSL
env.put(Context.SECURITY_PROTOCOL, "ssl");

// Authenticate as S. User and password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires,o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

**Note:** If you use SSL to connect to a server on a port that is **not** using SSL, then your program will hang. Similarly, if you use a plain socket to connect to a server's SSL socket, then your application will hang. This is a characteristic of the SSL protocol.

<a name="LDAPS" id="LDAPS"></a>

## Using the LDAPS URL

Instead of requesting the use of SSL via the use of the 
[<tt>Context.SECURITY_PROTOCOL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PROTOCOL) property, you can also request the use of SSL via the use of LDAPS URLs. An LDAPS URL is similar to an LDAP URL except that the URL scheme is "ldaps" instead of "ldap". It specifies the use of SSL when communicating with the LDAP server.

In the 
[`following example`](examples/Ldaps.java), the LDAP server is offering SSL at port 636. To run this program, you must enable SSL on port 636 on your LDAP server.

```

// Set up the environment for creating the initial context
Hashtable&lt;String, Object&gt; env = new Hashtable&lt;String, Object&gt;();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");

// Specify LDAPS URL
env.put(Context.PROVIDER_URL, "ldaps://localhost:636/o=JNDITutorial");

// Authenticate as S. User and password "mysecret"
env.put(Context.SECURITY_AUTHENTICATION, "simple");
env.put(Context.SECURITY_PRINCIPAL, 
        "cn=S. User, ou=NewHires, o=JNDITutorial");
env.put(Context.SECURITY_CREDENTIALS, "mysecret");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

LDAPS URLs are accepted anywhere LDAP URLs are accepted. Check out the 

[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/url.html) for details on LDAP and LDAPS URLs. <a name="EXTERNAL" id="EXTERNAL"></a>

## Client Authentication: Using SSL with the External SASL Mechanism

SSL provides authentication and other security services at a lower layer than the LDAP. If authentication has already been done at the SSL, the LDAP layer can use that authentication information from SSL by using the 
[External](http://www.ietf.org/rfc/rfc2222.txt) SASL mechanism.

The 
[`following example`](examples/External.java) is like the 
[`previous SSL example`](examples/Ssl.java), except that instead of using simple authentication, it uses the External SASL authentication. By using External, you do not need to supply any principal or password information, because they get picked up from the SSL.

<a name="SERVER2" id="SERVER2"></a> **Server Requirements:** This example requires the LDAP server to allow certificate-based client authentication. In addition, the LDAP server must trust (the CAs of) the client certificates that it receives, and must be able to map the owner distinguished names in the client certificates to principals that it knows about. Follow the instructions from your directory vendor on how to perform these tasks.

<a name="CLIENT2" id="CLIENT2"></a> **Client Requirements:** This example requires the client to have an X.509 SSL client certificate. Moreover, the certificate must be stored as the first key entry in a keystore file. If this entry is password-protected, it must have the same password as the keystore. For more information about JSSE keystores, see the 
[Java Secure Socket Extension (JSSE) Reference Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html).

```

// Set up the environment for creating the initial context
Hashtable&lt;String, Object&gt; env = new Hashtable&lt;String, Object&gt;(11);
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:636/o=JNDITutorial");

// Principal and credentials will be obtained from the connection
env.put(Context.SECURITY_AUTHENTICATION, "EXTERNAL");

// Specify SSL
env.put(Context.SECURITY_PROTOCOL, "ssl");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

...

```

To run this program so that the client's certificate is used for authentication, you must provide (as system properties) the location and password of the keystore containing the client's certificate. Here is an example of how to run the program.

```

java -Djavax.net.ssl.keyStore=MyKeystoreFile \
    -Djavax.net.ssl.keyStorePassword=mysecret \
    External

```

If you do not supply a keystore, the program will run using anonymous authentication because no client credential exists at the SSL.

This example shows the most basic way to accomplish certificate-based client authentication. More advanced ways can be accomplished by writing and using a custom socket factory that accesses the client certificate in a more flexible manner, perhaps by using an LDAP directory. The next section shows how to use a custom socket factory with your JNDI application.

## Using Custom Sockets

When using SSL, the LDAP provider will, by default, use the socket factory, 
[<tt>javax.net.ssl.SSLSocketFactory</tt>](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/SSLSocketFactory.html), for creating an SSL socket to communicate with the server, using the default JSSE configuration. The JSSE can be customized in a variety of ways, as detailed in the 
[Java Secure Socket Extension (JSSE) Reference Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html). However, there are times when those customizations are not sufficient and you need to have more control over the SSL sockets, or sockets in general, used by the LDAP service provider. For example, you might need sockets that can bypass firewalls, or JSSE sockets that use nondefault caching/retrieval policies for its trust and key stores. To set the socket factory implementation used by the LDAP service provider, set the <tt>"java.naming.ldap.factory.socket"</tt> property to the fully qualified class name of the socket factory. This class must implement the 
[<tt>javax.net.SocketFactory</tt>](https://docs.oracle.com/javase/8/docs/api/javax/net/SocketFactory.html) abstract class and provide an implementation of the 
[<tt>getDefault()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/net/SocketFactory.html#getDefault--) method that returns an instance of the socket factory. See the 
[Java Secure Socket Extension (JSSE) Reference Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html).

Here is an example of a 
[`custom socket factory`](examples/CustomSocketFactory.java) that produces plain sockets.

```

public class CustomSocketFactory extends SocketFactory {
    public static SocketFactory getDefault() {

        System.out.println("[acquiring the default socket factory]");
        return new CustomSocketFactory();
    }
        ...
}

```

Note that this example creates a new instance of <tt>CustomSocketFactory</tt> each time a new LDAP connection is created. This might be appropriate for some applications and socket factories. If you want to reuse the same socket factory, <tt>getDefault()</tt> should return a singleton.

To use this custom socket factory with a JNDI program, set the <tt>"java.naming.ldap.factory.socket"</tt> property, as shown in the 
[`following example`](examples/UseFactory.java).

```

// Set up the environment for creating the initial context
Hashtable&lt;String, Object&gt; env = new Hashtable&lt;String, Object&gt;();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Specify the socket factory
env.put("java.naming.ldap.factory.socket", "CustomSocketFactory");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```

The <tt>"java.naming.ldap.factory.socket"</tt> property is useful for setting the socket factory on a per context basis. Another way to control the sockets used by the LDAP service provider is to set the socket factory for all sockets used in the entire program, by using 
[<tt>java.net.Socket.setSocketImplFactory()</tt>](https://docs.oracle.com/javase/8/docs/api/java/net/Socket.html#setSocketImplFactory-java.net.SocketImplFactory-). Use of this method is less flexible because it affects all socket connections, not just LDAP connections and therefore, should be used with care.
