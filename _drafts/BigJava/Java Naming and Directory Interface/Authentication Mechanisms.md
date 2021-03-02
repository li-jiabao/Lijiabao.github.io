
# Anonymous

As just stated, the default authentication mechanism is <tt>"none"</tt> if no authentication environment properties have been set. If the client sets the 
[<tt>Context.SECURITY_AUTHENTICATION</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_AUTHENTICATION) environment property to <tt>"none"</tt>, then the authentication mechanism is <tt>"none"</tt> and all other authentication environment properties are ignored. You would want to do this explicitly only to ensure that any other authentication properties that might have been set are ignored. In either case, the client will be treated as an **anonymous** client. This means that the server does not know or care who the client is and will allow the client to access (read and update) any data that has been configured to be accessible by any unauthenticated client.

Because none of the directory examples in the 
[Naming and Directory Operations](../ops/index.html) lesson set any of the authentication environment properties, all of them use anonymous authentication.

Here is 
[`an example`](examples/None.java)
 that explicitly sets the <tt>Context.SECURITY_AUTHENTICATION</tt> property to <tt>"none"</tt> (even though doing this is not strictly necessary because that is the default).

```

// Set up the environment for creating the initial context
Hashtable&lt;String, Object&gt; env = new Hashtable&lt;String, Object&gt;();
env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

// Use anonymous authentication
env.put(Context.SECURITY_AUTHENTICATION, "none");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// ... do something useful with ctx

```
