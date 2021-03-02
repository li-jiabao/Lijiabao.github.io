
# Summary of Characters and Strings

Most of the time, if you are using a single character value, you will use the primitive `char` type. There are times, however, when you need to use a char as an object&#151;for example, as a method argument where an object is expected. The Java programming language provides a *wrapper* class that "wraps" the `char` in a `Character` object for this purpose. An object of type `Character` contains a single field whose type is `char`. This 
[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) class also offers a number of useful class (i.e., static) methods for manipulating characters.

Strings are a sequence of characters and are widely used in Java programming. In the Java programming language, strings are objects. The 
[`String` ](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) class has over 60 methods and 13 constructors.

Most commonly, you create a string with a statement like

```

String s = "Hello world!";

```

rather than using one of the `String` constructors.

The `String` class has many methods to find and retrieve substrings; these can then be easily reassembled into new strings using the `+` concatenation operator.

The `String` class also includes a number of utility methods, among them `split()`, `toLowerCase()`, `toUpperCase()`, and `valueOf()`. The latter method is indispensable in converting user input strings to numbers. The `Number` subclasses also have methods for converting strings to numbers and vice versa.

In addition to the `String` class, there is also a 
[`StringBuilder` ](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html) class. Working with `StringBuilder` objects can sometimes be more efficient than working with strings. The `StringBuilder` class offers a few methods that can be useful for strings, among them `reverse()`. In general, however, the `String` class has a wider variety of methods.

A string can be converted to a string builder using a `StringBuilder` constructor. A string builder can be converted to a string with the `toString()` method.
