
# Using and Creating a DataFlavor

The 
[`DataFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html) class allows you to specify the content type of your data. You need to specify a `DataFlavor` when fetching the data from the `importData` method. Several flavor types are predefined for you:

<li>
[`imageFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#imageFlavor) represents data in the `java.awt.Image` format. This is used when dragging image data.</li>
<li>
[`stringFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#stringFlavor) represents data in the most basic form of text &#8212; `java.lang.String`. This is the most commonly used data flavor for most applications.</li>
<li>
[`javaFileListFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#javaFileListFlavor) represents `java.io.File` objects in a `java.util.List` format. This is useful for applications that drag files, such as the `TopLevelTransferHandler` example, discussed in the 
[Top-Level Drop](toplevel.html) lesson.</li>

For most applications, this is all you need to know about data flavors. However, if you require a flavor other than these predefined types, you can create your own. If you create a custom component and want it to participate in data transfer, you will need to create a custom data flavor. The constructor for specifying a data flavor is 
[`DataFlavor(Class, String)`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#DataFlavor-java.lang.Class-java.lang.String-). For example, to create a data flavor for the `java.util.ArrayList` class:

```

new DataFlavor(ArrayList.class, "ArrayList");

```

To create a data flavor for an integer array:

```

new DataFlavor(int[].class, "Integer Array");

```

Transferring the data using this mechanism uses `Object` serialization, so the class you use to transfer the data must implement the `Serializable` interface, as must anything that is serialized with it. If everything is not serializable, you will see a `NotSerializableException` during drop or copy to the clipboard.

Creating a data flavor using the `DataFlavor(Class, String)` constructor allows you to transfer data between applications, including native applications. If you want to create a data flavor that transfers data only within an application, use 
[`<code>javaJVMLocalObjectMimeType`</code>](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#javaJVMLocalObjectMimeType) and the 
[`<code>DataFlavor(String)`</code>](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html#DataFlavor-java.lang.String-) constructor. For example, to specify a data flavor that transfers color from a `JColorChooser` only within your application, you could use this code:

```

String colorType = DataFlavor.javaJVMLocalObjectMimeType +
                   ";class=java.awt.Color";
DataFlavor colorFlavor = new DataFlavor(colorType);

```

To create a data flavor for an `ArrayList` that would work only within your application:

```

new DataFlavor(DataFlavor.javaJVMLocalObjectMimeType +
               ";class=java.util.ArrayList");

```

To create a data flavor for an integer array:

```

new DataFlavor(DataFlavor.javaJVMLocalObjectMimeType +
               ";class=\"" + int[].class.getName() + "\"");

```

A MIME type containing special characters, such as **[** or **;**, must have those characters enclosed in quotes.

A `Transferable` can be implemented to support multiple flavors. For example, you can use both local and serialization flavors together, or you can use two forms of the same data, such as the `ArrayList` and integer array flavors, together, or you can create a `TransferHandler` that accepts different types of data, such as color and text.

When you create an array of `DataFlavors` to be returned from the `Transferable`'s 
[`getTransferDataFlavors`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/Transferable.html#getTransferDataFlavors--) method, the flavors should be inserted in preferred order, with the most preferred appearing at element 0 of the array. Genereally the preferred order is from the richest, or most complex, form of the data down to the simpleset &#8212; the form most likely to be understood by other objects.
