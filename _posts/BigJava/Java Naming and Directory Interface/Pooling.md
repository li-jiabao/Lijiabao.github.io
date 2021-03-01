
# Configuration

Connection pooling is configured and maintained per Java runtime. Connections are not shared across different runtimes. To use connection pooling, no configuration is required. Configuration is necessary only if you want to customize how pooling is done, such as to control the size of the pools and which types of connections are pooled.

You configure connection pooling by using a number of system properties at program startup time. Note that these are system properties, **not** environment properties and that they affect all connection pooling requests.

Here is an example of a command line that sets the maximum pool size to 20, the preferred pool size to 10, and the idle timeout to a minute for pooled connections.

```

# java -Dcom.sun.jndi.ldap.connect.pool.maxsize=20 \
       -Dcom.sun.jndi.ldap.connect.pool.prefsize=10 \
       -Dcom.sun.jndi.ldap.connect.pool.timeout=60000 \
    UsePool

```

The following table lists the system properties for configuring connection pooling. They are described in more detail in the rest of this section.
<th id="h1">System Property Name</th><th id="h2">Description</th><th id="h3">Default</th>
<td headers="h1"><tt>com.sun.jndi.ldap.connect.pool.authentication</tt></td><td headers="h2">A list of space-separated authentication types of connections that may be pooled. Valid types are "none", "simple", and "DIGEST-MD5".</td><td headers="h3"><tt>"none simple"</tt></td>
<td headers="h1"><tt>com.sun.jndi.ldap.connect.pool.debug</tt></td><td headers="h2">A string that indicates the level of debug output to produce. Valid values are <tt>"fine"</tt> (trace connection creation and removal) and "all" (all debugging information).</td><td headers="h3">&#160;</td>
<td headers="h1"><tt>com.sun.jndi.ldap.connect.pool.initsize</tt></td><td headers="h2">The string representation of an integer that represents the number of connections per connection identity to create when initially creating a connection for the identity.</td><td headers="h3">1</td>
<td headers="h1"><tt>com.sun.jndi.ldap.connect.pool.maxsize</tt></td><td headers="h2">The string representation of an integer that represents the maximum number of connections per connection identity that can be maintained concurrently.</td><td headers="h3">no maximum size</td>
<td headers="h1"><tt>com.sun.jndi.ldap.connect.pool.prefsize</tt></td><td headers="h2">The string representation of an integer that represents the preferred number of connections per connection identity that should be maintained concurrently.</td><td headers="h3">no preferred size</td>
<td headers="h1"><tt>com.sun.jndi.ldap.connect.pool.protocol</tt></td><td headers="h2">A list of space-separated protocol types of connections that may be pooled. Valid types are "plain" and "ssl".</td><td headers="h3"><tt>"plain"</tt></td>
<td headers="h1"><tt>com.sun.jndi.ldap.connect.pool.timeout</tt></td><td headers="h2">The string representation of an integer that represents the number of milliseconds that an idle connection may remain in the pool without being closed and removed from the pool.</td><td headers="h3">no timeout</td>

## What Gets Pooled

When you request that a `Context` instance use connection pooling by using the `"com.sun.jndi.ldap.connect.pool"` environment property, the connection that is used might or might not be pooled. The default rule is that plain (non-SSL) connections that use simple or no authentication are allowed to be pooled. You can change this default to include SSL connections and the DIGEST-MD5 authentication type by using system properties. To allow both plain and SSL connections to be pooled, set the <tt>"com.sun.jndi.ldap.connect.pool.protocol"</tt> system property to the string <tt>"plain ssl"</tt>. To allow connections of anonymous (none), simple and DIGEST-MD5 authentication types to be pooled, set the <tt>com.sun.jndi.ldap.connect.pool.authentication</tt> system property to the string <tt>"none simple DIGEST-MD5"</tt>.

There are a couple of environment properties that automatically disqualify a <tt>Context</tt> instance from using a pooled connection. A <tt>Context</tt> instance cannot use a pooled connection if it has its <tt>"java.naming.ldap.factory.socket"</tt> property set to a custom socket factory class, or its <tt>"java.naming.security.sasl.callback"</tt> property set to a custom callback handler class, or its <tt>"com.sun.jndi.ldap.trace.ber"</tt> property set to enable protocol tracing.

