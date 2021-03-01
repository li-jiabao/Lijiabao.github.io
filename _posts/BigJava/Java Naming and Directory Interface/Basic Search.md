
# Filters

In addition to specifying a search using a set of attributes, you can specify a search in the form of a **search filter**. A search filter is a search query expressed in the form of a logical expression. The syntax of search filters accepted by 
[<tt>DirContext.search()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-) is described in 
[RFC 2254](http://www.ietf.org/rfc/rfc2254.txt).

The following search filter specifies that the qualifying entries must have an <tt>"sn"</tt> attribute with a value of <tt>"Geisel"</tt> and a <tt>"mail"</tt> attribute with any value:

```

(&amp;(sn=Geisel)(mail=*))

```

The following code creates a filter and default 
[<tt>SearchControls</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html), and uses them to perform a search. The search is equivalent to the one presented in the 
[basic search](basicsearch.html) example.

```

// Create the default search controls
SearchControls ctls = new SearchControls();

// Specify the search filter to match
// Ask for objects that have the attribute "sn" == "Geisel"
// and the "mail" attribute
String filter = "(&amp;(sn=Geisel)(mail=*))";

// Search for objects using the filter
NamingEnumeration answer = ctx.search("ou=People", filter, ctls);

```

Running 
[`this example`](examples/SearchWithFilterRetAll.java) produces the following result.

```

# java SearchWithFilterRetAll
&gt;&gt;&gt;cn=Ted Geisel
attribute: sn
value: Geisel
attribute: objectclass
value: top
value: person
value: organizationalPerson
value: inetOrgPerson
attribute: jpegphoto
value: [B@1dacd75e
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: facsimiletelephonenumber
value: +1 408 555 2329
attribute: cn
value: Ted Geisel
attribute: telephonenumber
value: +1 408 555 5252

```

## Quick Overview of Search Filter Syntax

The search filter syntax is basically a logical expression in prefix notation (that is, the logical operator appears before its arguments). The following table lists the symbols used for creating filters.
<th id="h1" width="30%">Symbol</th><th id="h2">Description</th>
<td headers="h1">&amp;</td><td headers="h2">conjunction (i.e., **and** -- all in list must be true)</td>
<td headers="h1">|</td><td headers="h2">disjunction (i.e., **or** -- one or more alternatives must be true)</td>
<td headers="h1">!</td><td headers="h2">negation (i.e., **not** -- the item being negated must not be true)</td>
<td headers="h1">=</td><td headers="h2">equality (according to the matching rule of the attribute)</td>
<td headers="h1">~=</td><td headers="h2">approximate equality (according to the matching rule of the attribute)</td>
<td headers="h1">&gt;=</td><td headers="h2">greater than (according to the matching rule of the attribute)</td>
<td headers="h1">&lt;=</td><td headers="h2">less than (according to the matching rule of the attribute)</td>
<td headers="h1">=*</td><td headers="h2">presence (i.e., the entry must have the attribute but its value is irrelevant)</td>
<td headers="h1">*</td><td headers="h2">wildcard (indicates zero or more characters can occur in that position); used when specifying attribute values to match</td>
<td headers="h1">\</td><td headers="h2">escape (for escaping '*', '(', or ')' when they occur inside an attribute value)</td>

Each item in the filter is composed using an attribute identifier and either an attribute value or symbols denoting the attribute value. For example, the item <tt>"sn=Geisel"</tt> means that the <tt>"sn"</tt> attribute must have the attribute value <tt>"Geisel"</tt> and the item <tt>"mail=*"</tt> indicates that the <tt>"mail"</tt> attribute must be present.

Each item must be enclosed within a set of parentheses, as in <tt>"(sn=Geisel)"</tt>. These items are composed using logical operators such as "&amp;" (conjunction) to create logical expressions, as in <tt>"(&amp; (sn=Geisel) (mail=*))"</tt>.

Each logical expression can be further composed of other items that themselves are logical expressions, as in <tt>"(| (&amp; (sn=Geisel) (mail=*)) (sn=L*))"</tt>. This last example is requesting either entries that have both a <tt>"sn"</tt> attribute of <tt>"Geisel"</tt> and the <tt>"mail"</tt> attribute or entries whose <tt>"sn"</tt> attribute begins with the letter "L."

For a complete description of the syntax, see 

[RFC 2254](http://ietf.org/rfc/rfc2254.txt).

## <a name="SELECT" id="SELECT">Returning Selected Attributes</a>

The previous example returned all attributes associated with the entries that satisfy the specified filter. You can select the attributes to return by setting the search controls argument. You create an array of attribute identifiers that you want to include in the result and pass it to 
[<tt>SearchControls.setReturningAttributes()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setReturningAttributes-java.lang.String:A-). Here's an example.

```

// Specify the ids of the attributes to return
String[] attrIDs = {"sn", "telephonenumber", "golfhandicap", "mail"};
SearchControls ctls = new SearchControls();
ctls.setReturningAttributes(attrIDs);

```

This example is equivalent to the 
[Returning Selected Attributes](basicsearch.html#SELECT) example in the Basic Search section. Running 
[`this example`](examples/SearchWithFilter.java) produces the following results. (The entry does not have a <tt>"golfhandicap"</tt> attribute, so it is not returned.)

```

# java SearchWithFilter
&gt;&gt;&gt;cn=Ted Geisel
attribute: sn
value: Geisel
attribute: mail
value: Ted.Geisel@JNDITutorial.example.com
attribute: telephonenumber
value: +1 408 555 5252

```
