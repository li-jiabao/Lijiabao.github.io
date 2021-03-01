
# Storing and Reading Objects

Applications and services can use the directory in different ways to store and locate objects:

- Store (a copy of) the object itself.
- Store a reference to an object.
- Store attributes that describe the object.

In general terms, a Java object's serialized form contains the object's state and an object's reference in a compact representation of addressing information that can be used to contact the object. Some examples are given in the 

[Lookup an Object](../ops/lookup.html) lesson. An object's attributes are properties that are used to describe the object; attributes might include addressing and/or state information.

Which of these three ways to use depends on the application/system that is being built and how it needs to interoperate with other applications and systems that will share the objects stored in the directory. Another factor is the support provided by the service provider and the underlying directory service.

Programmatically, all applications use one of the following methods when storing objects in the directory:

<li>
[<tt>Context.bind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#bind-javax.naming.Name-java.lang.Object-)</li>
<li>
[<tt>DirContext.bind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)</li>
<li>
[<tt>Context.rebind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rebind-javax.naming.Name-java.lang.Object-)</li>
<li>
[<tt>DirContext.rebind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#rebind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-)</li>

The application passes the object that it wants to store to one of these methods. Then, depending on the types of objects that the service provider supports, the object will be transformed into a representation acceptable to the underlying directory service.

This lesson shows how to store serializable objects in the directory once the object is stored, you can simply use 
[<tt>lookup()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#lookup-javax.naming.Name-)to get a copy of the object back from the directory, regardless of what type of information was actually stored.

You can get the object back not only by using <tt>lookup()</tt>, but also when you 
[<tt>list</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#list-javax.naming.Name-) a context and when you 
[<tt>search</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-) a context or its subtree. In all of these cases, **object factories** might be involved. Object factories are discussed in detail in the 
[JNDI Tutorial](https://docs.oracle.com/javase/jndi/tutorial/objects/factory/index.html).

For storing below objects types, please refer to the JNDI Tutorial:

<li>
[Referenceable objects and JNDI <tt>Reference</tt>s](https://docs.oracle.com/javase/jndi/tutorial/objects/storing/reference.html) <br />
The bind() examples in the  
[Add, Replace or Remove a Binding](../ops/bind.html) lesson use Referenceable objects.
</li>
<li>
[Objects with attributes (<tt>DirContext</tt>)](https://docs.oracle.com/javase/jndi/tutorial/objects/storing/dircontext.html)</li>
<li>
[RMI (Java Remote Method Invocation) objects (including those that use IIOP)](https://docs.oracle.com/javase/jndi/tutorial/objects/storing/remote.html)</li>
<li>
[CORBA objects](https://docs.oracle.com/javase/jndi/tutorial/objects/storing/corba.html)</li>

<a name="REQ" id="REQ"></a>

**Before you go on:** To run these examples successfully, you must either turn off schema-checking in the server or add 
[`the Java schema`](../software/config/java.schema) that accompany this tutorial to the server. This task is typically performed by the directory server's administrator. See the 
[Software Setup](../software/content.html#SCHEMA) lesson for more information.

**Windows Active Directory:** 
[<tt>Context.rebind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rebind-javax.naming.Name-java.lang.Object-) and 
[<tt>DirContext.rebind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#rebind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-) do not work against Active Directory because these methods work by reading the attributes of the entry to be updated, removing the entry, and then adding a new entry that contains the modified attributes. Active Directory returns some attributes that cannot be set by the user, causing the final addition step to fail. The workaround for this problem is to use 
[<tt>DirContext.getAttributes()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#getAttributes-javax.naming.Name-) to obtain and save the attributes that you want to keep. Then, remove the entry and add it back with the saved attributes (and any others that you want to add) using 
[<tt>DirContext.bind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-).
