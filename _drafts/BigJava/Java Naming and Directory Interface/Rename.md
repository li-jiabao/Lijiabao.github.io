
# Create and Destroy Subcontexts

The <tt>Context</tt> interface contains methods for [creating](#CREATE) and [destroying](#DESTROY) a **subcontext**, a context that is bound in another context of the same type.

The example described here use an object that has **attributes** and create a subcontext in the directory. You can use these <tt>DirContext</tt> methods to associate attributes with the object at the time that the binding or subcontext is added to the namespace. For example, you might create a <tt>Person</tt> object and bind it to the namespace and at the same time associate attributes about that <tt>Person</tt> object. The naming equivalent will have no attributes.

The createSubcontext() differs from bind() in that it creates a new Object i.e a new Context to be bound to the directory while as bind() binds the given Object in the directory.

## <a name="CREATE" id="CREATE">Creating a Context</a>

To create a naming context, you supply to 
[<tt>createSubcontext()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#createSubcontext-javax.naming.Name-) the name of the context that you want to create. To create a context that has attributes, you supply to 
[<tt>DirContext.createSubcontext()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#createSubcontext-javax.naming.Name-javax.naming.directory.Attributes-) the name of the context that you want to create and its attributes.

**Before you go on:** The examples in this lesson require that you make additions to the schema. You must either turn off schema-checking in the LDAP server or add 
[`the schema`](../software/config/java.schema) that accompanies this tutorial to the server. Both of these tasks are typically performed by the directory server's administrator. See the 
[LDAP Setup](../software/content.html) lesson.

```

// Create attributes to be associated with the new context
Attributes attrs = new BasicAttributes(true); // case-ignore
Attribute objclass = new BasicAttribute("objectclass");
objclass.add("top");
objclass.add("organizationalUnit");
attrs.put(objclass);

// Create the context
Context result = ctx.createSubcontext("NewOu", attrs);

```


[`This example`](examples/Create.java) creates a new context called <tt>"ou=NewOu"</tt> that has an attribute <tt>"objectclass"</tt> with two values, <tt>"top"</tt> and <tt>"organizationalUnit"</tt>, in the context <tt>ctx</tt>.

```

# java Create
ou=Groups: javax.naming.directory.DirContext
ou=People: javax.naming.directory.DirContext
ou=NewOu: javax.naming.directory.DirContext

```



[`This example`](examples/Create.java)
 creates a new context, called <tt>"NewOu"</tt>, that is a child of <tt>ctx</tt>.

## <a name="DESTROY" id="DESTROY">Destroying a Context</a>

To destroy a context, you supply to 
[<tt>destroySubcontext()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#destroySubcontext-javax.naming.Name-) the name of the context to destroy.

```

// Destroy the context
ctx.destroySubcontext("NewOu");

```


[`This example`](examples/Destroy.java) destroys the context <tt>"NewOu"</tt> in the context <tt>ctx</tt>.
