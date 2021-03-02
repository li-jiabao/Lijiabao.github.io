
# Character Classes

If you browse through the 
[`Pattern`](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) class specification, you'll see tables summarizing the supported regular expression constructs. In the "Character Classes" section you'll find the following:

<a name="CHART" id="CHART"></a>
<th id="h1">Construct</th><th id="h2">Description</th>
<td headers="h1">`[abc]`</td><td headers="h2">a, b, or c (simple class)</td>
<td headers="h1">`[^abc]`</td><td headers="h2">Any character except a, b, or c (negation)</td>
<td headers="h1">`[a-zA-Z]`</td><td headers="h2">a through z, or A through Z, inclusive (range)</td>
<td headers="h1">`[a-d[m-p]]`</td><td headers="h2">a through d, or m through p: [a-dm-p] (union)</td>
<td headers="h1">`[a-z&amp;&amp;[def]]`</td><td headers="h2">d, e, or f (intersection)</td>
<td headers="h1">`[a-z&amp;&amp;[^bc]]`</td><td headers="h2">a through z, except for b and c: [ad-z] (subtraction)</td>
<td headers="h1">`[a-z&amp;&amp;[^m-p]]`</td><td headers="h2">a through z, and not m through p: [a-lq-z] (subtraction)</td>

The left-hand column specifies the regular expression constructs, while the right-hand column describes the conditions under which each construct will match.

## Simple Classes

The most basic form of a character class is to simply place a set of characters side-by-side within square brackets. For example, the regular expression `[bcr]at` will match the words "bat", "cat", or "rat" because it defines a character class (accepting either "b", "c", or "r") as its first character.

```
 
Enter your regex: [bcr]at
Enter input string to search: bat
I found the text "bat" starting at index 0 and ending at index 3.

Enter your regex: [bcr]at
Enter input string to search: cat
I found the text "cat" starting at index 0 and ending at index 3.

Enter your regex: [bcr]at
Enter input string to search: rat
I found the text "rat" starting at index 0 and ending at index 3.

Enter your regex: [bcr]at
Enter input string to search: hat
No match found.

```

In the above examples, the overall match succeeds only when the first letter matches one of the characters defined by the character class.

### Negation

To match all characters *except* those listed, insert the "`^`" metacharacter at the beginning of the character class. This technique is known as *negation*.

```
 
Enter your regex: [^bcr]at
Enter input string to search: bat
No match found.

Enter your regex: [^bcr]at
Enter input string to search: cat
No match found.

Enter your regex: [^bcr]at
Enter input string to search: rat
No match found.

Enter your regex: [^bcr]at
Enter input string to search: hat
I found the text "hat" starting at index 0 and ending at index 3.

```

The match is successful only if the first character of the input string does *not* contain any of the characters defined by the character class.

### Ranges

Sometimes you'll want to define a character class that includes a range of values, such as the letters "a through h" or the numbers "1 through 5". To specify a range, simply insert the "`-`" metacharacter between the first and last character to be matched, such as `[1-5]` or `[a-h]`. You can also place different ranges beside each other within the class to further expand the match possibilities. For example, `[a-zA-Z]` will match any letter of the alphabet: a to z (lowercase) or A to Z (uppercase).

Here are some examples of ranges and negation:

```

Enter your regex: [a-c]
Enter input string to search: a
I found the text "a" starting at index 0 and ending at index 1.

Enter your regex: [a-c]
Enter input string to search: b
I found the text "b" starting at index 0 and ending at index 1.

Enter your regex: [a-c]
Enter input string to search: c
I found the text "c" starting at index 0 and ending at index 1.

Enter your regex: [a-c]
Enter input string to search: d
No match found.

Enter your regex: foo[1-5]
Enter input string to search: foo1
I found the text "foo1" starting at index 0 and ending at index 4.

Enter your regex: foo[1-5]
Enter input string to search: foo5
I found the text "foo5" starting at index 0 and ending at index 4.

Enter your regex: foo[1-5]
Enter input string to search: foo6
No match found.

Enter your regex: foo[^1-5]
Enter input string to search: foo1
No match found.

Enter your regex: foo[^1-5]
Enter input string to search: foo6
I found the text "foo6" starting at index 0 and ending at index 4.

```

