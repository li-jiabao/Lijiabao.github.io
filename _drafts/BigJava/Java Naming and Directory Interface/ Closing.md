
# Pooling

The 

[Connection Creation](create.html) section described when connections are created. It described how several <tt>Context</tt> instances can share the same connection.

Another type of connection sharing supported by the LDAP service provider is called **connection pooling**. In this type of sharing, the LDAP service provider maintains a pool of (possibly) previously used connections and assigns them to a <tt>Context</tt> instance as needed. When a <tt>Context</tt> instance is done with a connection (closed or garbage collected), the connection is returned to the pool for future use. Note that this form of sharing is sequential: a connection is retrieved from the pool, used, returned to the pool, and then, retrieved again from the pool for another <tt>Context</tt> instance.

The pool of connections is maintained per Java runtime system. For some situations, connection pooling can improve performance significantly. For example, only one connection is required for processing a search response that contains four referral references to the same LDAP server if connection pooling is used. Without connection pooling, such a scenario would require four separate connections.

The rest of this lesson describes in more detail how to use connection pooling.

## How to Use Connection Pooling

You request connection pooling by adding the property, <tt>"com.sun.jndi.ldap.connect.pool"</tt> to the environment properties passed to the initial context constructor. Here is 
[`an example`](examples/UsePool.java)
.

```

// Set up environment for creating initial context
Hashtable env = new Hashtable(11);
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Enable connection pooling
env.put("com.sun.jndi.ldap.connect.pool", "true");

// Create one initial context (Get connection from pool)
DirContext ctx = new InitialDirContext(env);

// do something useful with ctx

// Close the context when we're done
ctx.close();   // Return connection to pool

// Create another initial context (Get connection from pool)
DirContext ctx2 = new InitialDirContext(env);

// do something useful with ctx2

// Close the context when we're done
ctx2.close();   // Return connection to pool

```

This example creates two initial contexts in succession. The second initial context will reuse the connection used by the first. To run this program and observe how the connections are retrieved and returned to the pool, use the following command line.

```

#java -Dcom.sun.jndi.ldap.connect.pool.debug=fine UsePool

```

This should produce output that looks as follows.

```

Create com.sun.jndi.ldap.LdapClient@5d173[localhost:389]
Use com.sun.jndi.ldap.LdapClient@5d173
{ou=ou: NewHires, objectclass=objectClass: top, organizationalUnit}
Release com.sun.jndi.ldap.LdapClient@5d173
Use com.sun.jndi.ldap.LdapClient@5d173
{ou=ou: People, objectclass=objectClass: top, organizationalunit}
Release com.sun.jndi.ldap.LdapClient@5d173

```

You can decide when and where to use pooling by including or omitting the <tt>"com.sun.jndi.ldap.connect.pool"</tt> property, and thus control pooling on a **per-context** basis. In the previous example, if you removed this property from the environment properties before creating the second initial context, the second initial context would not use a pooled connection.

The LDAP provider keeps track of whether a connection is being used through indications from the application. It assumes that an application that is maintaining an open context handle is using the connection. Therefore, in order for the LDAP provider to properly manage the pooled connections, you must be diligent about invoking <tt>Context.close()</tt> on contexts that you no longer need.

Bad connections are automatically detected and removed from the pool by the LDAP provider. The probability of a context ending up with a bad connection is the same regardless of whether connection pooling is used.

## <a name="TIMEOUT" id="TIMEOUT">Creation Timeout</a>

The pool of connections maintained by the LDAP service provider may be limited in size; this is described in detail in the 
[Connection Pooling Configuration](config.html) section. When connection pooling has been enabled and no pooled connection is available, the client application will block, waiting for an available connection. You can use the <tt>"com.sun.jndi.ldap.connect.timeout"</tt> environment property to specify how long to wait for a pooled connection. If you omit this property, the application will wait indefinitely.

This same property is also used to specify a timeout period for establishment of the LDAP connection, as described in the 

[Connection Creation](create.html#TIMEOUT) section.

## When Not to Use Pooling!!

Pooled connections are intended to be reused. Therefore, if you plan to perform operations on a <tt>Context</tt> instance that might alter the underlying connection's state, then you should not use connection pooling for that <tt>Context</tt> instance. For example, if you plan to invoke the Start TLS extended operation on a <tt>Context</tt> instance, or plan to change security-related properties (such as <tt>"java.naming.security.principal"</tt> or <tt>"java.naming.security.protocol"</tt>) after the initial context has been created, you should not use connection pooling for that <tt>Context</tt> instance because the LDAP provider does not track any such state changes. If you use connection pooling in such situations, you might be compromising the security of your application.
