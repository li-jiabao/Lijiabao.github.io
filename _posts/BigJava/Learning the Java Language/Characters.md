
# Characters

Most of the time, if you are using a single character value, you will use the primitive `char` type. For example:

```

char ch = 'a'; 
// Unicode for uppercase Greek omega character
char uniChar = '\u03A9';
// an array of chars
char[] charArray = { 'a', 'b', 'c', 'd', 'e' };

```

There are times, however, when you need to use a char as an object&#151;for example, as a method argument where an object is expected. The Java programming language provides a *wrapper* class that "wraps" the `char` in a `Character` object for this purpose. An object of type `Character` contains a single field, whose type is `char`. This 
[Character](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) class also offers a number of useful class (i.e., static) methods for manipulating characters.

You can create a `Character` object with the `Character` constructor:

```

Character ch = new Character('a');

```

The Java compiler will also create a `Character` object for you under some circumstances. For example, if you pass a primitive `char` into a method that expects an object, the compiler automatically converts the `char` to a `Character` for you. This feature is called **autoboxing**&#151;or **unboxing**, if the conversion goes the other way. For more information on autoboxing and unboxing, see
[Autoboxing and Unboxing](autoboxing.html).

The following table lists some of the most useful methods in the `Character` class, but is not exhaustive. For a complete listing of all methods in this class (there are more than 50), refer to the 
[java.lang.Character](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) API specification.
<th id="h1" width="30%">Method</th><th id="h2">Description</th>
<td headers="h1">`boolean isLetter(char ch)<br />boolean isDigit(char ch)`</td><td headers="h2">Determines whether the specified char value is a letter or a digit, respectively.</td>
<td headers="h1">`boolean isWhitespace(char ch)`</td><td headers="h2">Determines whether the specified char value is white space.</td>
<td headers="h1">`boolean isUpperCase(char ch)<br />boolean isLowerCase(char ch)`</td><td headers="h2">Determines whether the specified char value is uppercase or lowercase, respectively.</td>
<td headers="h1">`char toUpperCase(char ch)<br />char toLowerCase(char ch)`</td><td headers="h2">Returns the uppercase or lowercase form of the specified char value.</td>
<td headers="h1">`toString(char ch)`</td><td headers="h2">Returns a `String` object representing the specified character value &#151; that is, a one-character string.</td>

## Escape Sequences

A character preceded by a backslash (\) is an *escape sequence* and has special meaning to the compiler. The following table shows the Java escape sequences:
<th id="h101" width="30%">Escape Sequence</th><th id="h102">Description</th>
<td headers="h101">`\t`</td><td headers="h102">Insert a tab in the text at this point.</td>
<td headers="h101">`\b`</td><td headers="h102">Insert a backspace in the text at this point.</td>
<td headers="h101">`\n`</td><td headers="h102">Insert a newline in the text at this point.</td>
<td headers="h101">`\r`</td><td headers="h102">Insert a carriage return in the text at this point.</td>
<td headers="h101">`\f`</td><td headers="h102">Insert a formfeed in the text at this point.</td>
<td headers="h101">`\'`</td><td headers="h102">Insert a single quote character in the text at this point.</td>
<td headers="h101">`\"`</td><td headers="h102">Insert a double quote character in the text at this point.</td>
<td headers="h101">`\\`</td><td headers="h102">Insert a backslash character in the text at this point.</td>

When an escape sequence is encountered in a print statement, the compiler interprets it accordingly. For example, if you want to put quotes within quotes you must use the escape sequence, \", on the interior quotes. To print the sentence

```

She said "Hello!" to me.

```

you would write

```

System.out.println("She said \"Hello!\" to me.");

```
