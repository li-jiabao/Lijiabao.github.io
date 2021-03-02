
# Converting Non-Unicode Text

In the Java programming language `char` values represent Unicode characters. Unicode is a 16-bit character encoding that supports the world's major languages. You can learn more about the Unicode standard at the 
[Unicode Consortium Web site](http://www.unicode.org/) .

Few text editors currently support Unicode text entry. The text editor we used to write this section's code examples supports only ASCII characters, which are limited to 7 bits. To indicate Unicode characters that cannot be represented in ASCII, such as &#246;, we used the `\uXXXX` escape sequence. Each `X` in the escape sequence is a hexadecimal digit. The following example shows how to indicate the &#246; character with an escape sequence:

```

String str = "\u00F6";
char c = '\u00F6';
Character letter = new Character('\u00F6');

```

A variety of character encodings are used by systems around the world. Currently few of these encodings conform to Unicode. Because your program expects characters in Unicode, the text data it gets from the system must be converted into Unicode, and vice versa. Data in text files is automatically converted to Unicode when its encoding matches the default file encoding of the Java Virtual Machine. You can identify the default file encoding by creating an `OutputStreamWriter` using it and asking for its canonical name:

```

OutputStreamWriter out = new OutputStreamWriter(new ByteArrayOutputStream());
System.out.println(out.getEncoding());

```

If the default file encoding differs from the encoding of the text data you want to process, then you must perform the conversion yourself. You might need to do this when processing text from another country or computing platform.

This section discusses the APIs you use to translate non-Unicode text into Unicode. Before using these APIs, you should verify that the character encoding you wish to convert into Unicode is supported. The list of supported character encodings is not part of the Java programming language specification. Therefore the character encodings supported by the APIs may vary with platform. To see which encodings the Java Development Kit supports, see the 
[Supported Encodings](https://docs.oracle.com/javase/8/docs/technotes/guides/intl/encoding.doc.html) document.

The material that follows describes two techniques for converting non-Unicode text to Unicode. You can convert non-Unicode byte arrays into `String` objects, and vice versa. Or you can translate between streams of Unicode characters and byte streams of non-Unicode text.

## [Byte Encodings and Strings](string.html)

This section shows you how to convert non-Unicode byte arrays into `String` objects, and vice versa.

## [Character and Byte Streams](stream.html)

In this section you'll learn how to translate between streams of Unicode characters and byte streams of non-Unicode text.
