
# Add, Replace or Remove a Binding

The <tt>Context</tt> interface contains methods for [adding](#BIND), [replacing](#REBIND), and [removing](#UNBIND) a binding in a context.

## <a name="BIND" id="BIND">Adding a Binding</a>


[<tt>Context.bind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#bind-javax.naming.Name-java.lang.Object-) is used to add a binding to a context. It accepts as arguments the name of the object and the object to be bound.

**Before you go on:** The examples in this lesson require that you make additions to the schema. You must either turn off schema-checking in the LDAP server or add 
[`the schema`](../software/config/java.schema) that accompanies this tutorial to the server. Both of these tasks are typically performed by the directory server's administrator. See the 
[LDAP Setup](../software/content.html)lesson.

```

// Create the object to be bound
Fruit fruit = new Fruit("orange");

// Perform the bind
ctx.bind("cn=Favorite Fruit", fruit);

```


[`This example`](examples/Bind.java) creates an object of class 
[`<tt>Fruit</tt>`](examples/Fruit.java) and binds it to the name <tt>"cn=Favorite Fruit"</tt> in the context <tt>ctx</tt>. If you subsequently looked up the name <tt>"cn=Favorite Fruit"</tt> in <tt>ctx</tt>, then you would get the <tt>fruit</tt> object. Note that to compile the <tt>Fruit</tt> class, you need the 
[`<tt>FruitFactory</tt>`](examples/FruitFactory.java)   class.

If you were to run this example twice, then the second attempt would fail with a 
[<tt>NameAlreadyBoundException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameAlreadyBoundException.html). This is because the name <tt>"cn=Favorite Fruit"</tt> is already bound. For the second attempt to succeed, you would have to use 
[<tt>rebind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#rebind-javax.naming.Name-java.lang.Object-).

## <a name="REBIND" id="REBIND">Adding or Replacing a Binding</a>

<tt>rebind()</tt> is used to add or replace a binding. It accepts the same arguments as <tt>bind()</tt>, but the semantics are such that if the name is already bound, then it will be unbound and the newly given object will be bound.

```

// Create the object to be bound
Fruit fruit = new Fruit("lemon");

// Perform the bind
ctx.rebind("cn=Favorite Fruit", fruit);

```

When you run 
[`this example`](examples/Rebind.java), it will replace the binding created by the 
[`<tt>bind()</tt>`](examples/Bind.java) example.

## <a name="UNBIND" id="UNBIND">Removing a Binding</a>

To remove a binding, you use 
[<tt>unbind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#unbind-javax.naming.Name-).

```

// Remove the binding
ctx.unbind("cn=Favorite Fruit");

```


[`This example`](examples/Unbind.java), when run, removes the binding that was created by the 
[`<tt>bind()</tt>`](examples/Bind.java) or 
[`<tt>rebind()</tt>`](examples/Rebind.java) example.
