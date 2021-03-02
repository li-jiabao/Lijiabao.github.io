
# Manipulating LdapName (Distinguished Name)

The Distinguished Name (DN) is used by LDAP in its string representation. The string format used to represent a DN is specified in 
[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt). The DN is made up of components called the Relative Distinguished Names (RDN). Below is an example of a DN:

"CN=John Smith, O=Isode Limited, C=GB"

It consist of the following RDNs:

- CN=John Smith
- O=Isode Limited
- C=GB

The classes below represent the DN and RDN respectively.

<li>
[<tt>javax.naming.ldap.LdapName</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html)</li>
<li>
[<tt>javax.naming.ldap.Rdn</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html)</li>

The LdapName class implements the 
[<tt>javax.naming.Name</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Name.html) interface similar to the 
[<tt>javax.naming.CompoundName</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Name.html) and 
[<tt>javax.naming.CompositeName</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/CompositeName.html) classes.

LdapName and Rdn classes allow easy manipulation of DNs and RDNs. Using these APIs it's easy to construct an RDN by pairing up names and values. A DN can be constructed with a list of RDNs. Similarly the individual components of DN and RDN can be retrieved from their string representation.

## LdapName

An LdapName can be created with its string representation as defined in 

[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt) or with a list of Rdns. When the former way is used, the specified string is parsed as per the rules defined in RFC2253. An 
[<tt>InvalidNameException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/InvalidNameException.html) is thrown if the string is not a valid DN. Here's an example that uses the constructor to parse an LDAP name and print its components.

```

String name = "cn=Mango,ou=Fruits,o=Food";
try {
    LdapName dn = new LdapName(name);
    System.out.println(dn + " has " + dn.size() + " RDNs: ");
    for (int i = 0; i &lt; dn.size(); i++) {
        System.out.println(dn.get(i));
    }
} catch (InvalidNameException e) {
    System.out.println("Cannot parse name: " + name);
}

```

Running this example with an input of <tt>"cn=Mango,ou=Fruits,o=Food"</tt> produces the following results:

```

cn=Mango,ou=Fruits,o=Food has 3 RDNs: 
o=Food
ou=Fruits
cn=Mango

```

The LdapName class contains methods to access its components as RDNs and strings, to modify an LdapName, to compare two LdapNames for equality, and to get a string representation of the name.

### Accessing the name components of an LDAP name:

Here are the methods that you can use to access the name components as RDNs and strings:


