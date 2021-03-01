
# Manipulating Relative Distringuished Name (RDN)

The class 
[<tt>javax.naming.ldap.Rdn</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html) represents a Relative Distinguished name (RDN) as specified in 
[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt). An RDN represents a component of a DN as explained in the 
[Manipulating LdapName](ldapname.html) lesson. An RDN is made up of type and value pair(s). Examples of RDNs are:

- <tt>OU=Sun</tt>
<li><tt>OU=Sales+CN=J.Smith</tt>.<br />
The above example shows a representation of a multi-valued RDN.</li>

The 
[<tt>Rdn</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html) class provides methods to access name/value pair(s) of an RDN, obtain its string representation, retrieve an 
[<tt>Attributes</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/Attributes.html) view, compare and determine equality of RDNs and methods to escape and unescape the value part of the RDN.

The <tt>Rdn</tt> class is immutable.

## Constructing an Rdn

An Rdn can be constructed with the specified name and value pair, if it's a single name/value paired RDN. For a multi-valued RDN, you should create an attribute set consisting of all the name/value pairs and use a constructor that takes <tt>Attributes</tt> as an argument. You can also create an Rdn from its string representation specified in 
[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt). Finally, you can clone an Rdn using its copy constructor. Here is an 

[`example`](examples/RdnConstructors.java)
 that creates RDNs using different types of constructors.

```

        Rdn rdn1 = new Rdn("ou= Juicy\\, Fruit");
        System.out.println("rdn1:" + rdn1.toString());

        Rdn rdn2 = new Rdn(rdn1);
        System.out.println("rdn2:" + rdn2.toString());

        Attributes attrs = new BasicAttributes();
        attrs.put("ou", "Juicy, Fruit");
        attrs.put("cn", "Mango");
        Rdn rdn3 = new Rdn(attrs);
        System.out.println("rdn3:" + rdn3.toString());

        Rdn rdn4 = new Rdn("ou", "Juicy, Fruit");
        System.out.println("rdn4:" + rdn4.toString());

```

## Accessing type/value pairs of an RDN

The type/values of and RDN can be obtained using the methods below:


