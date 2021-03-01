
# Rename

You can rename an object in a context by using 
[<tt>Context.rename()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rename-javax.naming.Name-javax.naming.Name-).

```

// Rename to Scott S
ctx.rename("cn=Scott Seligman", "cn=Scott S");

```


[`This example`](examples/Rename.java) renames the object that was bound to <tt>"cn=Scott Seligman"</tt> to <tt>"cn=Scott S"</tt>. After verifying that the object got renamed, the program renames it to its original name (<tt>"cn=Scott Seligman"</tt>), as follows.

```

// Rename back to Scott Seligman
ctx.rename("cn=Scott S", "cn=Scott Seligman");

```

For more examples on renaming of LDAP entries check out the 

[Advanced Topics for LDAP users](../ldap/rename.html)
 lesson.
