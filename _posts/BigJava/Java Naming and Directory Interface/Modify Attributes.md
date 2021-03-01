
# Add, Replace Bindings with Attributes

The naming examples discussed how you can use 
[<tt>bind()</tt>, <tt>rebind()</tt>](bind.html). The 
[<tt>DirContext</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html) interface contains overloaded versions of these methods that accept attributes. You can use these <tt>DirContext</tt> methods to associate attributes with the object at the time that the binding or subcontext is added to the namespace. For example, you might create a <tt>Person</tt> object and bind it to the namespace and at the same time associate attributes about that <tt>Person</tt> object.

## <a name="BIND" id="BIND">Adding a Binding That Has Attributes</a>


[<tt>DirContext.bind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#bind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-) is used to add a binding that has attributes to a context. It accepts as arguments the name of the object, the object to be bound, and a set of attributes.

```

// Create the object to be bound
Fruit fruit = new Fruit("orange");

// Create attributes to be associated with the object
Attributes attrs = new BasicAttributes(true); // case-ignore
Attribute objclass = new BasicAttribute("objectclass");
objclass.add("top");
objclass.add("organizationalUnit");
attrs.put(objclass);

// Perform bind
ctx.bind("ou=favorite, ou=Fruits", fruit, attrs);

```


[`This example`](examples/Bind.java) creates an object of class 

[`<tt>Fruit</tt>`](examples/Fruit.java)
 and binds it to the name <tt>"ou=favorite"</tt> into the context named <tt>"ou=Fruits"</tt>, relative to <tt>ctx</tt>. This binding has the <tt>"objectclass"</tt> attribute. If you subsequently looked up the name <tt>"ou=favorite, ou=Fruits"</tt> in <tt>ctx</tt>, then you would get the <tt>fruit</tt> object. If you then got the attributes of <tt>"ou=favorite, ou=Fruits"</tt>, you would get those attributes with which the object was created. Following is this example's output.

```

# java Bind
orange
attribute: objectclass
value: top
value: organizationalUnit
value: javaObject
value: javaNamingReference
attribute: javaclassname
value: Fruit
attribute: javafactory
value: FruitFactory
attribute: javareferenceaddress
value: #0#fruit#orange
attribute: ou
value: favorite

```

The extra attributes and attribute values shown are used to store information about the object (<tt>fruit</tt>). These extra attributes are discussed in more detail in the 
 trail.

If you were to run this example twice, then the second attempt would fail with a 
[<tt>NameAlreadyBoundException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameAlreadyBoundException.html). This is because the name <tt>"ou=favorite"</tt> is already bound in the <tt>"ou=Fruits"</tt> context. For the second attempt to succeed, you would have to use <tt>rebind()</tt>.

## <a name="REBIND" id="REBIND">Replacing a Binding That Has Attributes</a>


[<tt>DirContext.rebind()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#rebind-javax.naming.Name-java.lang.Object-javax.naming.directory.Attributes-) is used to add or replace a binding and its attributes. It accepts the same arguments as <tt>bind()</tt>. However, <tt>rebind()</tt>'s semantics require that if the name is already bound, then it will be unbound and the newly given object and attributes will be bound.

```

// Create the object to be bound
Fruit fruit = new Fruit("lemon");

// Create attributes to be associated with the object
Attributes attrs = new BasicAttributes(true); // case-ignore
Attribute objclass = new BasicAttribute("objectclass");
objclass.add("top");
objclass.add("organizationalUnit");
attrs.put(objclass);

// Perform bind
ctx.rebind("ou=favorite, ou=Fruits", fruit, attrs);

```

When you run 
[`this example`](examples/Rebind.java)
, it replaces the binding that the 
[`<tt>bind()</tt>`](examples/Bind.java)
 example created.

```

# java Rebind
lemon
attribute: objectclass
value: top
value: organizationalUnit
value: javaObject
value: javaNamingReference
attribute: javaclassname
value: Fruit
attribute: javafactory
value: FruitFactory
attribute: javareferenceaddress
value: #0#fruit#lemon
attribute: ou
value: favorite

```
