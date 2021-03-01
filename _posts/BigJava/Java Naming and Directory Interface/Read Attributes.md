
# Modify Attributes

The 
[<tt>DirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) interface contains methods for modifying the attributes and attribute values of objects in the directory.

## <a name="LIST" id="LIST">Using a List of Modifications</a>

<a name="LIST__1" id="LIST__1">One way to modify the attributes of an object is to supply a list of modification requests (
</a>[<tt>ModificationItem</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/ModificationItem.html)). Each <tt>ModificationItem</tt> consists of a numeric constant indicating the type of modification to make and an 
[<tt>Attribute</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/Attribute.html) describing the modification to make. Following are the three types of modifications:

<li>
[ADD_ATTRIBUTE](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#ADD_ATTRIBUTE)</li>
<li>
[REPLACE_ATTRIBUTE](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#REPLACE_ATTRIBUTE)</li>
<li>
[REMOVE_ATTRIBUTE](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#REMOVE_ATTRIBUTE)</li>

Modifications are applied in the order in which they appear in the list. Either all of the modifications are executed, or none are.

The following code creates a list of modifications. It replaces the <tt>"mail"</tt> attribute's value with a value of <tt>"geisel@wizards.com"</tt>, adds an additional value to the <tt>"telephonenumber"</tt> attribute, and removes the <tt>"jpegphoto"</tt> attribute.

```

// Specify the changes to make
ModificationItem[] mods = new ModificationItem[3];

// Replace the "mail" attribute with a new value
mods[0] = new ModificationItem(DirContext.REPLACE_ATTRIBUTE,
    new BasicAttribute("mail", "geisel@wizards.com"));

// Add an additional value to "telephonenumber"
mods[1] = new ModificationItem(DirContext.ADD_ATTRIBUTE,
    new BasicAttribute("telephonenumber", "+1 555 555 5555"));

// Remove the "jpegphoto" attribute
mods[2] = new ModificationItem(DirContext.REMOVE_ATTRIBUTE,
    new BasicAttribute("jpegphoto"));

```

**Windows Active Directory:** Active Directory defines "telephonenumber" to be a single-valued attribute, contrary to 
[RFC 2256](http://www.ietf.org/rfc/rfc2256.txt). To get this example to work against Active Directory, you must either use an attribute other than "telephonenumber", or change the <tt>DirContext.ADD_ATTRIBUTE</tt> to <tt>DirContext.REPLACE_ATTRIBUTE</tt>.

After creating this list of modifications, you can supply it to 
[<tt>modifyAttributes()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#modifyAttributes-javax.naming.Name-javax.naming.directory.ModificationItem:A-) as follows.

```

// Perform the requested modifications on the named object
ctx.modifyAttributes(name, mods);

```

## <a name="ATTRS" id="ATTRS">Using Attributes</a>

Alternatively, you can perform modifications by specifying the type of modification and the attributes to which to apply the modification.

For example, the following line replaces the attributes (identified in <tt>orig</tt>) associated with <tt>name</tt> with those in <tt>orig</tt>:

```

ctx.modifyAttributes(name, DirContext.REPLACE_ATTRIBUTE, orig);

```

Any other attributes of <tt>name</tt> remain unchanged.

Both of these uses of <tt>modifyAttributes()</tt> are demonstrated in 
[`the sample program `](examples/ModAttrs.java ). This program modifies the attributes by using a modification list and then uses the second form of <tt>modifyAttributes()</tt> to restore the original attributes.