### Unions

You can also use *unions* to create a single character class comprised of two or more separate character classes. To create a union, simply nest one class inside the other, such as `[0-4[6-8]]`. This particular union creates a single character class that matches the numbers 0, 1, 2, 3, 4, 6, 7, and 8.

```

Enter your regex: [0-4[6-8]]
Enter input string to search: 0
I found the text "0" starting at index 0 and ending at index 1.

Enter your regex: [0-4[6-8]]
Enter input string to search: 5
No match found.

Enter your regex: [0-4[6-8]]
Enter input string to search: 6
I found the text "6" starting at index 0 and ending at index 1.

Enter your regex: [0-4[6-8]]
Enter input string to search: 8
I found the text "8" starting at index 0 and ending at index 1.

Enter your regex: [0-4[6-8]]
Enter input string to search: 9
No match found.

```

### Intersections

To create a single character class matching only the characters common to all of its nested classes, use `&amp;&amp;`, as in `[0-9&amp;&amp;[345]]`. This particular intersection creates a single character class matching only the numbers common to both character classes: 3, 4, and 5.

```
 
Enter your regex: [0-9&amp;&amp;[345]]
Enter input string to search: 3
I found the text "3" starting at index 0 and ending at index 1.

Enter your regex: [0-9&amp;&amp;[345]]
Enter input string to search: 4
I found the text "4" starting at index 0 and ending at index 1.

Enter your regex: [0-9&amp;&amp;[345]]
Enter input string to search: 5
I found the text "5" starting at index 0 and ending at index 1.

Enter your regex: [0-9&amp;&amp;[345]]
Enter input string to search: 2
No match found.

Enter your regex: [0-9&amp;&amp;[345]]
Enter input string to search: 6
No match found.

```

And here's an example that shows the intersection of two ranges:

```
 
Enter your regex: [2-8&amp;&amp;[4-6]]
Enter input string to search: 3
No match found.

Enter your regex: [2-8&amp;&amp;[4-6]]
Enter input string to search: 4
I found the text "4" starting at index 0 and ending at index 1.

Enter your regex: [2-8&amp;&amp;[4-6]]
Enter input string to search: 5
I found the text "5" starting at index 0 and ending at index 1.

Enter your regex: [2-8&amp;&amp;[4-6]]
Enter input string to search: 6
I found the text "6" starting at index 0 and ending at index 1.

Enter your regex: [2-8&amp;&amp;[4-6]]
Enter input string to search: 7
No match found.

```

### Subtraction

Finally, you can use *subtraction* to negate one or more nested character classes, such as `[0-9&amp;&amp;[^345]]`. This example creates a single character class that matches everything from 0 to 9, *except* the numbers 3, 4, and 5.

```
 
Enter your regex: [0-9&amp;&amp;[^345]]
Enter input string to search: 2
I found the text "2" starting at index 0 and ending at index 1.

Enter your regex: [0-9&amp;&amp;[^345]]
Enter input string to search: 3
No match found.

Enter your regex: [0-9&amp;&amp;[^345]]
Enter input string to search: 4
No match found.

Enter your regex: [0-9&amp;&amp;[^345]]
Enter input string to search: 5
No match found.

Enter your regex: [0-9&amp;&amp;[^345]]
Enter input string to search: 6
I found the text "6" starting at index 0 and ending at index 1.

Enter your regex: [0-9&amp;&amp;[^345]]
Enter input string to search: 9
I found the text "9" starting at index 0 and ending at index 1.

```

Now that we've covered how character classes are created, You may want to review the [Character Classes table](#CHART) before continuing with the next section.
