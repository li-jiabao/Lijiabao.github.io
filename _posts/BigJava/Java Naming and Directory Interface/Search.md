
# Basic Search

The simplest form of search requires that you specify the set of attributes that an entry must have and the name of the target context in which to perform the search.

The following code creates an attribute set <tt>matchAttrs</tt>, which has two attributes <tt>"sn"</tt> and <tt>"mail"</tt>. It specifies that the qualifying entries must have a surname (<tt>"sn"</tt>) attribute with a value of <tt>"Geisel"</tt> and a <tt>"mail"</tt> attribute with any value. It then invokes 
[<tt>DirContext.search()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-) to search the context <tt>"ou=People"</tt> for entries that have the attributes specified by <tt>matchAttrs</tt>.

```

// Specify the attributes to match
// Ask for objects that has a surname ("sn") attribute with 
// the value "Geisel" and the "mail" attribute

// ignore attribute name case
Attributes matchAttrs = new BasicAttributes(true); 
matchAttrs.put(new BasicAttribute("sn", "Geisel"));
matchAttrs.put(new BasicAttribute("mail"));

// Search for objects that have those matching attributes
NamingEnumeration answer = ctx.search("ou=People", matchAttrs);

```

You can then print the results as follows.

```

while (answer.hasMore()) {
    SearchResult sr = (SearchResult)answer.next();
    System.out.println("&gt;&gt;&gt;" + sr.getName());
    printAttrs(sr.getAttributes());
}

```

<tt>printAttrs()</tt>is similar to the code in 
[the <tt>getAttributes()</tt>]() example that prints an attribute set.

Running 
[`this example`](examples/SearchRetAll.java) produces the following result.

```

# java SearchRetAll
&gt;&gt;&gt;cn=Ted Geisel
attribute: sn
value: Geisel
attribute: objectclass
value: top
value: person
value: organizationalPerson
value: inetOrgPerson
attribute: jpegphoto
value: [B@1dacd78b
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: facsimiletelephonenumber
value: +1 408 555 2329
attribute: cn
value: Ted Geisel
attribute: telephonenumber
value: +1 408 555 5252

```

## <a name="SELECT" id="SELECT">Returning Selected Attributes</a>

The previous example returned all attributes associated with the entries that satisfy the specified query. You can select the attributes to return by passing <tt>search()</tt> an array of attribute identifiers that you want to include in the result. After creating the <tt>matchAttrs</tt> as shown previously, you also need to create the array of attribute identifiers, as shown next.

```

// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};

// Search for objects that have those matching attributes
NamingEnumeration answer = ctx.search("ou=People", matchAttrs, attrIDs);

```


[`This example`](examples/Search.java) returns the attributes <tt>"sn"</tt>, <tt>"telephonenumber"</tt>, <tt>"golfhandicap"</tt>, and <tt>"mail"</tt> of entries that have an attribute <tt>"mail"</tt> and have a <tt>"sn"</tt> attribute with the value <tt>"Geisel"</tt>. This example produces the following result. (The entry does not have a <tt>"golfhandicap"</tt> attribute, so it is not returned.)

```

# java Search 
&gt;&gt;&gt;cn=Ted Geisel
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```
