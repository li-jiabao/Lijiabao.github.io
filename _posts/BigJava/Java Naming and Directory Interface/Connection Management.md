
# Creation

There are several ways in which a connection is created. The most common way is from the creation of an initial context. When you create an 
[<tt>InitialContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html), 
[<tt>InitialDirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html), or 
[<tt>InitialLdapContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/InitialLdapContext.html) by using the LDAP service provider, a connection is set up immediately with the target LDAP server named in the 
[<tt>Context.PROVIDER_URL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#PROVIDER_URL) property. Each time an initial context is created, a new LDAP connection is created. See the 
[Pooling](pool.html) section for information on how to change this behavior.

If the property value contains more than one URL, then each URL is tried in turn until one is used to create a successful connection. The property value is then updated to be the successful URL. See the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/url.html#MULTI) for an example of how to create an initial context using a list of URLs.

There are three other direct ways in which a connection is created.

<li>By passing a URL as the name argument to the initial context. When an LDAP or LDAPS URL is passed as a name parameter to the initial context, the information in the URL is used to create a new connection to the LDAP server, regardless of whether the initial context instance itself has a connection to an LDAP server. In fact, the initial context might not be connected to any server. See the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/beyond/url/initctx.html) for more information on how URLs are used as names.</li>
<li>Another way that a connection is created is by use of a <tt>Reference</tt>. When a 
[<tt>Reference</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Reference.html) containing an LDAP or LDAPS URL is passed to 
[<tt>NamingManager.getObjectInstance()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/NamingManager.html#getObjectInstance-java.lang.Object-javax.naming.Name-javax.naming.Context-java.util.Hashtable-) or 
[<tt>DirectoryManager.getObjectInstance()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/DirectoryManager.html#getObjectInstance-java.lang.Object-javax.naming.Name-javax.naming.Context-java.util.Hashtable-javax.naming.directory.Attributes-), a new connection is created using information specified in the URL.</li>
<li>Finally, when a referral is followed manually or automatically, the information in the referral is used to create a new connection. See the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/index.html) for information on referrals.</li>

<a name="SHARE" id="SHARE"></a>

## Shared Connections

<tt>Context</tt> instances and 
[<tt>NamingEnumeration</tt>s](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html) that are derived from one <tt>Context</tt> instance share the same connection until changes to one of the <tt>Context</tt> instances make sharing no longer possible. For example, if you invoke 
[<tt>Context.lookup()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-), 
[<tt>Context.listBindings()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#listBindings-javax.naming.Name-) or 
[<tt>DirContext.search()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-) from the initial context and get back other <tt>Context</tt> instances, then all of those <tt>Context</tt> instances will share the same connection.

Here is 
[`an example`](examples/Shared.java).

```

// Create initial context
DirContext ctx = new InitialDirContext(env);

// Get a copy of the same context
Context ctx2 = (Context)ctx.lookup("");

// Get a child context
Context ctx3 = (Context) ctx.lookup("ou=NewHires");

```

In this example, <tt>ctx</tt>, <tt>ctx2</tt>, and <tt>ctx3</tt> will share the same connection.

Sharing is done regardless of how the <tt>Context</tt> instance came into existence. For example, a <tt>Context</tt> instance obtained by following a referral will share the same connection as the referral.

When you change a <tt>Context</tt> instance's environment properties that are related to a connection, such as the principal name or credentials of the user, the <tt>Context</tt> instance on which you make these changes will get its own connection (if the connection is shared). <tt>Context</tt> instances that are derived from this <tt>Context</tt> instance in the future will share this new connection. <tt>Context</tt> instances that previously shared the old connection are not affected (that is, they continue to use the old connection).

Here is 
[`an example`](examples/NewConn.java)
 that uses two connections.

```

// Create initial context (first connection)
DirContext ctx = new InitialDirContext(env);

// Get a copy of the same context
DirContext ctx2 = (DirContext)ctx.lookup("");

// Change authentication properties in ctx2
ctx2.addToEnvironment(Context.SECURITY_PRINCIPAL, 
    "cn=C. User, ou=NewHires, o=JNDITutorial");
ctx2.addToEnvironment(Context.SECURITY_CREDENTIALS, "mysecret");

// Method on ctx2 will use new connection
System.out.println(ctx2.getAttributes("ou=NewHires"));

```

<tt>ctx2</tt> initially shares the same connection with <tt>ctx</tt>. But when its principal and password properties are changed, it can no longer use <tt>ctx</tt>'s connection. The LDAP provider will automatically create a new connection for <tt>ctx2</tt>.

Similarly, if you use 
[<tt>LdapContext.reconnect()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapContext.html#reconnect-javax.naming.ldap.Control:A-) to change the <tt>Context</tt> instance's connection controls, the <tt>Context</tt> instance will get its own connection if the connection was being shared.

If the <tt>Context</tt> instance's connection was not being shared (i.e., no <tt>Context</tt>s have been derived from it), then changes to its environment or connection controls will not cause a new connection to be created. Instead, any changes relevant to the connection will be applied to the existing connection.

## <a name="TIMEOUT" id="TIMEOUT">Creation Timeouts</a>

Not all connection creations are successful. If the LDAP provider cannot establish a connection within a certain timeout period, it aborts the connection attempt. By default, this timeout period is the network (TCP) timeout value, which is in the order of a few minutes. To change the timeout period, you use the <tt>"com.sun.jndi.ldap.connect.timeout"</tt> environment property. The value of this property is the string representation of an integer representing the connection timeout in milliseconds.

Here is 
[`an example`](examples/Timeout.java).

```

// Set up environment for creating initial context
Hashtable env = new Hashtable(11);
env.put(Context.INITIAL_CONTEXT_FACTORY, 
    "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Specify timeout to be 5 seconds
env.put("com.sun.jndi.ldap.connect.timeout", "5000");

// Create initial context
DirContext ctx = new InitialDirContext(env);

// do something useful with ctx

```

In this example, if a connection cannot be created within 5 seconds, an exception will be thrown.

If the <tt>Context.PROVIDER_URL</tt> property contains more than one URL, the provider will use the timeout for each URL. For example, if there are 3 URLs and the timeout has been specified to be 5 seconds, then the provider will wait for a maximum of 15 seconds in total.

See the 
[Connection Pooling](pool.html#TIMEOUT) section for information on how this property affects connection pooling.
