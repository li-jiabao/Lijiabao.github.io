
# Serializable Objects

To **serialize** an object means to convert its state to a byte stream so that the byte stream can be reverted back into a copy of the object. A Java object is **serializable** if its class or any of its superclasses implements either the <tt>java.io.Serializable</tt> interface or its subinterface, <tt>java.io.Externalizable</tt>. **Deserialization** is the process of converting the serialized form of an object back into a copy of the object.

For example, the <tt>java.awt.Button</tt> class implements the <tt>Serializable</tt> interface, so you can serialize a <tt>java.awt.Button</tt> object and store that serialized state in a file. Later, you can read back the serialized state and deserialize into a <tt>java.awt.Button</tt> object.

The Java platform specifies a default way by which serializable objects are serialized. A (Java) class can override this default serialization and define its own way of serializing objects of that class. The 
[Object Serialization Specification](https://docs.oracle.com/javase/8/docs/technotes/guides/serialization/index.html) describes object serialization in detail.

When an object is serialized, information that identifies its class is recorded in the serialized stream. However, the class's definition ("class file") itself is not recorded. It is the responsibility of the system that is deserializing the object to determine how to locate and load the necessary class files. For example, a Java application might include in its classpath a JAR file that contains the class files of the serialized object(s) or load the class definitions by using information stored in the directory, as explained later in this lesson. <a name="EXAMPLE" id="EXAMPLE"></a>

## Binding a Serializable Object

You can store a serializable object in the directory if the underlying service provider supports that action, as does Oracle's LDAP service provider.

The following example invokes 
[`Context.bind`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#bind-javax.naming.Name-java.lang.Object-) to bind an AWT button to the name <tt>"cn=Button"</tt>. To associate attributes with the new binding, you use 
[`DirContext.bind`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-). To overwrite an existing binding, use 
[`Context.rebind`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rebind-javax.naming.Name-java.lang.Object-) and 
[`DirContext.rebind`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#rebind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-).

```

// Create the object to be bound
Button b = new Button("Push me");

// Perform the bind
ctx.bind("cn=Button", b);

```

You can then read the object back using 
[`Context.lookup`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-), as follows.

```

// Check that it is bound
Button b2 = (Button)ctx.lookup("cn=Button");
System.out.println(b2);

```

Running 
[`this example`](examples/SerObj.java) produces the following output.

```

# java SerObj
java.awt.Button[button0,0,0,0x0,invalid,label=Push me]

```

<a name="CODEBASE" id="CODEBASE"></a>

## Specifying a Codebase

**Note:** The procedures described here are for binding a serializable object in a directory service that follows the schema defined in 
[RFC 2713](http://www.ietf.org/rfc/rfc2713.txt). These procedures might not be generally applicable to other naming and directory services that support binding a serializable object with a specified codebase.

When a serialized object is bound in the directory as shown in the previous example, applications that read the serialized object from the directory must have access to the class definitions necessary to deserialize the object.

Alternatively, you can record a **codebase** with the serialized object in the directory, either when you bind the object or subsequently by adding an attribute by using 
[`DirContext.modifyAttributes`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#modifyAttributes-javax.naming.Name-int-javax.naming.directory.Attributes-). You can use any attribute to record this codebase and have your application read that attribute from the directory and use it appropriately. Or you can use the <tt>"javaCodebase"</tt> attribute specified in . In the latter case, Oracle's LDAP service provider will automatically use the attribute to load the class definitions as needed. <tt>"javaCodebase"</tt> should contain the URL of a codebase directory or a JAR file. If the codebase contains more than one URL, then each URL must be separated by a space character.

The following example resembles the one for binding a <tt>java.awt.Button</tt>. It differs in that it uses a user-defined <tt>Serializable</tt> class, 
[`<tt>Flower</tt>`](examples/Flower.java), and supplies a <tt>"javaCodebase"</tt> attribute that contains the location of <tt>Flower</tt>'s class definition. Here's the code that does the binding.

```

String codebase = ...;

// Create the object to be bound
Flower f = new Flower("rose", "pink");

// Perform the bind and specify the codebase
ctx.bind("cn=Flower", f, new BasicAttributes("javaCodebase", codebase));

```

When you run 
[`this example`](examples/SerObjWithCodebase.java), you must supply the URL of the location at which the class file <tt>Flower.class</tt> was installed. For example, if <tt>Flower.class</tt> was installed at the Web server <tt>web1</tt>, in the directory <tt>example/classes</tt>, then you would run this example as follows.

```

# java SerObjWithCodebase http://web1/example/classes/
pink rose

```

Afterward, you may remove <tt>Flower.class</tt> from your classpath and run any program that looks up or lists this object without directly referencing the <tt>Flower</tt> class. If your program references <tt>Flower</tt> directly, then you must make its class file available for compilation and execution.
