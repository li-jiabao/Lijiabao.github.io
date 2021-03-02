
# Scope

The default 
[<tt>SearchControls</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html) specifies that the search is to be performed in the named context (
[<tt>SearchControls.ONELEVEL_SCOPE</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#ONELEVEL_SCOPE)). This default is used in the examples in the 
[Search Filters section](filter.html).

In addition to this default, you can specify that the search be performed in the **entire subtree** or only in the named object.

## <a name="SCOPE" id="SCOPE">Search the Subtree</a>

A search of the entire subtree searches the named object and all of its descendants. To make the search behave in this way, pass 
[<tt>SearchControls.SUBTREE_SCOPE</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#SUBTREE_SCOPE) to 
[<tt>SearchControls.setSearchScope()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setSearchScope-int-) as follows.

```

// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};
SearchControls ctls = new SearchControls();
ctls.setReturningAttributes(attrIDs);
ctls.setSearchScope(SearchControls.SUBTREE_SCOPE);

// Specify the search filter to match
// Ask for objects that have the attribute "sn" == "Geisel"
// and the "mail" attribute
String filter = "(&amp;(sn=Geisel)(mail=*))";

// Search the subtree for objects by using the filter
NamingEnumeration answer = ctx.search("", filter, ctls);

```


[`This example`](examples/SearchSubtree.java)
 searches the context <tt>ctx</tt>'s subtree for entries that satisfy the specified filter. It finds the entry <tt>"cn= Ted Geisel, ou=People"</tt> in this subtree that satisfies the filter.

```

# java SearchSubtree
&gt;&gt;&gt;cn=Ted Geisel, ou=People
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```

## Search the Named Object

You can also search the named object. This is useful, for example, to test whether the named object satisfies a search filter. To search the named object, pass 
[<tt>SearchControls.OBJECT_SCOPE</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#OBJECT_SCOPE) to <tt>setSearchScope()</tt>.

```

// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};
SearchControls ctls = new SearchControls();
ctls.setReturningAttributes(attrIDs);
ctls.setSearchScope(SearchControls.OBJECT_SCOPE);

// Specify the search filter to match
// Ask for objects that have the attribute "sn" == "Geisel"
// and the "mail" attribute
String filter = "(&amp;(sn=Geisel)(mail=*))";

// Search the subtree for objects by using the filter
NamingEnumeration answer = 
    ctx.search("cn=Ted Geisel, ou=People", filter, ctls);

```


[`This example`](examples/SearchObject.java) tests whether the object <tt>"cn=Ted Geisel, ou=People"</tt> satisfies the given filter.

```

# java SearchObject
&gt;&gt;&gt;
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```

The example found one answer and printed it. Notice that the name of the result is the empty string. This is because the name of the object is always named relative to the context of the search (in this case, <tt>"cn=Ted Geisel, ou=People"</tt>).
