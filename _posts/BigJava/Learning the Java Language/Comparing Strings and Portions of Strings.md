
# Comparing Strings and Portions of Strings

The `String` class has a number of methods for comparing strings and portions of strings. The following table lists these methods.
<th id="h1">Method</th><th id="h2" width="50%">Description</th>
<td headers="h1">`boolean endsWith(String suffix)<br />boolean startsWith(String prefix)`</td><td headers="h2">Returns `true` if this string ends with or begins with the substring specified as an argument to the method.</td>
<td headers="h1">`boolean startsWith(String prefix, int offset)`</td><td headers="h2">Considers the string beginning at the index `offset`, and returns `true` if it begins with the substring specified as an argument.</td>
<td headers="h1">`int compareTo(String anotherString)`</td><td headers="h2">Compares two strings lexicographically. Returns an integer indicating whether this string is greater than (result is &gt; 0), equal to (result is = 0), or less than (result is &lt; 0) the argument.</td>
<td headers="h1">`int compareToIgnoreCase(String str)`</td><td headers="h2">Compares two strings lexicographically, ignoring differences in case. Returns an integer indicating whether this string is greater than (result is &gt; 0), equal to (result is = 0), or less than (result is &lt; 0) the argument.</td>
<td headers="h1">`boolean equals(Object anObject)`</td><td headers="h2">Returns `true` if and only if the argument is a `String` object that represents the same sequence of characters as this object.</td>
<td headers="h1">`boolean equalsIgnoreCase(String anotherString)`</td><td headers="h2">Returns `true` if and only if the argument is a `String` object that represents the same sequence of characters as this object, ignoring differences in case.</td>
<td headers="h1">`boolean regionMatches(int toffset, String other, int ooffset, int len)`</td><td headers="h2">Tests whether the specified region of this string matches the specified region of the String argument.Region is of length `len` and begins at the index `toffset` for this string and `ooffset` for the other string.</td>
<td headers="h1">`boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)`</td><td headers="h2">Tests whether the specified region of this string matches the specified region of the String argument.Region is of length `len` and begins at the index `toffset` for this string and `ooffset` for the other string.The boolean argument indicates whether case should be ignored; if true, case is ignored when comparing characters.</td>

The boolean argument indicates whether case should be ignored; if true, case is ignored when comparing characters.
<td headers="h1">`boolean matches(String regex)`</td><td headers="h2">Tests whether this string matches the specified regular expression. Regular expressions are discussed in the lesson titled "Regular Expressions."</td>

The following program, `RegionMatchesDemo`, uses the `regionMatches` method to search for a string within another string:

```


public class RegionMatchesDemo {
    public static void main(String[] args) {
        String searchMe = "Green Eggs and Ham";
        String findMe = "Eggs";
        int searchMeLength = searchMe.length();
        int findMeLength = findMe.length();
        boolean foundIt = false;
        for (int i = 0; 
             i &lt;= (searchMeLength - findMeLength);
             i++) {
           if (searchMe.regionMatches(i, findMe, 0, findMeLength)) {
              foundIt = true;
              System.out.println(searchMe.substring(i, i + findMeLength));
              break;
           }
        }
        if (!foundIt)
            System.out.println("No match found.");
    }
}

```

The output from this program is `Eggs`.

The program steps through the string referred to by `searchMe` one character at a time. For each character, the program calls the regionMatches method to determine whether the substring beginning with the current character matches the string the program is looking for.
