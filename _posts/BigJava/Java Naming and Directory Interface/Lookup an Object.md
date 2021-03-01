
# List the Context

Instead of getting a single object at a time, as with 
[<tt>Context.lookup()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-) , you can list an entire context by using a single operation. There are two methods for listing a context: one that returns the bindings and one that returns only the name-to-object class name pairs.

## The Context.List() Method


[<tt>Context.list()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#list-javax.naming.Name-) returns an enumeration of 
[<tt>NameClassPair</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html). Each <tt>NameClassPair</tt> consists of the object's name and its class name. The following code fragment lists the contents of the <tt>"ou=People"</tt> directory (i.e., the files and directories found in <tt>"ou=People"</tt> directory).

```

NamingEnumeration list = ctx.list("ou=People");

while (list.hasMore()) {
    NameClassPair nc = (NameClassPair)list.next();
    System.out.println(nc);
}

```

Running 
[`this example`](examples/List.java) yields the following output.

```

# java List
cn=Jon Ruiz: javax.naming.directory.DirContext
cn=Scott Seligman: javax.naming.directory.DirContext
cn=Samuel Clemens: javax.naming.directory.DirContext
cn=Rosanna Lee: javax.naming.directory.DirContext
cn=Maxine Erlund: javax.naming.directory.DirContext
cn=Niels Bohr: javax.naming.directory.DirContext
cn=Uri Geller: javax.naming.directory.DirContext
cn=Colleen Sullivan: javax.naming.directory.DirContext
cn=Vinnie Ryan: javax.naming.directory.DirContext
cn=Rod Serling: javax.naming.directory.DirContext
cn=Jonathan Wood: javax.naming.directory.DirContext
cn=Aravindan Ranganathan: javax.naming.directory.DirContext
cn=Ian Anderson: javax.naming.directory.DirContext
cn=Lao Tzu: javax.naming.directory.DirContext
cn=Don Knuth: javax.naming.directory.DirContext
cn=Roger Waters: javax.naming.directory.DirContext
cn=Ben Dubin: javax.naming.directory.DirContext
cn=Spuds Mackenzie: javax.naming.directory.DirContext
cn=John Fowler: javax.naming.directory.DirContext
cn=Londo Mollari: javax.naming.directory.DirContext
cn=Ted Geisel: javax.naming.directory.DirContext

```

## The Context.listBindings() Method


[<tt>Context.listBindings()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#listBindings-javax.naming.Name-) returns an enumeration of 
[<tt>Binding</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Binding.html). <tt>Binding</tt> is a subclass of <tt>NameClassPair</tt>. A binding contains not only the object's name and class name, but also the object. The following code enumerates the <tt>"ou=People"</tt> context, printing out each binding's name and object.

```

NamingEnumeration bindings = ctx.listBindings("ou=People");

while (bindings.hasMore()) {
    Binding bd = (Binding)bindings.next();
    System.out.println(bd.getName() + ": " + bd.getObject());
}

```

Running 
[`this example`](examples/ListBindings.java) yields the following output.

```

# java ListBindings
cn=Jon Ruiz: com.sun.jndi.ldap.LdapCtx@1d4c61c
cn=Scott Seligman: com.sun.jndi.ldap.LdapCtx@1a626f
cn=Samuel Clemens: com.sun.jndi.ldap.LdapCtx@34a1fc
cn=Rosanna Lee: com.sun.jndi.ldap.LdapCtx@176c74b
cn=Maxine Erlund: com.sun.jndi.ldap.LdapCtx@11b9fb1
cn=Niels Bohr: com.sun.jndi.ldap.LdapCtx@913fe2
cn=Uri Geller: com.sun.jndi.ldap.LdapCtx@12558d6
cn=Colleen Sullivan: com.sun.jndi.ldap.LdapCtx@eb7859
cn=Vinnie Ryan: com.sun.jndi.ldap.LdapCtx@12a54f9
cn=Rod Serling: com.sun.jndi.ldap.LdapCtx@30e280
cn=Jonathan Wood: com.sun.jndi.ldap.LdapCtx@16672d6
cn=Aravindan Ranganathan: com.sun.jndi.ldap.LdapCtx@fd54d6
cn=Ian Anderson: com.sun.jndi.ldap.LdapCtx@1415de6
cn=Lao Tzu: com.sun.jndi.ldap.LdapCtx@7bd9f2
cn=Don Knuth: com.sun.jndi.ldap.LdapCtx@121cc40
cn=Roger Waters: com.sun.jndi.ldap.LdapCtx@443226
cn=Ben Dubin: com.sun.jndi.ldap.LdapCtx@1386000
cn=Spuds Mackenzie: com.sun.jndi.ldap.LdapCtx@26d4f1
cn=John Fowler: com.sun.jndi.ldap.LdapCtx@1662dc8
cn=Londo Mollari: com.sun.jndi.ldap.LdapCtx@147c5fc
cn=Ted Geisel: com.sun.jndi.ldap.LdapCtx@3eca90

```

## <a name="CLOSE" id="CLOSE">Terminating a NamingEnumeration</a>

A 
[<tt>NamingEnumeration</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html) can be terminated in one of three ways: naturally, explicitly, or unexpectedly.

<li>When 
[<tt>NamingEnumeration.hasMore()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html#hasMore--) returns <tt>false</tt>, the enumeration is complete and effectively terminated.</li>
<li>You can terminate an enumeration explicitly before it has completed by invoking 
[<tt>NamingEnumeration.close()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html#close--). Doing this provides a hint to the underlying implementation to free up any resources associated with the enumeration.</li>
<li>If either <tt>hasMore()</tt> or 
[<tt>next()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html#next--) throws a 
[<tt>NamingException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html), then the enumeration is effectively terminated.</li>

Regardless of how an enumeration has been terminated, once terminated it can no longer be used. Invoking a method on a terminated enumeration yields an undefined result.

## Why Two Different List Methods?

<tt>list()</tt> is intended for browser-style applications that just want to display the names of objects in a context. For example, a browser might list the names in a context and wait for the user to select one or a few of the names displayed to perform further operations. Such applications typically do not need access to all of the objects in a context.

<tt>listBindings()</tt> is intended for applications that need to perform operations on the objects in a context en masse. For example, a backup application might need to perform "file stats" operations on all of the objects in a file directory. Or a printer administration program might want to restart all of the printers in a building. To perform such operations, these applications need to obtain all of the objects bound in a context. Thus it is more expedient to have the objects returned as part of the enumeration.

The application can use either <tt>list()</tt> or the potentially more expensive <tt>listBindings()</tt>, depending on the type of information it needs.
