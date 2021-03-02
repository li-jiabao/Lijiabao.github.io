
# Read Attributes

To read the attributes of an object from the directory, use 
[<tt>DirContext.getAttributes()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#getAttributes-javax.naming.Name-) and pass it the name of the object for which you want the attributes. Suppose that an object in the naming service has the name <tt>"cn=Ted Geisel, ou=People"</tt>. To retrieve this object's attributes, you'll need 
[`code`](examples/GetAllAttrs.java) 
 that looks like this:

```

Attributes answer = ctx.getAttributes("cn=Ted Geisel, ou=People");

```

You can then print the contents of this answer as follows.

```

for (NamingEnumeration ae = answer.getAll(); ae.hasMore();) {
    Attribute attr = (Attribute)ae.next();
    System.out.println("attribute: " + attr.getID());
    /* Print each value */
    for (NamingEnumeration e = attr.getAll(); e.hasMore();
         System.out.println("value: " + e.next()))
        ;
}

```

This produces the following output.

```

# java GetattrsAll
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
attribute: telephonenumber
value: +1 408 555 5252
attribute: cn
value: Ted Geisel

```

## Returning Selected Attributes

To read a selective subset of attributes, you supply an array of strings that are attribute identifiers of the attributes that you want to retrieve.

```

// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};

// Get the attributes requested
Attributes answer = ctx.getAttributes("cn=Ted Geisel, ou=People", attrIDs);

```


[`This example`](examples/GetAllAttrs.java) asks for the <tt>"sn"</tt>, <tt>"telephonenumber"</tt>, <tt>"golfhandicap"</tt> and <tt>"mail"</tt> attributes of the object <tt>"cn=Ted Geisel, ou=People"</tt>. This object has all but the <tt>"golfhandicap"</tt> attribute, and so three attributes are returned in the answer. Following is the output of the example.

```

# java Getattrs
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```
