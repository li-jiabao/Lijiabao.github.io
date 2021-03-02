
# Search Results

When you use the search methods in the 
[<tt>DirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) interface, you get back a 
[<tt>NamingEnumeration</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html). Each item in <tt>NamingEnumeration</tt> is a 
[<tt>SearchResult</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchResult.html), which contains the following information:

- [Name](#NAME)
- [Object](#OBJ)
- [Class name](#CLASS)
- [Attributes](#ATTRS)

## <a name="NAME" id="NAME">Name</a>

Each <tt>SearchResult</tt> contains the name of the LDAP entry that satisfied the search filter. You obtain the name of the entry by using 
[<tt>getName()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getName--). This method returns the 
[<tt>composite name</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/CompositeName.html) of the LDAP entry **relative** to the **target context**. The target context is the context to which the <tt>name</tt> parameter resolves. In LDAP parlance, the target context is the **base object** for the search. Here's an example.

```

NamingEnumeration answer = ctx.search("ou=NewHires", 
    "(&amp;(mySpecialKey={0}) (cn=*{1}))",  // Filter expression
    new Object[]{key, name},                // Filter arguments
    null);                                  // Default search controls

```

The target context in this example is that named by <tt>"ou=NewHires"</tt>. The names in <tt>SearchResult</tt>s in <tt>answer</tt> are relative to <tt>"ou=NewHires"</tt>. For example, if <tt>getName()</tt> returns <tt>"cn=J. Duke"</tt>, then its name relative to <tt>ctx</tt> will be <tt>"cn=J. Duke, ou=NewHires"</tt>.

If you performed the search by using 
[<tt>SearchControls.SUBTREE_SCOPE</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#SUBTREE_SCOPE) or 
[<tt>SearchControls.OBJECT_SCOPE</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#OBJECT_SCOPE) and the target context itself satisfied the search filter, then the name returned will be "" (the empty name) because that is the name relative to the target context.

This isn't the whole story. If the search involves referrals (see the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/index.html)) or dereferencing aliases (see the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/aliases.html) ), then the corresponding <tt>SearchResult</tt>s will have names that are not relative to the target context. Instead, they will be URLs that refer directly to the entry. To determine whether the name returned by <tt>getName()</tt> is relative or absolute, use 
[<tt>isRelative()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#isRelative--). If this method returns <tt>true</tt>, then the name is relative to the target context; if it returns <tt>false</tt>, the name is a URL.

If the name is a URL and you need to use that URL, then you can pass it to the initial context, which understands URLs (see the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/url.html)).

If you need to get the entry's full DN, you can use 
[<tt>NameClassPair.getNameInNamespace()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getNameInNamespace--).

## <a name="OBJ" id="OBJ">Object</a>

If the search was conducted requesting that the entry's object be returned 
[(<tt>SearchControls.setReturningObjFlag()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setReturningObjFlag-boolean-) was invoked with <tt>true</tt>), then <tt>SearchResult</tt> will contain an object that represents the entry. To retrieve this object, you invoke 
[<tt>getObject()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Binding.html#getObject--). If a <tt>java.io.Serializable</tt>, 
[<tt>Referenceable</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Referenceable.html), or 
[<tt>Reference</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Reference.html) object was previously bound to that LDAP name, then the attributes from the entry are used to reconstruct that object (see the example in the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/objects/reading/search.html)). Otherwise, the attributes from the entry are used to create a <tt>DirContext</tt> instance that represents the LDAP entry. In either case, the LDAP provider invokes 
[<tt>DirectoryManager.getObjectInstance()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/spi/DirectoryManager.html#getObjectInstance-java.lang.Object-javax.naming.Name-javax.naming.Context-java.util.Hashtable-javax.naming.directory.Attributes-) on the object and returns the results.

## <a name="CLASS" id="CLASS">Class Name</a>

If the search was conducted requesting that the entry's object be returned, then the class name is derived from the returned object. If the search requested attributes that included the retrieval of the <tt>"javaClassName"</tt> attribute of the LDAP entry, then the class name is the value of that attribute. Otherwise, the class name is <tt>"javax.naming.directory.DirContext"</tt>. The class name is obtained from 
[<tt>getClassName()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getClassName--).

## <a name="ATTRS" id="ATTRS">Attributes</a>

When you perform a search, you can select the return attributes either by supplying a parameter to one of the 
[<tt>search()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-java.lang.String:A-) methods or by setting the search controls using 
[<tt>SearchControls.setReturningAttributes()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setReturningAttributes-java.lang.String:A-) . If no attributes have been specified explicitly, then all of the LDAP entry's attributes are returned. To specify that no attributes be returned, you must pass an empty array (<tt>new String[0]</tt>).

To retrieve the LDAP entry's attributes, you invoke 
[<tt>getAttributes()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchResult.html#getAttributes--) on the <tt>SearchResult</tt>.

## Response Controls

See the 
[Controls and Extensions](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/response.html) lesson of the JNDI Tutorial for details on how to retrieve a search result's response controls.
