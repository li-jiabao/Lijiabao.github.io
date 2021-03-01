
# Trouble Shooting Tips

Here are the most common problems that you might encounter when you try to run a successfully compiled program that uses the JNDI classes.

1. [No Initial Context](#1)
1. [Connection Refused](#2)
1. [Connection Fails](#3)
1. [Program Hangs](#4)
1. [Name Not Found](#5)
1. [Cannot Connect to Arbitrary Hosts](#6)
1. [Cannot Access System Properties for Configuration](#7)
1. [Cannot Authenticate by Using CRAM-MD5](#8)

<a style="font-weight: bold" name="1" id="1">1. You get a <tt>NoInitialContextException</tt></a>.

**Cause**: You did not specify which implementation to use for the initial context. Specifically, the 
[<tt>Context.INITIAL_CONTEXT_FACTORY</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#INITIAL_CONTEXT_FACTORY) environment property was not set to the class name of the factory that will create the initial context. Or, you did not make available to the program the classes of the service provider named by <tt>Context.INITIAL_CONTEXT_FACTORY</tt>.

**Solution**: Set the <tt>Context.INITIAL_CONTEXT_FACTORY</tt> environment property to the class name of the initial context implementation that you are using. See 
[Configuration](index.html) section for details.

If the property was set, then make sure that the class name was not mistyped, and that the class named is available to your program (either in its classpath or installed in the <tt>jre/lib/ext</tt> directory of the JRE). The [Java Platform]() includes service providers for LDAP, COS naming, DNS, and the RMI registry. All other service providers must be installed and added to the execution environment.

<a style="font-weight: bold" name="2" id="2">2. You get a <tt>CommunicationException</tt>, indicating "connection refused."</a>

**Cause**: The server and port identified by the <tt>Context.PROVIDER_URL</tt> environment property is not being served by the server. Perhaps someone has disabled or turned off the machine on which the server is running. Or, maybe you mistyped the server's name or port number.

**Solution**: Check that there is indeed a server running on that port, and restart the server if necessary. The way that you perform this check depends on the LDAP server that you are using. Usually, an administrative console or tool is available that you can use to administer the server. You may use that tool to verify the server's status.

<a style="font-weight: bold" name="3" id="3">3. The LDAP server responds to other utilities (such as its administration console) but does not seem to respond to your program's requests.</a>

**Cause**: The server does not respond to LDAP v3 connection requests. Some servers (especially public servers) do not respond correctly to the LDAP v3, ignoring the requests instead of rejecting them. Also, some LDAP v3 servers have problems handling a control that Oracle's LDAP service provider automatically sends and often return a server-specific failure code.

**Solution**. Try setting the environment property <tt>"java.naming.ldap.version"</tt> to <tt>"2"</tt>. The LDAP service provider by default attempts to connect to an LDAP server using the LDAP v3; if that fails, then it uses the LDAP v2. If the server silently ignores the v3 request, then the provider will assume that the request worked. To work around such servers, you must explicitly set the protocol version to ensure proper behavior by the server.

If the server is a v3 server, then try setting the following environment property before creating the initial context:

```

env.put(Context.REFERRAL, "throw");

```

This will turn off the control that the LDAP provider sends automatically. (Check out the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/index.html) for details.)

<a style="font-weight: bold" name="4" id="4">4. The program hangs.</a>

**Causes**: Some servers (especially public ones) won't respond (not even with a negative answer) if you attempt to perform a search that would generate too many results or that would require the server to examine too many entries in order to generate the answer. Such servers are trying to limit the amount of resources that they expend on a per-request basis.

Or, you tried to use Secure Socket Layer (SSL) against a server/port that does not support it, and vice versa (that is, you tried to use a plain socket to talk to an SSL port).

And lastly, the server is either responding very slowly due to heavy load or is not responding at all for some reason.

**Solution**: If your program is hanging because the server is trying to restrict the use of its resources, then retry your request using a query that will return a single result or only a few results. This will help you to determine whether the server is alive. If it is, then you can broaden your initial query and resubmit it.

If your program is hanging because of SSL problems, then you need to find out whether the port is an SSL port and then set the 
[<tt>Context.SECURITY_PROTOCOL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PROTOCOL) environment property appropriately. If the port is an SSL port, then this property should be set to <tt>"ssl"</tt>. If it is not an SSL port, then this property should not be set.

If you program is hanging for none of the above reasons the property <tt>com.sun.jndi.ldap.read.timeout</tt> comes in handy to specify the read timeout. The value of this property is the string representation of an integer representing the read timeout in milliseconds for LDAP operations. If the LDAP provider cannot get a LDAP response within that period, it aborts the read attempt. The integer should be greater than zero. An integer less than or equal to zero means no read timeout is specified which is equivalent to waiting for the response infinitely until it is received.

If this property is not specified, the default is to wait for the response until it is received.

<a>For example,</a>

<a><tt>env.put("com.sun.jndi.ldap.read.timeout", "5000");</tt></a> causes the LDAP service provider to abort the read attempt if the server does not respond with a reply within 5 seconds.

<a style="font-weight: bold" name="5" id="5">5. You get a <tt>NameNotFoundException</tt></a>.

**Causes**: When you initialized the initial context for the LDAP, you supply a root-distinguished name. For example, if you set the <tt>Context.PROVIDER_URL</tt> environment property for the initial context to <tt>"ldap://ldapserver:389/o=JNDITutorial"</tt> and subsequently supplied a name such as <tt>"cn=Joe,c=us"</tt>, then the full name that you passed to the LDAP service was <tt>"cn=Joe,c=us,o=JNDITutorial"</tt>. If this was really the name that you intended, then you should check your server to make sure that it contains such an entry.

Also, the Oracle Directory Server returns this error if you supply an incorrect distinguished name for authentication purposes. For example, the LDAP provider will throw a 
[<tt>NameNotFoundException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameNotFoundException.html) if you set the 
[<tt>Context.SECURITY_PRINCIPAL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL) environment property to <tt>"cn=Admin, o=Tutorial"</tt>, and <tt>"cn=Admin, o=Tutorial"</tt> is not an entry on the LDAP server. The correct error for the Oracle Directory Server to return actually should be something related to authentication, rather than "name not found."

**Solution**: Verify the name that you supplied is that of an entry existing on the server. You can do this by listing the entry's parent context or using some other tool such as the directory server's administration console.

Here are some problems that you might encounter when trying to deploy an applet that uses the JNDI classes.

<a style="font-weight: bold" name="6" id="6">6. You get an <tt>AppletSecurityException</tt> when your applet attempts to communicate with a directory server that is running on a machine different from the one from which the applet was loaded</a>

**Cause**: Your applet was not signed, so it can connect only to the machine from which it was loaded. Or, if the applet was signed, the browser has not granted the applet permission to connect to the directory server machine.

**Solution**: If you want to allow the applet to connect to directory servers running on arbitrary machines, then you need to sign **both** your applet **and** all of the JNDI JARs that your applet will be using. For information on signing jars, see 
[Signing and Verifying JAR files](../../deployment/jar/signindex.html)
 .

<a style="font-weight: bold" name="7" id="7">7. You get an <tt>AppletSecurityException</tt> when your applet attempts to set up the environment properties using system properties.</a>

**Cause**: Web browsers limit access to system properties and throw a <tt>SecurityException</tt> if you attempt to read them.

**Solution**: If you need to obtain input for your applet, then try using applet params instead.

<a style="font-weight: bold" name="8" id="8">8. You get an <tt>AppletSecurityException</tt> when an applet running inside Firefox attempts to authenticate using CRAM-MD5 to the LDAP server.</a>

**Cause**: Firefox disables access to the <tt>java.security</tt> packages. The LDAP provider used the message digest functionality provided by <tt>java.security.MessageDigest</tt> for implementing CRAM-MD5.

**Solution**: Use the Java Plug-in.
