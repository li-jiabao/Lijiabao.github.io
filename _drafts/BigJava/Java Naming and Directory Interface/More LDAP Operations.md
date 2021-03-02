
# LDAP Compare

The LDAP "compare" operation allows a client to ask the server whether the named entry has an attribute/value pair. This allows the server to keep certain attribute/value pairs secret (i.e., not exposed for general "search" access) while still allowing the client limited use of them. Some servers might use this feature for passwords, for example, although it is insecure for the client to pass clear-text passwords in the "compare" operation itself.

To accomplish this in the JNDI, use suitably constrained arguments for the following methods:

<li>
[<tt>search(Name name, String filter, SearchControls ctls)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-)</li>
<li>
[<tt>search(Name name, String filterExpr, Object[]filterArgs, SearchControls ctls)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-java.lang.Object:A-javax.naming.directory.SearchControls-)</li>

1. The filter must be of the form "(**name**=**value**)". You cannot use wild cards.
<li>The search scope must be 
[<tt>SearchControls.OBJECT_SCOPE</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#OBJECT_SCOPE).</li>
1. You must request that no attributes be returned. If these criteria are not met, then these methods will use an LDAP "search" operation instead of an LDAP "compare" operation.

Here's 
[`an example`](examples/Compare.java) that causes an LDAP "compare" operation to be used.

```

// Value of the attribute
byte[] key = {(byte)0x61, (byte)0x62, (byte)0x63, (byte)0x64, 
              (byte)0x65, (byte)0x66, (byte)0x67};

// Set up the search controls
SearchControls ctls = new SearchControls();
ctls.setReturningAttributes(new String[0]);       // Return no attrs
ctls.setSearchScope(SearchControls.OBJECT_SCOPE); // Search object only

// Invoke search method that will use the LDAP "compare" operation
NamingEnumeration answer = ctx.search("cn=S. User, ou=NewHires", 
                                      "(mySpecialKey={0})", 
                                       new Object[]{key}, ctls);

```

If the compare is successful, the resulting enumeration will contain a single item whose name is the empty name and which contains no attributes.
