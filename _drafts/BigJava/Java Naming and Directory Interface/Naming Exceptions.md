
# Lookup an Object

To look up an object from the naming service, use 
[<tt>Context.lookup()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-) and pass it the name of the object that you want to retrieve. Suppose that there is an object in the naming service with the name <tt>cn=Rosanna Lee,ou=People</tt>. To retrieve the object, you would write

```

Object obj = ctx.lookup("cn=Rosanna Lee,ou=People");

```

The type of object that is returned by <tt>lookup()</tt> depends both on the underlying naming system and on the data associated with the object itself. A naming system can contain many different types of objects, and a lookup of an object in different parts of the system might yield objects of different types. In this example, <tt>"cn=Rosanna Lee,ou=People"</tt> happens to be bound to a context object (<tt>javax.naming.ldap.LdapContext</tt>). You can cast the result of <tt>lookup()</tt> to its target class.

For example, the following code looks up the object <tt>"cn=Rosanna Lee,ou=People"</tt> and casts it to <tt>LdapContext</tt>.

```

import javax.naming.ldap.LdapContext;
...
LdapContext ctx = (LdapContext) ctx.lookup("cn=Rosanna Lee,ou=People");

```

The complete example is in the file 
[`<tt>Lookup.java</tt>`](examples/Lookup.java).

There are two new static methods available in Java SE 6 to lookup a name:

<li>
[<tt>InitialContext.doLookup(Name name)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html#doLookup-javax.naming.Name-)</li>
<li>
[<tt>InitialContext.doLookup(String name)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/InitialContext.html#doLookup-java.lang.String-)</li>

These methods provide a shortcut way of looking up a name without instantiating an InitialContext.
