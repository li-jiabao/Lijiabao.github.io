
# More LDAP Operations

The rest of the LDAP lesson covers how the JNDI provides ability to perform certain interesting LDAP operations.

## Renaming Objects

You use 
[<tt>Context.rename()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rename-javax.naming.Name-javax.naming.Name-) to rename an object in the directory. In the 
[LDAP v2](http://www.ietf.org/rfc/rfc1777.txt), this corresponds to the "modify RDN" operation that renames an entry within the same context (that is, renaming a sibling). In the 
[LDAP v3](http://www.ietf.org/rfc/rfc2251.txt), this corresponds to the "modify DN" operation, which is like "modify RDN," except that the old and new entries need not be in the same context. You can use <tt>Context.rename()</tt> to rename a leaf entry or an interior node. The example shown in the 
[Naming and Directory Operations](../ops/rename.html) lesson renames a leaf entry. The following 
[`code`](examples/RenameInterior.java) renames an interior node from <tt>"ou=NewHires"</tt> to <tt>"ou=OldHires"</tt>:

```

ctx.rename("ou=NewHires", "ou=OldHires");

```

### Renaming to a Different Part of the DIT

With the LDAP v3, you can rename an entry to a different part of the DIT. To do this by using <tt>Context.rename()</tt>, you must use a context that is the common ancestor for both the new and the old entries. For example, to rename <tt>"cn=C. User, ou=NewHires, o=JNDITutorial"</tt> to <tt>"cn=C. User, ou=People, o=JNDITutorial"</tt>, you must use the context named by <tt>"o=JNDITutorial"</tt>. Following is 
[`an example`](examples/RenameDiffParent.java) that demonstrates this. If you try to run this example against an LDAP v2 server, then you will get an 
[<tt>InvalidNameException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/InvalidNameException.html) because version 2 does not support this feature.

```

ctx.rename("cn=C. User, ou=NewHires", "cn=C. User, ou=People");

```

### Keeping the Old Name Attributes

In the LDAP, when you rename an entry, you have the option of keeping the entry's old RDN as an attribute of the updated entry. For example, if you rename the entry <tt>"cn=C. User"</tt> to <tt>"cn=Claude User"</tt>, you can specify whether you want the old RDN <tt>"cn=C. User"</tt> to be kept as an attribute.

To specify whether you want to keep the old name attribute when you use <tt>Context.rename()</tt>, use the <tt>"java.naming.ldap.deleteRDN"</tt> environment property. If this property's value is <tt>"true"</tt> (the default), the old RDN is removed. If its value is <tt>"false"</tt>, then the old RDN is kept as an attribute of the updated entry. The complete example is 
[`here`](examples/RenameKeepRDN.java).

```

// Set the property to keep RDN
env.put("java.naming.ldap.deleteRDN", "false");

// Create the initial context
DirContext ctx = new InitialDirContext(env);

// Perform the rename
ctx.rename("cn=C. User, ou=NewHires", "cn=Claude User,ou=NewHires");

```