[<tt>getRdn(int posn)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getRdn-int-)<br />
[<tt>get(int posn)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#get-int-)<br />
[<tt>getRdns()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getRdns--)<br />
[<tt>getAll()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getAll--)<br />
[<tt>getPrefix(int posn)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getPrefix-intposn-)<br />
[<tt>getSuffix(int posn)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getSuffix-intposn-)<br />
[<tt>clone()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#clone--)

To retrieve the component at a particular position within an LdapName, you use getRdn() or get(). The previous constructor example shows an example of its use. 
[<tt>getRdns()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getRdns--) returns a list of all the RDNs and 
[<tt>getAll()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getAll--) returns all of the components of an LdapName as an enumeration.

The right most RDN is at index 0, and the left most RDN is at index n-1. For example, the distinguished name: <tt>"cn=Mango, ou=Fruits, o=Food"</tt> is numbered in the following sequence ranging from 0 to 2: <tt>{o=Food, ou=Fruits, cn=Mango}</tt>

You can also get a LdapNames's suffix or prefix as a LdapName instance. Here's an 
[`example`](examples/LdapNameSuffixPrefix.java) that gets the suffix and prefix of an LDAP name.

```

LdapName dn = new LdapName("cn=Mango, ou=Fruits, o=Food");
Name suffix = dn.getSuffix(1);  // 1 &lt;= index &lt; cn.size()
Name prefix = dn.getPrefix(1);  // 0 &lt;= index &lt; 1

```

When you run this program, it generates the following output:

```

cn=Mango ou=Fruits
o=Food 

```

To make a copy of an LdapName, you use clone().

### Modifying an LDAP name

Following are the methods that you can use to modify an LDAP name:


[<tt>add(Rdn rdn)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#add-Rdn-)<br />
[<tt>add(String comp)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#add-String-)<br />
[<tt>add(int posn, String comp)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#add-int-String-)<br />
[<tt>addAll(List suffixRdns)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#addAll-List-)<br />
[<tt>addAll(Name suffix)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#addAll-Namesuffix-)<br />
[<tt>addAll(int posn, List suffixRdns)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#addAll-int-List-)<br />
[<tt>addAll(int posn, Name suffix)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#addAll-int-Name-)<br />
[<tt>remove(int posn)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#remove-int-)

After creating an LdapName instance, you can add and remove components from it. Here's an 

[`example`](examples/ModifyLdapName.java)
 that appends an LdapName to an existing LdapName, adds components to the front and the end, and removes the second component.

```

     LdapName dn = new LdapName("ou=Fruits,o=Food");
     LdapName dn2 = new LdapName("ou=Summer");
     System.out.println(dn.addAll(dn2)); // ou=Summer,ou=Fruits,o=Food
     System.out.println(dn.add(0, "o=Resources")); 
// ou=Summer,ou=Fruits,o=Food,o=Resources
     System.out.println(dn.add("cn=WaterMelon")); 
// cn=WaterMelon,ou=Summer,ou=Fruits,o=Food,o=Resources
     System.out.println(dn.remove(1));  // o=Food
     System.out.println(dn);  
// cn=WaterMelon,ou=Summer,ou=Fruits,o=Resources

```

### Comparing an LDAP name

Following are the methods that you can use to compare two LDAP names:


[<tt>compareTo(Object name)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#compareTo-Object-)<br />
[<tt>equals(Object name)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#equals-Object-)<br />
[<tt>endsWith(List)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#endsWith-List-)<br />
[<tt>endWith(Name name)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#endsWith-Name-)<br />
[<tt>startsWith(List rdns)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#startsWith-iList-)<br />
[<tt>startsWith(Name name)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#startsWith-Name-)<br />
[<tt>isEmpty()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#isEmpty--)

You can use <tt>compareTo()</tt> to sort a list of LdapName instances. The <tt>equals()</tt> method lets you determine whether two LdapNames are syntactically equal. Two LdapNames are equal if they both have the same (case-exact matched) components in the same order.

With <tt>startsWith()</tt> and <tt>endsWith()</tt>, you can learn whether an LdapName starts or ends with another LdapName; that is, whether an LdapName is a suffix or prefix of another LdapName.

The convenience method <tt>isEmpty()</tt> enables you to determine whether an LdapName has zero components. You can also use the expression <tt>size() == 0</tt> to perform the same check.

Here is an example,
[`CompareLdapNames`](examples/CompareLdapNames.java), that uses some of these comparison methods.

```

LdapName one = new LdapName("cn=Vincent Ryan, ou=People, o=JNDITutorial");
LdapName two = new LdapName("cn=Vincent Ryan");
LdapName three = new LdapName("o=JNDITutorial");
LdapName four = new LdapName("");

System.out.println(one.equals(two));        // false
System.out.println(one.startsWith(three));  // true
System.out.println(one.endsWith(two));      // true
System.out.println(one.startsWith(four));   // true
System.out.println(one.endsWith(four));     // true
System.out.println(one.endsWith(three));    // false
System.out.println(one.isEmpty());          // false
System.out.println(four.isEmpty());         // true
System.out.println(four.size() == 0);       // true

```

### Getting the String Representation

The method below gets you the string representation of an LDAP name formatted according to the syntax specified in RFC 2253:


[<tt>toString()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#toString--)

When you use the LdapName constructor, you supply the string representation of an LDAP name and get back an LdapName instance. To do the reverse, that is, to get the string representation of an LdapName instance, you use <tt>toString()</tt>. The result of <tt>toString()</tt> can be fed back to the constructor to produce a LdapName instance that is equal to the original LdapName instance. Here's an example,

[`LdapNametoString`](examples/LdapNametoString.java)
:

```

LdapName dn = new LdapName(name);
String str = dn.toString();
System.out.println(str);
LdapName dn2 = new LdapName(str);
System.out.println(dn.equals(dn2));  // true

```

### LdapName as an Argument to Context Methods

The Context method require either a composite or a compound name passed as argument to its methods. Hence an LdapName can be directly passed to a context method as shown in
[`LookupLdapName`](examples/LookupLdapName.java):

```

// Create the initial context
Context ctx = new InitialContext(env);

// An LDAP name
LdapName dn = new LdapName("ou=People,o=JNDITutorial");

// Perform the lookup using the dn
Object obj = ctx.lookup(dn);

```

Similarly, when the Context methods return results from list(), listBindings(), or search() operations, the DN can be retrieved by calling 
[<tt>getNameInNamespace()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html#getNameInNamepspace--). The LdapName can be constructed directly from the DN as shown in the example,
[`RetrievingLdapName`](examples/RetrievingLdapName.java):

```

while (answer.hasMore()) {
    SearchResult sr = (SearchResult) answer.next();
    String name = sr.getNameInNamespace();
    System.out.println(name);
    LdapName dn = new LdapName(name);

    // do something with the dn

```
