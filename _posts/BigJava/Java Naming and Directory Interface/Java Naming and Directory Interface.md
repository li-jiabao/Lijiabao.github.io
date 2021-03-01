
# Lesson: Naming and Directory Concepts

## Naming Concepts

A fundamental facility in any computing system is the **naming service**--the means by which names are associated with objects and objects are found based on their names. When using almost any computer program or system, you are always naming one object or another. For example, when you use an electronic mail system, you must provide the name of the recipient. To access a file in the computer, you must supply its name. A naming service allows you to look up an object given its name.

A naming service's primary function is to map people friendly names to objects, such as addresses, identifiers, or objects typically used by computer programs.

For example, the 
[Internet Domain Name System (DNS)](http://www.ietf.org/rfc/rfc1034.txt) maps machine names to IP Addresses:

```

www.example.com ==&gt; 192.0.2.5

```

A file system maps a filename to a file reference that a program can use to access the contents of the file.

```

c:\bin\autoexec.bat ==&gt; File Reference

```

These two examples also illustrate the wide range of scale at which naming services exist--from naming an object on the Internet to naming a file on the local file system.

## Names

To look up an object in a naming system, you supply it the **name** of the object. The naming system determines the syntax that the name must follow. This syntax is sometimes called the naming systems **naming convention**. A name is made up components. A name's representation consist of a component separator marking the components of the name.
<th id="h1">Naming System</th><th id="h2">Component Separator</th><th id="h3">Names</th>
<td headers="h1">UNIX file system</td><td headers="h2">"/"</td><td headers="h3">/usr/hello</td>
<td headers="h1">DNS</td><td headers="h2">"."</td><td headers="h3">sales.Wiz.COM</td>
<td headers="h1">LDAP</td><td headers="h2">"," and "="</td><td headers="h3">cn=Rosanna Lee, o=Sun, c=US</td>

The UNIX file system's naming convention is that a file is named from its path relative to the root of the file system, with each component in the path separated from left to right using the forward slash character ("/"). The UNIX **pathname**, <tt>/usr/hello</tt>, for example, names a file <tt>hello</tt> in the file directory <tt>usr</tt>, which is located in the root of the file system.

DNS naming convention calls for components in the DNS name to be ordered from right to left and delimited by the dot character ("."). Thus the DNS name <tt>sales.Wiz.COM</tt> names a DNS entry with the name <tt>sales</tt>, relative to the DNS entry <tt>Wiz.COM</tt>. The DNS entry <tt>Wiz.COM</tt>, in turn, names an entry with the name <tt>Wiz</tt> in the <tt>COM</tt> entry.

The 
[Lightweight Directory Access Protocol (LDAP)](http://www.ietf.org/rfc/rfc2251.txt) naming convention orders components from right to left, delimited by the comma character (","). Thus the LDAP name <tt>cn=Rosanna Lee, o=Sun, c=US</tt> names an LDAP entry <tt>cn=Rosanna Lee</tt>, relative to the entry <tt>o=Sun</tt>, which in turn, is relative to <tt>c=us</tt>. LDAP has the further rule that each component of the name must be a name/value pair with the name and value separated by an equals character ("=").

## Bindings

The association of a name with an object is called a **binding**. A file name is **bound** to a file.

The DNS contains bindings that map machine names to IP addresses. An LDAP name is bound to an LDAP entry.

## References and Addresses

<a name="REF" id="REF"></a>

Depending on the naming service, some objects cannot be stored directly by the naming service; that is, a copy of the object cannot be placed inside the naming service. Instead, they must be stored by reference; that is, a **pointer** or **reference** to the object is placed inside the naming service. A reference represents information about how to access an object. Typically, it is a compact representation that can be used to communicate with the object, while the object itself might contain more state information. Using the reference, you can contact the object and obtain more information about the object.

For example, an airplane object might contain a list of the airplane's passengers and crew, its flight plan, and fuel and instrument status, and its flight number and departure time. By contrast, an airplane object reference might contain only its flight number and departure time. The reference is a much more compact representation of information about the airplane object and can be used to obtain additional information. A file object, for example, is accessed using a **file reference**. A printer object, for example, might contain the state of the printer, such as its current queue and the amount of paper in the paper tray. A printer object reference, on the other hand, might contain only information on how to reach the printer, such as its print server name and printing protocol.

Although in general a reference can contain any arbitrary information, it is useful to refer to its contents as **addresses** (or communication end points): specific information about how to access the object.

For simplicity, this tutorial uses "object" to refer to both objects and object references when a distinction between the two is not required.

## Context

A **context** is a set of name-to-object bindings. Every context has an associated naming convention. A context always provides a lookup (**resolution**) operation that returns the object, it typically also provides operations such as those for binding names, unbinding names, and listing bound names. A name in one context object can be bound to another context object (called a **subcontext**) that has the same naming convention.

A file directory, such as <tt>/usr</tt>, in the UNIX file system represents a context. A file directory named relative to another file directory represents a subcontext (UNIX users refer to this as a **subdirectory**). That is, in a file directory <tt>/usr/bin</tt>, the directory <tt>bin</tt> is a subcontext of <tt>usr</tt>. A DNS domain, such as <tt>COM</tt>, represents a context. A DNS domain named relative to another DNS domain represents a subcontext. For the DNS domain <tt>Sun.COM</tt>, the DNS domain <tt>Sun</tt> is a subcontext of <tt>COM</tt>.

Finally, an LDAP entry, such as <tt>c=us</tt>, represents a context. An LDAP entry named relative to another LDAP entry represents a subcontext. For the LDAP entry <tt>o=sun,c=us</tt>, the entry <tt>o=sun</tt> is a subcontext of <tt>c=us</tt>.

## Naming Systems and Namespaces

A **naming system** is a connected set of contexts of the same type (they have the same naming convention) and provides a common set of operations.

A system that implements the DNS is a naming system. A system that communicates using the LDAP is a naming system.

A naming system provides a **naming service** to its customers for performing naming-related operations. A naming service is accessed through its own interface. The DNS offers a naming service that maps machine names to IP addresses. LDAP offers a naming service that maps LDAP names to LDAP entries. A file system offers a naming service that maps filenames to files and directories.

A **namespace** is the set of all possible names in a naming system. The UNIX file system has a namespace consisting of all of the names of files and directories in that file system. The DNS namespace contains names of DNS domains and entries. The LDAP namespace contains names of LDAP entries.
