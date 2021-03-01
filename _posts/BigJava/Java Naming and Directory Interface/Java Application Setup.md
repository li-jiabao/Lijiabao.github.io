
# Lesson: Naming and Directory Operations

You can use the JNDI to perform naming operations, including read operations and operations for updating the namespace. The following operations are described in this lesson:

- [Looking up an object](lookup.html)
- [Listing the contents of a context](list.html)
- [Adding, overwriting, and removing a binding](bind.html)
- [Renaming an object](rename.html)
- [Creating and destroying subcontexts](create.html)

## Configuration

Before performing any operation on a naming or directory service, you need to acquire an **initial context**--the starting point into the namespace. This is because all methods on naming and directory services are performed relative to some context. To get an initial context, you must follow these steps.

1. Select the service provider of the corresponding service you want to access.
1. Specify any configuration that the initial context needs.
<li>Call the 
[<tt>InitialContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html#constructor_detail) constructor.</li>

## Step1: Select the Service Provider for the Initial Context

You can specify the service provider to use for the initial context by creating a set of **environment properties** (a <tt>Hashtable</tt>) and adding the name of the service provider class to it. Environment properties are described in detail in the 

[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/beyond/env/index.html).

If you are using the LDAP service provider included in the JDK, then your code would look like the following.

```

Hashtable&lt;String, Object&gt; env = new Hashtable&lt;String, Object&gt;();
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "com.sun.jndi.ldap.LdapCtxFactory");

```

To specify the file system service provider in the JDK, you would write code that looks like the following.

```

Hashtable&lt;String, Object&gt; env = new Hashtable&gt;String, Object&gt;();
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "com.sun.jndi.fscontext.RefFSContextFactory");

```

You can also use **system properties** to specify the service provider to use. Check out the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/beyond/index.html) for details.

## Step2: Supply the Information Needed by the Initial Context

Clients of different directories might need various information for contacting the directory. For example, you might need to specify on which machine the server is running and what information is needed to identify the user to the directory. Such information is passed to the service provider via environment properties. The JNDI specifies some generic environment properties that service providers can use. Your service provider documentation will give details on the information required for these properties.

The LDAP provider requires that the program specify the location of the LDAP server, as well as user identity information. To provide this information, you would write code that looks as follows.

```

env.put(Context.PROVIDER_URL, "ldap://ldap.wiz.com:389");
env.put(Context.SECURITY_PRINCIPAL, "joeuser");
env.put(Context.SECURITY_CREDENTIALS, "joepassword");

```

This tutorial uses the LDAP service provider in the JDK. The examples assume that a server has been set up on the local machine at port 389 with the root-distinguished name of <tt>"o=JNDITutorial"</tt> and that no authentication is required for updating the directory. They include the following code for setting up the environment.

```

env.put(Context.PROVIDER_URL, "ldap://localhost:389/o=JNDITutorial");

```

If you are using a directory that is set up differently, then you will need to set up these environment properties accordingly. You will need to replace <tt>"localhost"</tt> with the name of that machine. You can run these examples against any 
[public directory servers](../software/index.html) or your own server running on a different machine. You will need to replace <tt>"localhost"</tt> with the name of that machine and replace <tt>o=JNDITutorial</tt> with the naming context that is available.

## Step3: Creating the Initial Context

You are now ready to create the initial context. To do that, you pass to the 
[<tt>InitialContext</tt> constructor](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html#constructor_detail) the environment properties that you created previously:

```

Context ctx = new InitialContext(env);

```

Now that you have a reference to a 
[<tt>Context</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html) object, you can begin to access the naming service.

To perform directory operations, you need to use an 
[<tt>InitialDirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html). To do that, use one of its 
[constructors](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InitialDirContext.html#constructor_detail):

```

DirContext ctx = new InitialDirContext(env);

```

This statement returns a reference to a 
[<tt>DirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) object for performing directory operations.