[<tt>getType()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#getType--)<br />
[<tt>getValue()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#getValue--)<br />
[<tt>toAttributes()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#toAttributes--)

For a an RDN made up of single type/value pair, the 
[<tt>getType()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#getType--)method returns the type and the 
[<tt>getValue()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#getValue--) method returns the value of the RDN. The method 
[<tt>toAttributes()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#toAttributes--) returns the attribute view of the type/value pairs. The 
[`example`](examples/RdnGetters.java) below prints the type/value pairs of RDNs.

```

        Attributes attrs = new BasicAttributes();
        attrs.put("o", "Yellow");
        attrs.put("cn", "Mango");

        // create a binary value for the RDN
        byte[] mangoJuice = new byte[6];
        for (int i = 0; i &lt; mangoJuice.length; i++) {
            mangoJuice[i] = (byte) i;
        }
        attrs.put("ou", mangoJuice);
        Rdn rdn = new Rdn(attrs);
        
        System.out.println();
        System.out.println("size:" + rdn.size());
        System.out.println("getType(): " + rdn.getType());
        System.out.println("getValue(): " + rdn.getValue());
        
        // test toAttributes
        System.out.println();
        System.out.println("toAttributes(): " + rdn.toAttributes());

```

## Getting the String Representation

In order to get the string representation of an RDN formatted according to the syntax specified in 
[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt), you can use:


[<tt>toString()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#toString--)

When you use the Rdn constructor that takes a <tt>String</tt> argument, you supply the string representation of an RDN, and get back an Rdn instance. To do the reverse, that is, to get the string representation of a Rdn instance, you use toString(). The result of toString() can be fed back to the Rdn constructor to produce an Rdn instance that is equal to the original Rdn instance.

Here's an 
[`example`](examples/RdntoString.java):

```

Rdn rdn = new Rdn("cn=Juicy\\,Fruit");
String str = rdn.toString();
System.out.println(str);
Rdn rdn2 = new Rdn(str);
System.out.println(rdn.equals(rdn2));    // true

```

## Comparing RDNs

The methods below enable comparison of RDNs:


[<tt>equals(Object Rdn)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#equals-Object-)<br />
[<tt>compareTo(Object Rdn)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#compareTo-Object-)

You can use compareTo() to sort a list of Rdn instances. equals() lets you determine whether two Rdns are syntactically equal. Two Rdns are equal if they both have the same (case-exact matched) type/value pairs. The order of components in a multi-valued RDNs is not significant.

Here's an 
[`example`](examples/CompareRdns.java):

```

        Rdn one = new Rdn("ou=Sales+cn=Bob");
        Rdn two = new Rdn("cn=Bob+ou=Sales");
        Rdn three = new Rdn("ou=Sales+cn=Bob+c=US");
        Rdn four = new Rdn("cn=lowercase");
        Rdn five = new Rdn("cn=LowerCASE");
        System.out.println(one.equals(two));    // true
        System.out.println(two.equals(three));  // false
        System.out.println(one.equals(three));  // false
        System.out.println(four.equals(five));  // true

```

## Escaping and Unescaping of Special Characters

One of the best use of Rdn class is when one has to deal with DNs consisting of special characters. It automatically takes care of escaping and unescaping the special characters. The characters such has '\' (backslash), ','(comma), + (plus) etc have a specific semantic according to 
[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt). You can find the list of all the special characters in RFC2253. When these characters are used as literals in a DN, they must be escaped with a '\' (blackslash).

For example, consider an RDN: `cn=Juicy, Fruit` The character , (comma) appearing between Juicy and Fruit is a special character that needs to be escaped by a '\' (blackslash). The resulting syntactically formatted RDN looks as below: `cn=Juicy\, Fruit` However, the '\' (backslash) character itself is a special character according to the Java Language String syntax, and it needs to escaped with a '\' (backslash) again. The Java Language String format and 
[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt) both use '\' (backslash) to escape the special characters. And therefore the Java formatted RDN String looks as below: `cn=Juicy\\, Fruit` Note that, the above mentioned formatting rules apply only to the value component of an Rdn. The <tt>Rdn</tt> class provides two <tt>static</tt> methods to handle automatic escaping and unescaping of the RDN values:


[<tt>escapeValue()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#escapeValue--)<br />
[<tt>unescapeValue()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html#unescapeValue--)

The 
[`example`](examples/EscapingDNs.java) below shows how to get the string representation of a DN without having to deal with the syntax for handling special characters defined in 
[RFC 2253](http://www.ietf.org/rfc/rfc2253.txt).

```

    // DN with ',' (comma)
    String unformatted = "Juicy, Fruit";
    String formatted = Rdn.escapeValue(unformatted);
    LdapName dn = new LdapName("cn=" + formatted);
    System.out.println("dn:" + dn);


    unformatted = "true+false";
    formatted = Rdn.escapeValue(unformatted); 
    dn = new LdapName("cn=" + formatted);
    System.out.println("dn:" + dn);

    // DN with a binary value as one one of its attribute value
    byte[] bytes = new byte[] {1, 2, 3, 4};
    formatted = Rdn.escapeValue(bytes);
    System.out.println("Orig val: " + bytes + "Escaped val: " + formatted);

```

Similarly the using the static <tt>unescapeValue()</tt> method one can obtain the original string from the formatted value. 
[`Here`](examples/UnescapingValues.java)
 is an example for retrieving the original value.

```

    // DN with ',' (comma)
    String unformatted = "Juicy, Fruit";
    String formatted = Rdn.escapeValue(unformatted);
    System.out.println("Formatted:" + formatted);
    Object original = Rdn.unescapeValue(formatted);
    System.out.println("Original:" +  original);  

    // DN with a '+' (plus)
    unformatted = "true+false";
    formatted = Rdn.escapeValue(unformatted); 
    System.out.println("Formatted:" + formatted);
    original = Rdn.unescapeValue(formatted);
    System.out.println("Original:" +  original);  

    // DN with a binary value as one one of its attribute value
    byte[] bytes = new byte[] {1, 2, 3, 4};
    formatted = Rdn.escapeValue(bytes);
    System.out.println("Formatted:" + formatted);
    original = Rdn.unescapeValue(formatted);
    System.out.println("Original:" +  original);  

```
