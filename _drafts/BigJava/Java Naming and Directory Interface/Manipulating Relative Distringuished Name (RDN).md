
# Setting Timeout for Ldap Operations

When an LDAP request is made by a client to a server and the server does not respond for some reason, the client waits forever for the server to respond until the TCP timeouts. On the client-side what the user experiences is esentially a process hang. In order to control the LDAP request in a timely manner, a read timeout can be configured for the JNDI/LDAP Service Provider since Java SE 6.

The new environment property:

`com.sun.jndi.ldap.read.timeout`

can be used to specify the read timeout for an LDAP operation. The value of this property is the string representation of an integer representing the read timeout in milliseconds for LDAP operations. If the LDAP provider doesn't get an LDAP response within the specified period, it aborts the read attempt. The integer should be greater than zero. An integer less than or equal to zero means no read timeout is specified which is equivalent to waiting for the response infinitely until it is received which defaults to the original behavior.

If this property is not specified, the default is to wait for the response until it is received.<br />
For example, `env.put("com.sun.jndi.ldap.read.timeout", "5000");` causes the LDAP service provider to abort the read attempt if the server does not respond with a reply within 5 seconds.

Here is an example,
[`ReadTimeoutTest`](examples/ReadTimeoutTest.java), that uses a dummy server which does not respond to LDAP requests to show how this property behaves when set to a non-zero value.

```

env.put(Context.INITIAL_CONTEXT_FACTORY,
        "com.sun.jndi.ldap.LdapCtxFactory");
env.put("com.sun.jndi.ldap.read.timeout", "1000");
env.put(Context.PROVIDER_URL, "ldap://localhost:2001");

Server s = new Server();

try {

    // start the server
    s.start();
 
   // Create initial context
   DirContext ctx = new InitialDirContext(env);
   System.out.println("LDAP Client: Connected to the Server");
        :
        :
} catch (NamingException e) {
   e.printStackTrace();
}

```

The above program prints the stack trace below, as the server does not even respond to the LDAP bind request when an InitialDirContext is created. The client times out waiting for the server's response.

```

Server: Connection accepted
javax.naming.NamingException: LDAP response read timed out, timeout used:1000ms.
:
:

at javax.naming.directory.InitialDirContext.&lt;init&gt;(InitialDirContext.java:82)
at ReadTimeoutTest.main(ReadTimeoutTest.java:32)

```

Note that this property is different from the another environment property <tt>com.sun.jndi.ldap.connect.timeout</tt> that sets the timeout for connecting to the server. The read timeout applies to the LDAP response from the server after the initial connection is established with the server.
