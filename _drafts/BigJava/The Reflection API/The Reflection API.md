
# Lesson: Classes

Every type is either a reference or a primitive. Classes, enums, and arrays (which all inherit from
[`java.lang.Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)) as well as interfaces are all reference types. Examples of reference types include 
[`java.lang.String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html), all of the wrapper classes for primitive types such as 
[`java.lang.Double`](https://docs.oracle.com/javase/8/docs/api/java/lang/Double.html), the interface 
[`java.io.Serializable`](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), and the enum 
[`javax.swing.SortOrder`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SortOrder.html). There is a fixed set of primitive types: `boolean`, `byte`, `short`, `int`, `long`, `char`, `float`, and `double`.

For every type of object, the Java virtual machine instantiates an immutable instance of 
[`java.lang.Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) which provides methods to examine the runtime properties of the object including its members and type information. 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) also provides the ability to create new classes and objects. Most importantly, it is the entry point for all of the Reflection APIs. This lesson covers the most commonly used reflection operations involving classes:

<li>[Retrieving Class Objects](classNew.html) describes the ways to get a 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)</li>
- [Examining Class Modifiers and Types](classModifiers.html) shows how to access the class declaration information
- [Discovering Class Members](classMembers.html) illustrates how to list the constructors, fields, methods, and nested classes in a class
<!--
- [Navigating the Class Hierarchy](classNavigation.html) shows how to obtain the ancestors of a class-->
<li>[Troubleshooting](classTrouble.html) describes common errors encountered when using 
[`Class`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)</li>
