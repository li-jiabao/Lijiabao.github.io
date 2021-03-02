
# Directory and LDAP Packages

## Directory Package

The 
[<tt>javax.naming.directory</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/package-summary.html) package extends the 
[<tt>javax.naming</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-summary.html) package to provide functionality for accessing directory services in addition to naming services. This package allows applications to retrieve associated with objects stored in the directory and to search for objects using specified attributes.

### The Directory Context

The 
[<tt>DirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) interface represents a **directory context**. <tt>DirContext</tt> also behaves as a naming context by extending the 
[<tt>Context</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html) interface.  This means that
any directory object can also provide a naming context. It defines methods
for examining and updating attributes associated with a directory entry.

## LDAP Package

The 
[<tt>javax.naming.ldap</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/package-summary.html) package contains classes and interfaces for using features that are specific to the 
[LDAP v3](http://www.ietf.org/rfc/rfc2251.txt) that are not already covered by the more generic 
[<tt>javax.naming.directory</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/package-summary.html) package. In fact, most JNDI applications that use the LDAP will find the `javax.naming.directory` package sufficient and will not need to use the <tt>javax.naming.ldap</tt> package at all. This package is primarily for those applications that need to use "extended" operations, controls, or unsolicited notifications.

### The LDAP Context

The 
[<tt>LdapContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/LdapContext.html) interface represents a **context** for performing "extended" operations, sending request controls, and receiving response controls. Examples of how to use these features are described in the JNDI Tutorial's 
[Controls and Extensions](https://docs.oracle.com/javase/jndi/tutorial/ldap/ext/index.html) lesson.<br />