## How Connections are Pooled

When a <tt>Context</tt> instance requests to use a pooled connection, the LDAP provider needs to determine whether the request can be satisfied by an existing pooled connection. It does this by assigning a **connection identity** to each pooled connection and checking whether the incoming request has the same connection identity as that of one of its pooled connections.

A connection identity is the set of the parameters required to create a possibly authenticated LDAP connection. Its composition depends on the authentication type of the request, as shown in the following table.
<th id="h101">Authentication Type</th><th id="h102">Connection Identity Contents</th>
<td headers="h101">none</td><td headers="h102">- connection controls- host name, port number as specified in the <tt>"java.naming.provider.url"</tt> property, referral, or URL supplied to the initial context- the contents of the following properties:<pre>`java.naming.security.protocoljava.naming.ldap.version`</pre></td>
<td headers="h101">simple</td><td headers="h102">- all of the information listed for none- the contents of following properties:<pre>`java.naming.security.principaljava.naming.security.credentials`</pre></td>
<td headers="h101">DIGEST-MD5</td><td headers="h102">- all of the information listed for simple- the contents of following properties:<pre>`java.naming.security.sasl.authorizationIdjava.naming.security.sasl.realmjavax.security.sasl.qopjavax.security.sasl.strengthjavax.security.sasl.server.authenticationjavax.security.sasl.maxbufferjavax.security.sasl.policy.noplaintextjavax.security.sasl.policy.noactivejavax.security.sasl.policy.nodictionaryjavax.security.sasl.policy.noanonymousjavax.security.sasl.policy.forwardjavax.security.sasl.policy.credentials`</pre></td>

## Pool Sizes

The LDAP provider maintains pools of connections; each pool holds connections (either in-use or idle) that have the same connection identity. There are three sizes that affect the management of each pool. These sizes are global and affect all pools.

The **initial pool size** is the number of connections per connection identity that the LDAP service provider creates when first creating the pool (which corresponds to when the application first requests a pooled connection for that connection identity). Authentication of each connection in the pool is performed on demand, as the connection gets used. By default, the initial pool size is 1 and can be changed by using the system property <tt>"com.sun.jndi.ldap.connect.pool.initsize"</tt>. It is typically used at application start-up time to prime the pool with a certain number of connections to a server.

The **maximum pool size** is the maximum number of connections per connection identity that can be maintained concurrently by the LDAP service provider. Both in-use and idle connections contribute to this number. When the pool size reaches this number, no new connection for the corresponding connection identity may be created until a connection in the pool has been removed (i.e., the physical connection is closed). When the pool size reaches the maximum and all of the connections in the pool are in use, the application's request for a connection from that pool is blocked until a connection in the pool either becomes idle or is removed. A maximum pool size of 0 means that there is no maximum size: A request for a pooled connection will use an existing pooled idle connection or a newly created pooled connection.

The **preferred pool size** is the preferred number of connections per connection identity that the LDAP service provider should maintain. Both in-use and idle connections contribute to this number. When an application requests the use of a pooled connection and the pool size is less than the preferred size, the LDAP provider will create and use a new pooled connection regardless of whether an idle connection is available. When an application is finished with a pooled connection (by invoking <tt>Context.close()</tt> on all contexts that share the connection) and the pool size is greater than the preferred size, the LDAP provider will close and remove the pooled connection from the pool. A preferred pool size of 0 means that there is no preferred size: A request for a pooled connection will result in a newly created connection only if no idle ones are available.

Note that the maximum pool size overrides both the initial and preferred pool sizes. For example, setting the preferred pool size greater than the maximum pool size is effectively setting it to the maximum pool size.

## Idle Connections

When the application is finished with a pooled connection (by invoking <tt>Context.close()</tt> on all contexts that share the connection), the underlying pooled connection is marked as idle, waiting to be reused. By default, idle connections remain in the pool indefinitely until they are garbage-collected. If the <tt>"com.sun.jndi.ldap.connect.pool.timeout"</tt> system property has been set, the LDAP provider will automatically close and remove pooled connections that have been idle for more than the specified period.
