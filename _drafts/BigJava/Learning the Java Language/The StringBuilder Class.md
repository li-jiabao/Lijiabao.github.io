
# The StringBuilder Class


[`StringBuilder` ](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html) objects are like 
[`String` ](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) objects, except that they can be modified. Internally, these objects are treated like variable-length arrays that contain a sequence of characters. At any point, the length and content of the sequence can be changed through method invocations.

Strings should always be used unless string builders offer an advantage in terms of simpler code (see the sample program at the end of this section) or better performance. For example, if you need to concatenate a large number of strings, appending to a `StringBuilder` object is more efficient.

## Length and Capacity

The `StringBuilder` class, like the `String` class, has a `length()` method that returns the length of the character sequence in the builder.

Unlike strings, every string builder also has a *capacity*, the number of character spaces that have been allocated. The capacity, which is returned by the `capacity()` method, is always greater than or equal to the length (usually greater than) and will automatically expand as necessary to accommodate additions to the string builder.
<th id="h1">Constructor</th><th id="h2" width="50%">Description</th>
<td headers="h1">`StringBuilder()`</td><td headers="h2">Creates an empty string builder with a capacity of 16 (16 empty elements).</td>
<td headers="h1">`StringBuilder(CharSequence cs)`</td><td headers="h2">Constructs a string builder containing the same characters as the specified `CharSequence`, plus an extra 16 empty elements trailing the `CharSequence`.</td>
<td headers="h1">`StringBuilder(int initCapacity)`</td><td headers="h2">Creates an empty string builder with the specified initial capacity.</td>
<td headers="h1">`StringBuilder(String s)`</td><td headers="h2">Creates a string builder whose value is initialized by the specified string, plus an extra 16 empty elements trailing the string.</td>

For example, the following code

```

// creates empty builder, capacity 16
StringBuilder sb = new StringBuilder();
// adds 9 character string at beginning
sb.append("Greetings");

```

will produce a string builder with a length of 9 and a capacity of 16: 

The `StringBuilder` class has some methods related to length and capacity that the `String` class does not have:
<th id="h101">Method</th><th id="h102" width="50%">Description</th>
<td headers="h101">`void setLength(int newLength)`</td><td headers="h102">Sets the length of the character sequence. If `newLength` is less than `length()`, the last characters in the character sequence are truncated. If `newLength` is greater than `length()`, null characters are added at the end of the character sequence.</td>
<td headers="h101">`void ensureCapacity(int minCapacity)`</td><td headers="h102">Ensures that the capacity is at least equal to the specified minimum.</td>

A number of operations (for example, `append()`, `insert()`, or `setLength()`) can increase the length of the character sequence in the string builder so that the resultant `length()` would be greater than the current `capacity()`. When this happens, the capacity is automatically increased.

## StringBuilder Operations

The principal operations on a `StringBuilder` that are not available in `String` are the `append()` and `insert()` methods, which are overloaded so as to accept data of any type. Each converts its argument to a string and then appends or inserts the characters of that string to the character sequence in the string builder. The append method always adds these characters at the end of the existing character sequence, while the insert method adds the characters at a specified point.

Here are a number of the methods of the `StringBuilder` class.
<th id="h201">Method</th><th id="h202" width="50%">Description</th>
<td headers="h201">`StringBuilder append(boolean b)<br />StringBuilder append(char c)<br />StringBuilder append(char[] str)<br />StringBuilder append(char[] str, int offset, int len)<br />StringBuilder append(double d)<br />StringBuilder append(float f)<br />StringBuilder append(int i)<br />StringBuilder append(long lng)<br />StringBuilder append(Object obj)<br />StringBuilder append(String s)`</td><td headers="h202">Appends the argument to this string builder. The data is converted to a string before the append operation takes place.</td>
<td headers="h201">`StringBuilder delete(int start, int end)<br />StringBuilder deleteCharAt(int index)`</td><td headers="h202">The first method deletes the subsequence from start to end-1 (inclusive) in the `StringBuilder`'s char sequence. The second method deletes the character located at `index`.</td>
<td headers="h201">`StringBuilder insert(int offset, boolean b)<br />StringBuilder insert(int offset, char c)<br />StringBuilder insert(int offset, char[] str)<br />StringBuilder insert(int index, char[] str, int offset, int len)<br />StringBuilder insert(int offset, double d)<br />StringBuilder insert(int offset, float f)<br />StringBuilder insert(int offset, int i)<br />StringBuilder insert(int offset, long lng)<br />StringBuilder insert(int offset, Object obj)<br />StringBuilder insert(int offset, String s)`</td><td headers="h202">Inserts the second argument into the string builder. The first integer argument indicates the index before which the data is to be inserted. The data is converted to a string before the insert operation takes place.</td>
<td headers="h201">`StringBuilder replace(int start, int end, String s)<br />void setCharAt(int index, char c)`</td><td headers="h202">Replaces the specified character(s) in this string builder.</td>
<td headers="h201">`StringBuilder reverse()`</td><td headers="h202">Reverses the sequence of characters in this string builder.</td>
<td headers="h201">`String toString()`</td><td headers="h202">Returns a string that contains the character sequence in the builder.</td>

## An Example

The `StringDemo` program that was listed in the section titled "Strings" is an example of a program that would be more efficient if a `StringBuilder` were used instead of a `String`.

`StringDemo` reversed a palindrome. Here, once again, is its listing:

```


public class StringDemo {
    public static void main(String[] args) {
        String palindrome = "Dot saw I was Tod";
        int len = palindrome.length();
        char[] tempCharArray = new char[len];
        char[] charArray = new char[len];
        
        // put original string in an 
        // array of chars
        for (int i = 0; i &lt; len; i++) {
            tempCharArray[i] = 
                palindrome.charAt(i);
        } 
        
        // reverse array of chars
        for (int j = 0; j &lt; len; j++) {
            charArray[j] =
                tempCharArray[len - 1 - j];
        }
        
        String reversePalindrome =
            new String(charArray);
        System.out.println(reversePalindrome);
    }
}

```

Running the program produces this output:

```

doT saw I was toD

```

To accomplish the string reversal, the program converts the string to an array of characters (first `for` loop), reverses the array into a second array (second `for` loop), and then converts back to a string.

If you convert the `palindrome` string to a string builder, you can use the `reverse()` method in the `StringBuilder` class. It makes the code simpler and easier to read:

```


public class StringBuilderDemo {
    public static void main(String[] args) {
        String palindrome = "Dot saw I was Tod";
         
        StringBuilder sb = new StringBuilder(palindrome);
        
        sb.reverse();  // reverse it
        
        System.out.println(sb);
    }
}

```

Running this program produces the same output:

```

doT saw I was toD

```

Note that `println()` prints a string builder, as in:

```

System.out.println(sb);

```

because `sb.toString()` is called implicitly, as it is with any other object in a `println()` invocation.
