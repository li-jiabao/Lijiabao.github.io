
# Connection Management

JNDI provides a high-level interface for accessing naming and directory services. The mapping between a JNDI 
[`Context`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html) instance and an underlying network connection might not be one-to-one. The service provider is free to share and reuse connections as long as the interface semantics are preserved. The application developer usually does not need to know the details of how <tt>Context</tt> instances create and use connections. These details are useful when the developer needs to tune his program.

This lesson describes how the LDAP service provider uses connections. It describes 
[when connections are created](create.html) and how to specify special connection parameters, such as multiple servers and connection timeouts. This lesson also shows how to dynamically discover and use LDAP servers in network environments that support it.

A connection that is created must eventually be closed. This lesson contains a section that describes 
[connection closures](close.html) by the client and the server.

Finally, this lesson shows you how to use 
[connection pooling](pool.html) to make applications that use many short-lived connections more efficient.
