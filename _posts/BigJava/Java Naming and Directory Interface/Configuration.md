
# Frequently Asked Questions

This lesson answers the frequently asked questions users often have when using the JNDI to access LDAP services. Some of the common problems are answered in the 
[Trouble Shooting Tips](../ops/faq.html) of the Naming and Directory Operations lesson.

<li style="list-style: none; display: inline">
<h2>Contexts:</h2>
</li>
1. [Is the context safe for multithreaded access?](#1)
1. [Why does the LDAP provider ignore my security environment properties?](#2)
1. [Why do I keep getting a CommunicationException?](#3)
1. [How can I get a trace of LDAP messages?](#4)
1. [How do I use a different authentication mechanism such as Kerberos?](#5)
<li>[Should I enable ssl when changing the password?](#6)
<h2>Attributes:</h2>
</li>
1. [When I ask for one attribute, I get back another. Why?](#7)
1. [How do I know the type of an attribute's value?](#8)
1. [How do I get back an attribute's value in a form other than a String or byte array?](#9)
<li>[Why does putting an "*" as an attribute value not work as expected in my search?](#10)
<h2>Searches:</h2>
</li>
1. [Why don't wildcards in search filters always work?](#11)
1. [Why do I get back only **n** number of entries when I know there are more in the directory?](#12)
1. [How do I pass controls with my search?](#13)
<li>[How do I find out how many search results I got back?](#14)
<h2>Names:</h2>
</li>
1. [Why do I get an empty string as a name in my SearchResult?](#15)
1. [Why do I get a URL string as a name in my SearchResult?](#16)
1. [What type is the Name argument passed to the context methods?](#17)
1. [Can I pass the name I got back from NameParser to the Context methods?](#18)
1. [What's the relationship between the name I use for the <tt>Context.SECURITY_PRINCIPAL</tt> property and the directory?](#19)
1. [Why are there strange quotation marks and escapes in the names that I read from the directory?](#20)
1. [How do I get an LDAP entry's full DN?](#21)

## Attributes:

## Names:


 <a style="font-weight: bold" name="1" id="1">1. Is the context safe for multithreaded access, or do I need to lock/synchronize access to a context?</a>

The answer depends on the implementation. This is because the 
[<tt>Context</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html) and 
[<tt>DirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) interfaces do not specify synchronization requirements. The LDAP implementation in the JDK is optimized for single-threaded access. If you have multiple threads accessing the same <tt>Context</tt> instance, then each thread needs to lock the <tt>Context</tt> instance when using it. This also applies to any <tt>NamingEnumeration</tt> that is derived from the same <tt>Context</tt> instance. However, multiple threads can access **different** <tt>Context</tt> instances (even those derived from the same initial context) concurrently without locks.

<a style="font-weight: bold" name="2" id="2">2. Why does the LDAP provider ignore my security environment properties if I do not set the 
</a>[<tt>Context.SECURITY_CREDENTIALS</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_CREDENTIALS) (<tt>"java.naming.security.credentials"</tt>) property or set it to the empty string?

If you supply an empty string, an empty <tt>byte</tt>/<tt>char</tt> array, or <tt>null</tt> to the <tt>Context.SECURITY_CREDENTIALS</tt> environment property, then an anonymous bind will occur even if the <tt>Context.SECURITY_AUTHENTICATION</tt> property was set to <tt>"simple"</tt>. This is because for simple authentication, the LDAP requires the password to be nonempty. If a password is not supplied, then the protocol automatically converts the authentication to <tt>"none"</tt>.

<a style="font-weight: bold" name="3" id="3">3. Why do I keep getting a 
</a>[<tt>CommunicationException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/CommunicationException.html) when I try to create an initial context?

You might be talking to a server that supports only the LDAP v2. See the Miscellaneous lesson of the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/version.html) for an example of how to set the version number.

<a style="font-weight: bold" name="4" id="4">4. How can I trace the LDAP message?</a>

Try using the <tt>"com.sun.jndi.ldap.trace.ber"</tt> environment property. If the value of this property is an instance of <tt>java.io.OutputStream</tt>, then trace information about BER buffers sent and received by the LDAP provider is written to that stream. If the property's value is <tt>null</tt>, then no trace output is written.

For example, the following code will send the trace output to <tt>System.err</tt>.

```

env.put("com.sun.jndi.ldap.trace.ber", System.err);

```

<a style="font-weight: bold" name="5" id="5">5. How do I use a different authentication mechanism such as Kerberos?</a>

Follow the instructions in the GSS-API/Kerberos v5 Authentication of the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/gssapi.html) for information on how to use Kerberos authentication. To use other authentication mechanisms, see the Using Arbitrary SASL Mechanisms section of the 
[JNDI tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/security/mechanism.html).

<a style="font-weight: bold" name="6" id="6">6. Should I enable SSL when changing the password?</a> /

It really depends on the directory server you are using. Some directory servers won't allow you to change the password if SSL is not enabled but some do allow it. It's good to have SSL enabled to have your password secured in the communication channel.

<a style="font-weight: bold" name="7" id="7">7. When I ask for one attribute, I get back another. Why?</a>

The attribute name that you are using might be a synonym for another attribute. In this case, the LDAP server might return the canonical attribute name instead of the one that you supplied. When you look in the <tt>Attributes</tt> returned by the server, you need to use the canonical name instead of the synonym.

For example, <tt>"fax"</tt> might be a synonym for the canonical attribute name <tt>"facsimiletelephonenumber"</tt>. If you ask for <tt>"fax"</tt>, the server will return the attribute named <tt>"facsimiletelephonenumber"</tt>. See the 
[Naming and Directory Operations](../ops/attrnames.html) lesson for details on synonyms and other issues regarding attribute names.

<a style="font-weight: bold" name="8" id="8">8. How do I know the type of an attribute's value?</a>

An attribute's value can be either <tt>java.lang.String</tt> or <tt>byte[]</tt>. See the Miscellaneous section of the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/attrs.html) for information on which attributes' values are returned as <tt>byte[]</tt>. To do this programmatically, you can use the <tt>instanceof</tt> operator to examine the attribute value that you get back from the LDAP provider.

<a style="font-weight: bold" name="9" id="9">9. How do I get back an attribute's value in a form other than a String or byte array?</a>

Currently you can't. The LDAP provider returns only attribute values that are either <tt>java.lang.String</tt> or <tt>byte[]</tt>. See the Miscellaneous section of the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/misc/attrs.html).

<a style="font-weight: bold" name="10" id="10">10. Why does putting an "*" as an attribute value not work as expected in my search?</a>

When you use the following form of <tt>search()</tt>, the attribute values are treated as literals; that is, the attribute in the directory entry is expected to contain exactly that value: 
[<tt>search(Name name, Attributes matchingAttrs)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-) To use wildcards, you should use the string filter forms of <tt>search()</tt>, as follows. 
[<tt>search(Name name, String filter, SearchControls ctls)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-)<br />
[<tt>search(Name name, String filterExpr, Object[]filterArgs, SearchControls ctls)</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-java.lang.Object:A-javax.naming.directory.SearchControls-)

For the last form, the wildcard characters must appear in the <tt>filterExpr</tt> argument, and not in <tt>filterArgs</tt>. The values in <tt>filterArgs</tt> are also treated as literals.

<a style="font-weight: bold" name="11" id="11">11. Why don't wildcards in search filters always work?</a>

A wildcard that appears before or after the attribute value (such as in <tt>"attr=*I*"</tt>) indicates that the server is to search for matching attribute values by using the attribute's substring matching rule. If the attribute's definition does not have a substring matching rule, then the server cannot find the attribute. You'll have to search by using an equality or "present" filter instead.

<a style="font-weight: bold" name="12" id="12">12. Why do I get back only **n** number of entries when I know there are more in the directory?</a> Some servers are configured to limit the number of entries that can be returned. Others also limit the number of entries that can be examined during a search. Check your server configuration.

<a style="font-weight: bold" name="13" id="13">13. How do I pass controls with my search?</a>

Controls are not explained in this tutorial. Check out the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/context.html).

<a style="font-weight: bold" name="14" id="14">14. How do I find out how many search results I got back?</a>

You must keep count as you enumerate through the results. The LDAP does not provide this information.

<a style="font-weight: bold" name="15" id="15">15. Why do I get an empty string as a name in my 
</a>[<tt>SearchResult</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchResult.html)?


[<tt>getName()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getName--) always returns a name **relative** to the **target context** of the search. So, if the target context satisfies the search filter, then the name returned will be "" (the empty name) because that is the name relative to the target context. See the 
[Search Results](result.html) section for details.

<a style="font-weight: bold" name="16" id="16">16. Why do I get a URL string as a name in my 
</a>[<tt>SearchResult</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchResult.html)?

The LDAP entry was retrieved by following either an alias or a referral, so its name is a URL. See the 
[Search Results](result.html) lesson for details.

<a style="font-weight: bold" name="17" id="17">17. What type is the 
</a>[<tt>Name</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Name.html) argument passed to the 
[<tt>Context</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html) and 
[<tt>DirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) methods? - a 
[<tt>CompoundName</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/CompoundName.html) or a 
[<tt>CompositeName</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/CompositeName.html)?

The string forms accept the string representation of a composite name. That is, using a string name is equivalent to calling <tt>new CompositeName(stringName)</tt> and passing the results to the <tt>Context</tt>/<tt>DirContext</tt> method. The <tt>Name</tt> argument can be any object that implements the <tt>Name</tt> interface. If it is an instance of <tt>CompositeName</tt>, then the name is treated as a composite name; otherwise, it is treated as a compound name.

<a style="font-weight: bold" name="18" id="18">18. Can I pass the name I got back from 
</a>[<tt>NameParser</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameParser.html) to 
[<tt>Context</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html) methods?

This is related to the previous question. Yes, you can. 
[<tt>NameParser.parse()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameParser.html#parse-java.lang.String-) returns a compound name that implements the <tt>Name</tt> interface. This name can be passed to the <tt>Context</tt> methods, which will interpret it as a compound name.

<a style="font-weight: bold" name="19" id="19">19. What's the relationship between the name I use for the 
</a>[<tt>Context.SECURITY_PRINCIPAL</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#SECURITY_PRINCIPAL) property and the directory?

You can think of the principal name as coming from a different namespace than the directory. See 
[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt) and the 
[Security](../ldap/security.html) section for details on LDAP authentication mechanisms. The LDAP service provider in the JDK accepts a string principal name, which it passes directly to the LDAP server. Some LDAP servers accept DNs, whereas others support the schemes proposed by 
[RFC 2829](http://www.ietf.org/rfc/rfc2829.txt).

<a style="font-weight: bold" name="20" id="20">20. Why are there strange quotation marks and escapes in the names that I read from the directory?</a>

The LDAP name parser in the JDK is conservative with respect to quoting rules, but it nevertheless produces "correct" names. Also, remember that the names of entries returned by 
[<tt>NamingEnumeration</tt>s](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html) are composite names that can be passed back to the <tt>Context</tt> and <tt>DirContext</tt> methods. So, if the name contains a character that conflicts with the composite name syntax (such as the forward slash character "/"), then the LDAP provider will provide an encoding to ensure that the slash character will be treated as part of the LDAP name rather than as a composite name separator.

Start using the 
[<tt>LdapName</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapName.html) and 
[<tt>Rdn</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/Rdn.html) classes that enable easy name manipulation.

<a style="font-weight: bold" name="21" id="21">21. How do I get an LDAP entry's full DN?</a>

You can use 
[<tt>NameClassPair.getNameInNamespace()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameClassPair.html#getNameInNamespace--).
