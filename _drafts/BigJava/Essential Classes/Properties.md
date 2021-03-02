
# Properties

*Properties* are configuration values managed as *key/value pairs*. In each pair, the key and value are both 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) values. The key identifies, and is used to retrieve, the value, much as a variable name is used to retrieve the variable's value. For example, an application capable of downloading files might use a property named "download.lastDirectory" to keep track of the directory used for the last download.

To manage properties, create instances of 
[`java.util.Properties`](https://docs.oracle.com/javase/8/docs/api/java/util/Properties.html). This class provides methods for the following:

- loading key/value pairs into a `Properties` object from a stream,
- retrieving a value from its key,
- listing the keys and their values,
- enumerating over the keys, and
- saving the properties to a stream.

For an introduction to streams, refer to the section 
[I/O Streams](../../essential/io/streams.html) in the 
[Basic I/O](../../essential/io/index.html) lesson.

`Properties` extends 
[`java.util.Hashtable`](https://docs.oracle.com/javase/8/docs/api/java/util/Hashtable.html). Some of the methods inherited from `Hashtable` support the following actions:

- testing to see if a particular key or value is in the `Properties` object,
- getting the current number of key/value pairs,
- removing a key and its value,
- adding a key/value pair to the `Properties` list,
- enumerating over the values or the keys,
- retrieving a value by its key, and
- finding out if the `Properties` object is empty.

The `System` class maintains a `Properties` object that defines the configuration of the current working environment. For more about these properties, see [System Properties](sysprop.html). The remainder of this section explains how to use properties to manage application configuration.

## Properties in the Application Life Cycle

The following figure illustrates how a typical application might manage its configuration data with a `Properties` object over the course of its execution.

Next, the application creates another `Properties` object and loads the properties that were saved from the last time the application was run. Many applications store properties on a per-user basis, so the properties loaded in this step are usually in a specific file in a particular directory maintained by this application in the user&#39;s home directory. Finally, the application uses the default and remembered properties to initialize itself.

The key here is consistency. The application must always load and save properties to the same location so that it can find them the next time it&#39;s executed.

<li>`Starting Up`<br />
The actions given in the first three boxes occur when the application is starting up. First, the application loads the default properties from a well-known location into a `Properties` object. Normally, the default properties are stored in a file on disk along with the `.class` and other resource files for the application.<br />
Next, the application creates another `Properties` object and loads the properties that were saved from the last time the application was run. Many applications store properties on a per-user basis, so the properties loaded in this step are usually in a specific file in a particular directory maintained by this application in the user's home directory. Finally, the application uses the default and remembered properties to initialize itself.<br />
The key here is consistency. The application must always load and save properties to the same location so that it can find them the next time it's executed.<br />
</li>
<li>`Running`<br />
During the execution of the application, the user may change some settings, perhaps in a Preferences window, and the `Properties` object is updated to reflect these changes. If the users changes are to be remembered in future sessions, they must be saved.
</li>
<li>`Exiting`<br />
Upon exiting, the application saves the properties to its well-known location, to be loaded again when the application is next started up.
</li>

## Setting Up the Properties Object

The following Java code performs the first two steps described in the previous section: loading the default properties and loading the remembered properties:

```

. . .
// create and load default properties
Properties defaultProps = new Properties();
FileInputStream in = new FileInputStream("defaultProperties");
defaultProps.load(in);
in.close();

// create application properties with default
Properties applicationProps = new Properties(defaultProps);

// now load properties 
// from last invocation
in = new FileInputStream("appProperties");
applicationProps.load(in);
in.close();
. . .

```

First, the application sets up a default `Properties` object. This object contains the set of properties to use if values are not explicitly set elsewhere. Then the load method reads the default values from a file on disk named `defaultProperties`.

Next, the application uses a different constructor to create a second `Properties` object, `applicationProps`, whose default values are contained in `defaultProps`. The defaults come into play when a property is being retrieved. If the property can't be found in `applicationProps`, then its default list is searched.

Finally, the code loads a set of properties into `applicationProps` from a file named `appProperties`. The properties in this file are those that were saved from the application the last time it was invoked, as explained in the next section.

## Saving Properties

The following example writes out the application properties from the previous example using `Properties.store`. The default properties don't need to be saved each time because they never change.

```

FileOutputStream out = new FileOutputStream("appProperties");
applicationProps.store(out, "---No Comment---");
out.close();

```

The `store` method needs a stream to write to, as well as a string that it uses as a comment at the top of the output.

## Getting Property Information

Once the application has set up its `Properties` object, the application can query the object for information about various keys and values that it contains. An application gets information from a `Properties` object after start up so that it can initialize itself based on choices made by the user. The `Properties` class has several methods for getting property information:

<li>`contains(Object value)` and  `containsKey(Object key)`<br />
Returns `true` if the value or the key is in the `Properties` object. `Properties` inherits these methods from `Hashtable`. Thus they accept `Object` arguments, but only `String` values should be used.
</li>
<li>`getProperty(String key)` and `getProperty(String key, String default)`<br />
Returns the value for the specified property. The second version provides for a default value. If the key is not found, the default is returned.
</li>
<li>`list(PrintStream s)` and `list(PrintWriter w)`<br />
Writes all of the properties to the specified stream or writer. This is useful for debugging.
</li>
<li>`elements()`, `keys()`, and `propertyNames()`<br />
Returns an `Enumeration` containing the keys or values (as indicated by the method name) contained in the `Properties` object. The `keys` method only returns the keys for the object itself; the `propertyNames` method returns the keys for default properties as well.
</li>
<li>`stringPropertyNames()`<br />
Like `propertyNames`, but returns a `Set&lt;String&gt;`, and only returns names of properties where both key and value are strings. Note that the `Set` object is not backed by the `Properties` object, so changes in one do not affect the other.
</li>
<li>`size()`<br />
Returns the current number of key/value pairs.
</li>

## Setting Properties

A user's interaction with an application during its execution may impact property settings. These changes should be reflected in the `Properties` object so that they are saved when the application exits (and calls the `store` method). The following methods change the properties in a `Properties` object:

<li>`setProperty(String key, String value)`<br />
Puts the key/value pair in the `Properties` object.
</li>
<li>`remove(Object key)`<br />
Removes the key/value pair associated with key.
</li>
