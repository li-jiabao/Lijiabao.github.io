
# JNDI as an LDAP API

Both the JNDI and LDAP models define a hierarchical namespace in which you name objects. Each object in the namespace may have attributes that can be used to search for the object. At this high level, the two models are similar, so it is not surprising that the JNDI maps well to the LDAP.

## Models

You can think of an LDAP entry as a JNDI 
[<tt>DirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html). Each LDAP entry contains a name and a set of attributes, as well as an optional set of child entries. For example, the LDAP entry <tt>"o=JNDITutorial"</tt> may have as its attributes <tt>"objectclass"</tt> and <tt>"o"</tt>, and it may have as its children <tt>"ou=Groups"</tt> and <tt>"ou=People"</tt>.

In the JNDI, the LDAP entry <tt>"o=JNDITutorial"</tt> is represented as a context with the name <tt>"o=JNDITutorial"</tt> that has two subcontexts, named: <tt>"ou=Groups"</tt> and <tt>"ou=People"</tt>. An LDAP entry's attributes are represented by the 
[<tt>Attributes</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/Attributes.html) interface, whereas individual attributes are represented by the 
[<tt>Attribute</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/Attribute.html) interface.

See 
[the next part of this lesson](operations.html) for details on how the LDAP operations are accessed through the JNDI.

## Names

As a result of federation, the names that you supply to the JNDI's context methods can span multiple namespaces. These are called composite names. When using the JNDI to access an LDAP service, you should be aware that the forward slash character ("/") in a string name has special meaning to the JNDI. If the LDAP entry's name contains this character, then you need to escape it (using the backslash character ("\")). For example, an LDAP entry with the name <tt>"cn=O/R"</tt> must be presented as the string <tt>"cn=O\\/R"</tt> to the JNDI context methods. For more information about Names check out the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/beyond/names/index.html). The 
[LdapName](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html) and 
[Rdn](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html) classes simplify creation and manipulation of LDAP names.

LDAP names as they are used in the protocol are always fully qualified names that identify entries that start from the root of the LDAP namespace (as defined by the server). Following are some examples of fully qualified LDAP names.

```

cn=Ted Geisel, ou=Marketing, o=Some Corporation, c=gb
cn=Vinnie Ryan, ou=People, o=JNDITutorial

```

In the JNDI, however, names are always **relative**; that is, you always name an object relative to a context. For example, you can name the entry <tt>"cn=Vinnie Ryan"</tt> relative to the context named <tt>"ou=People, o=JNDITutorial"</tt>. Or you can name the entry <tt>"cn=Vinnie Ryan, ou=People"</tt> relative to the context named <tt>"o=JNDITutorial"</tt>. Or, you can create an initial context that points at the root of the LDAP server's namespace and name the entry <tt>"cn=Vinnie Ryan, ou=People, o=JNDITutorial"</tt>.

In the JNDI, you can also use LDAP URLs to name LDAP entries. See the LDAP URL discussion in the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/url.html).
