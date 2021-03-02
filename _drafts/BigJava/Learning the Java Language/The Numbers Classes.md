
# The Numbers Classes

When working with numbers, most of the time you use the primitive types in your code. For example:

```

int i = 500;
float gpa = 3.65f;
byte mask = 0xff;

```

There are, however, reasons to use objects in place of primitives, and the Java platform provides *wrapper* classes for each of the primitive data types. These classes "wrap" the primitive in an object. Often, the wrapping is done by the compiler&#151;if you use a primitive where an object is expected, the compiler *boxes* the primitive in its wrapper class for you. Similarly, if you use a number object when a primitive is expected, the compiler *unboxes* the object for you. For more information, see
[Autoboxing and Unboxing](autoboxing.html)

All of the numeric wrapper classes are subclasses of the abstract class `Number`: <!-- figure -->

There are three reasons that you might use a `Number` object rather than a primitive:

1. As an argument of a method that expects an object (often used when manipulating collections of numbers).
1. To use constants defined by the class, such as `MIN_VALUE` and `MAX_VALUE`, that provide the upper and lower bounds of the data type.
1. To use class methods for converting values to and from other primitive types, for converting to and from strings, and for converting between number systems (decimal, octal, hexadecimal, binary).

The following table lists the instance methods that all the subclasses of the `Number` class implement.
<th id="h1">Method</th><th id="h2">Description</th>
<td headers="h1">`byte byteValue()<br />short shortValue()<br />int intValue()<br />long longValue()<br />float floatValue()<br />double doubleValue()`</td><td headers="h2">Converts the value of this `Number` object to the primitive data type returned.</td>
<td headers="h1">`int compareTo(Byte anotherByte)<br />int compareTo(Double anotherDouble)<br />int compareTo(Float anotherFloat)<br />int compareTo(Integer anotherInteger)<br />int compareTo(Long anotherLong)<br />int compareTo(Short anotherShort)`</td><td headers="h2">Compares this `Number` object to the argument.</td>
<td headers="h1">`boolean equals(Object obj)`</td><td headers="h2">Determines whether this number object is equal to the argument.<br />The methods return `true` if the argument is not `null` and is an object of the same type and with the same numeric value.<br />There are some extra requirements for `Double` and `Float` objects that are described in the Java API documentation.</td>

Each `Number` class contains other methods that are useful for converting numbers to and from strings and for converting between number systems. The following table lists these methods in the `Integer` class. Methods for the other `Number` subclasses are similar:
<th id="h101">Method</th><th id="h102">Description</th>
<td headers="h101">`static Integer decode(String s)`</td><td headers="h102">Decodes a string into an integer. Can accept string representations of decimal, octal, or hexadecimal numbers as input.</td>
<td headers="h101">`static int parseInt(String s)`</td><td headers="h102">Returns an integer (decimal only).</td>
<td headers="h101">`static int parseInt(String s, int radix)`</td><td headers="h102">Returns an integer, given a string representation of decimal, binary, octal, or hexadecimal (`radix` equals 10, 2, 8, or 16 respectively) numbers as input.</td>
<td headers="h101">`String toString()`</td><td headers="h102">Returns a `String` object representing the value of this `Integer`.</td>
<td headers="h101">`static String toString(int i)`</td><td headers="h102">Returns a `String` object representing the specified integer.</td>
<td headers="h101">`static Integer valueOf(int i)`</td><td headers="h102">Returns an `Integer` object holding the value of the specified primitive.</td>
<td headers="h101">`static Integer valueOf(String s)`</td><td headers="h102">Returns an `Integer` object holding the value of the specified string representation.</td>
<td headers="h101">`static Integer valueOf(String s, int radix)`</td><td headers="h102">Returns an `Integer` object holding the integer value of the specified string representation, parsed with the value of radix. For example, if s = "333" and radix = 8, the method returns the base-ten integer equivalent of the octal number 333.</td>
