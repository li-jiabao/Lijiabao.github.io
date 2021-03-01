
# Bean Persistence

A bean has the property of persistence when its properties, fields, and state information are saved to and retrieved from storage. Component models provide a mechanism for persistence that enables the state of components to be stored in a non-volatile place for later retrieval.

The mechanism that makes persistence possible is called **serialization**. Object serialization means converting an object into a data stream and writing it to storage. Any applet, application, or tool that uses that bean can then "reconstitute" it by deserialization. The object is then restored to its original state.

For example, a Java application can serialize a Frame window on a Microsoft Windows machine, the serialized file can be sent with e-mail to a Solaris machine, and then a Java application can restore the Frame window to the exact state which existed on the Microsoft Windows machine.

Any applet, application, or tool that uses that bean can then "reconstitute" it by *deserialization*.

All beans must persist. To persist, your beans must support serialization by implementing either the 
[`java.io.Serializable`](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html) (in the API reference documentation) interface, or the 
[`java.io.Externalizable`](https://docs.oracle.com/javase/8/docs/api/java/io/Externalizable.html) (in the API reference documentation) interface. These interfaces offer you the choices of automatic serialization and customized serialization. If any class in a class's inheritance hierarchy implements `Serializable` or `Externalizable`, then that class is serializable.

## Classes That Are Serializable

Any class is serializable as long as that class or a parent class implements the `java.io.Serializable` interface. Examples of serializable classes include `Component`, `String`, `Date`, `Vector`, and `Hashtable`. Thus, any subclass of the `Component` class, including `Applet`, can be serialized. Notable classes not supporting serialization include `Image`, `Thread`, `Socket`, and `InputStream`. Attempting to serialize objects of these types will result in an `NotSerializableException`.

The Java Object Serialization API automatically serializes most fields of a Serializable object to the storage stream. This includes primitive types, arrays,and strings. The API does not serialize or deserialize fields that are marked transient or static.

## Controlling Serialization

You can control the level of serialization that your beans undergo. Three ways to control serilization are:

- Automatic serialization, implemented by the `Serializable` interface. The Java serialization software serializes the entire object, except transient and static fields.
- Customized serialization. Selectively exclude fields you do not want serialized by marking with the `transient` (or `static`) modifier.
- Customized file format, implemented by the `Externalizable` interface and its two methods. Beans are written in a specific file format.

## Default Serialization: The Serializable Interface

The `Serializable` interface provides automatic serialization by using the Java Object Serialization tools. `Serializable` declares no methods; it acts as a marker, telling the Object Serialization tools that your bean class is serializable. Marking your class `Serializable` means you are telling the Java Virtual Machine (JVM) that you have made sure your class will work with default serialization. Here are some important points about working with the `Serializable` interface:

- Classes that implement `Serializable` must have an access to a **no-argument constructor** of supertype. This constructor will be called when an object is "reconstituted" from a `.ser` file.
- You don't need to implement `Serializable` in your class if it is already implemented in a superclass.
- All fields except static and transient fields are serialized. Use the `transient` modifier to specify fields you do not want serialized, and to specify classes that are not serializable.

## Selective Serialization Using the transient Keyword

To exclude fields from serialization in a `Serializable` object mark the fields with the `transient` modifier.

```

transient int status;

```

Default serialization will not serialize `transient` and `static` fields.

## Selective Serialization: writeObject and readObject

If your serializable class contains either of the following two methods (the signatures must be exact), then the default serialization will not take place.

```

private void writeObject(java.io.ObjectOutputStream out)
    throws IOException;
private void readObject(java.io.ObjectInputStream in)
    throws IOException, ClassNotFoundException;

```

You can control how more complex objects are serialized, by writing your own implementations of the `writeObject` and `readObject` methods. Implement `writeObject` when you need to exercise greater control over what gets serialized when you need to serialize objects that default serialization cannot handle, or when you need to add data to the serialization stream that is not an object data member. Implement `readObject` to reconstruct the data stream you wrote with `writeObject`.

## The Externalizable Interface

Use the `Externalizable` interface when you need complete control over your bean's serialization (for example, when writing and reading a specific file format). To use the `Externalizable` interface you need to implement two methods: `readExternal` and `writeExternal`. Classes that implement `Externalizable` must have a no-argument constructor.
