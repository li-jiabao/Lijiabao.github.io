
# Fields

A **field** is a class, interface, or enum with an associated value. Methods in the 
[`java.lang.reflect.Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html) class can retrieve information about the field, such as its name, type, modifiers, and annotations. (The section 
[Examining Class Modifiers and Types](../class/classModifiers.html) in the 
[Classes](../class/index.html) lesson describes how to retrieve annotations.) There are also methods which enable dynamic access and modification of the value of the field. These tasks are covered in the following sections:

- [Obtaining Field Types](fieldTypes.html) describes how to get the declared and generic types of a field
- [Retrieving and Parsing Field Modifiers](fieldModifiers.html) shows how to get portions of the field declaration such as `public` or `transient`
- [Getting and Setting Field Values](fieldValues.html) illustrates how to access field values
- [Troubleshooting](fieldTrouble.html) describes some common coding errors which may cause confusion

When writing an application such as a class browser, it might be useful to find out which fields belong to a particular class. A class's fields are identified by invoking 
[`Class.getFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--). The 
[`getFields()`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields--) method returns an array of 
[`Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html) objects containing one object per accessible public field.

A public field is accessible if it is a member of either:

- this class
- a superclass of this class
- an interface implemented by this class
- an interface extended from an interface implemented by this class

A field may be a class (instance) field, such as 
[`java.io.Reader.lock`](https://docs.oracle.com/javase/8/docs/api/java/io/Reader.html#lock) , a static field, such as 
[`java.lang.Integer.MAX_VALUE`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#MAX_VALUE) , or an enum constant, such as 
[`java.lang.Thread.State.WAITING`](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.State.html#WAITING).
