
# Manipulating Characters in a String

The `String` class has a number of methods for examining the contents of strings, finding characters or substrings within a string, changing case, and other tasks.

## Getting Characters and Substrings by Index

You can get the character at a particular index within a string by invoking the `charAt()` accessor method. The index of the first character is 0, while the index of the last character is `length()-1`. For example, the following code gets the character at index 9 in a string:

```

String anotherPalindrome = "Niagara. O roar again!"; 
char aChar = anotherPalindrome.charAt(9);

```

Indices begin at 0, so the character at index 9 is 'O', as illustrated in the following figure: <!-- figure -->

If you want to get more than one consecutive character from a string, you can use the `substring` method. The `substring` method has two versions, as shown in the following table:
<th id="h1">Method</th><th id="h2" width="60%">Description</th>
<td headers="h1">`String substring(int beginIndex, int endIndex)`</td><td headers="h2">Returns a new string that is a substring of this string. The substring begins at the specified `beginIndex` and extends to the character at index `endIndex - 1`.</td>
<td headers="h1">`String substring(int beginIndex)`</td><td headers="h2">Returns a new string that is a substring of this string. The integer argument specifies the index of the first character. Here, the returned substring extends to the end of the original string.</td>

The following code gets from the Niagara palindrome the substring that extends from index 11 up to, but not including, index 15, which is the word "roar":

```

String anotherPalindrome = "Niagara. O roar again!"; 
String roar = anotherPalindrome.substring(11, 15); 

```

## Other Methods for Manipulating Strings

Here are several other `String` methods for manipulating strings:
<th id="h101">Method</th><th id="h102" width="50%">Description</th>
<td headers="h101">`String[] split(String regex)`<br />`String[] split(String regex, int limit)`</td><td headers="h102">Searches for a match as specified by the string argument (which contains a regular expression) and splits this string into an array of strings accordingly. The optional integer argument specifies the maximum size of the returned array. Regular expressions are covered in the lesson titled "Regular Expressions."</td>
<td headers="h101">`CharSequence subSequence(int beginIndex, int endIndex)`</td><td headers="h102">Returns a new character sequence constructed from `beginIndex` index up until `endIndex` - 1.</td>
<td headers="h101">`String trim()`</td><td headers="h102">Returns a copy of this string with leading and trailing white space removed.</td>
<td headers="h101">`String toLowerCase()<br />String toUpperCase()`</td><td headers="h102">Returns a copy of this string converted to lowercase or uppercase. If no conversions are necessary, these methods return the original string.</td>

## Searching for Characters and Substrings in a String

Here are some other `String` methods for finding characters or substrings within a string. The `String` class provides accessor methods that return the position within the string of a specific character or substring: `indexOf()` and `lastIndexOf()`. The `indexOf()` methods search forward from the beginning of the string, and the `lastIndexOf()` methods search backward from the end of the string. If a character or substring is not found, `indexOf()` and `lastIndexOf()` return -1.

The `String` class also provides a search method, `contains`, that returns true if the string contains a particular character sequence. Use this method when you only need to know that the string contains a character sequence, but the precise location isn't important.

The following table describes the various string search methods.
<th id="h201">Method</th><th id="h202" width="50%">Description</th>
<td headers="h201">`int indexOf(int ch)<br />int lastIndexOf(int ch)`</td><td headers="h202">Returns the index of the first (last) occurrence of the specified character.</td>
<td headers="h201">`int indexOf(int ch, int fromIndex)<br />int lastIndexOf(int ch, int fromIndex)`</td><td headers="h202">Returns the index of the first (last) occurrence of the specified character, searching forward (backward) from the specified index.</td>
<td headers="h201">`int indexOf(String str)<br />int lastIndexOf(String str)`</td><td headers="h202">Returns the index of the first (last) occurrence of the specified substring.</td>
<td headers="h201">`int indexOf(String str, int fromIndex)<br />int lastIndexOf(String str, int fromIndex)`</td><td headers="h202">Returns the index of the first (last) occurrence of the specified substring, searching forward (backward) from the specified index.</td>
<td headers="h201">`boolean contains(CharSequence s)`</td><td headers="h202">Returns true if the string contains the specified character sequence.</td>

## Replacing Characters and Substrings into a String

The `String` class has very few methods for inserting characters or substrings into a string. In general, they are not needed: You can create a new string by concatenation of substrings you have *removed* from a string with the substring that you want to insert.

The `String` class does have four methods for *replacing* found characters or substrings, however. They are:
<th id="h301">Method</th><th id="h302" width="50%">Description</th>
<td headers="h301">`String replace(char oldChar, char newChar)`</td><td headers="h302">Returns a new string resulting from replacing all occurrences of oldChar in this string with newChar.</td>
<td headers="h301">`String replace(CharSequence target, CharSequence replacement)`</td><td headers="h302">Replaces each substring of this string that matches the literal target sequence with the specified literal replacement sequence.</td>
<td headers="h301">`String replaceAll(String regex, String replacement)`</td><td headers="h302">Replaces each substring of this string that matches the given regular expression with the given replacement.</td>
<td headers="h301">`String replaceFirst(String regex, String replacement)`</td><td headers="h302">Replaces the first substring of this string that matches the given regular expression with the given replacement.</td>

## An Example

The following class, 
[`Filename`](examples/Filename.java), illustrates the use of `lastIndexOf()` and `substring()` to isolate different parts of a file name.

```


public class Filename {
    private String fullPath;
    private char pathSeparator, 
                 extensionSeparator;

    public Filename(String str, char sep, char ext) {
        fullPath = str;
        pathSeparator = sep;
        extensionSeparator = ext;
    }

    public String extension() {
        int dot = fullPath.lastIndexOf(extensionSeparator);
        return fullPath.substring(dot + 1);
    }

    // gets filename without extension
    public String filename() {
        int dot = fullPath.lastIndexOf(extensionSeparator);
        int sep = fullPath.lastIndexOf(pathSeparator);
        return fullPath.substring(sep + 1, dot);
    }

    public String path() {
        int sep = fullPath.lastIndexOf(pathSeparator);
        return fullPath.substring(0, sep);
    }
}

```

Here is a program, 
[`FilenameDemo`](examples/FilenameDemo.java), that constructs a `Filename` object and calls all of its methods:

```


public class FilenameDemo {
    public static void main(String[] args) {
        final String FPATH = "/home/user/index.html";
        Filename myHomePage = new Filename(FPATH, '/', '.');
        System.out.println("Extension = " + myHomePage.extension());
        System.out.println("Filename = " + myHomePage.filename());
        System.out.println("Path = " + myHomePage.path());
    }
}

```

And here's the output from the program:

```

Extension = html
Filename = index
Path = /home/user

```

As shown in the following figure, our `extension` method uses `lastIndexOf` to locate the last occurrence of the period (.) in the file name. Then `substring` uses the return value of `lastIndexOf` to extract the file name extension &#151; that is, the substring from the period to the end of the string. This code assumes that the file name has a period in it; if the file name does not have a period, `lastIndexOf` returns -1, and the substring method throws a `StringIndexOutOfBoundsException`.

Also, notice that the `extension` method uses `dot + 1` as the argument to `substring`. If the period character (.) is the last character of the string, `dot + 1` is equal to the length of the string, which is one larger than the largest index into the string (because indices start at 0). This is a legal argument to `substring` because that method accepts an index equal to, but not greater than, the length of the string and interprets it to mean "the end of the string."
