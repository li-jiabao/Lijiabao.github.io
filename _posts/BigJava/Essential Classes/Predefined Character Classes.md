
# Predefined Character Classes

The 
[`Pattern`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) API contains a number of useful *predefined character classes*, which offer convenient shorthands for commonly used regular expressions:

<a name="CHART" id="CHART"></a>
<th id="h1">Construct</th><th id="h2">Description</th>
<td headers="h1">`.`</td><td headers="h2">Any character (may or may not match line terminators)</td>
<td headers="h1">`\d`</td><td headers="h2">A digit: `[0-9]`</td>
<td headers="h1">`\D`</td><td headers="h2">A non-digit: `[^0-9]`</td>
<td headers="h1">`\s`</td><td headers="h2">A whitespace character: `[ \t\n\x0B\f\r]`</td>
<td headers="h1">`\S`</td><td headers="h2">A non-whitespace character: `[^\s]`</td>
<td headers="h1">`\w`</td><td headers="h2">A word character: `[a-zA-Z_0-9]`</td>
<td headers="h1">`\W`</td><td headers="h2">A non-word character: `[^\w]`</td>

In the table above, each construct in the left-hand column is shorthand for the character class in the right-hand column. For example, `\d` means a range of digits (0-9), and `\w` means a word character (any lowercase letter, any uppercase letter, the underscore character, or any digit). Use the predefined classes whenever possible. They make your code easier to read and eliminate errors introduced by malformed character classes.

Constructs beginning with a backslash are called *escaped constructs*. We previewed escaped constructs in the 
[String Literals](literals.html) section where we mentioned the use of backslash and `\Q` and `\E` for quotation. If you are using an escaped construct within a string literal, you must precede the backslash with another backslash for the string to compile. For example:

```
 
private final String REGEX = "\\d"; // a single digit

```

In this example `\d` is the regular expression; the extra backslash is required for the code to compile. The test harness reads the expressions directly from the `Console`, however, so the extra backslash is unnecessary.

The following examples demonstrate the use of predefined character classes.

```
 
Enter your regex: .
Enter input string to search: @
I found the text "@" starting at index 0 and ending at index 1.

Enter your regex: . 
Enter input string to search: 1
I found the text "1" starting at index 0 and ending at index 1.

Enter your regex: .
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: \d
Enter input string to search: 1
I found the text "1" starting at index 0 and ending at index 1.

Enter your regex: \d
Enter input string to search: a
No match found.

Enter your regex: \D
Enter input string to search: 1
No match found.

Enter your regex: \D
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: \s
Enter input string to search:  
I found the text " " starting at index 0 and ending at index 1.

Enter your regex: \s
Enter input string to search: a
No match found.

Enter your regex: \S
Enter input string to search:  
No match found.

Enter your regex: \S
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: \w
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: \w
Enter input string to search: !
No match found.

Enter your regex: \W
Enter input string to search: a
No match found.

Enter your regex: \W
Enter input string to search: !
I found the text "!" starting at index 0 and ending at index 1.

```

In the first three examples, the regular expression is simply `.` (the "dot" metacharacter) that indicates "any character." Therefore, the match is successful in all three cases (a randomly selected `@` character, a digit, and a letter). The remaining examples each use a single regular expression construct from the [Predefined Character Classes table](#CHART). You can refer to this table to figure out the logic behind each match:

- `\d` matches all digits
- `\s` matches spaces
- `\w` matches word characters

Alternatively, a capital letter means the opposite:

- `\D` matches non-digits
- `\S` matches non-spaces
- `\W` matches non-word characters
