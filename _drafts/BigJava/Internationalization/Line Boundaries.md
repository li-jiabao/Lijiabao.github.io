
# Line Boundaries

Applications that format text or that perform line wrapping must locate potential line breaks. You can find these line breaks, or boundaries, with a `BreakIterator` that has been created with the `getLineInstance` method:

```

BreakIterator lineIterator =
    BreakIterator.getLineInstance(currentLocale);

```

This `BreakIterator` determines the positions in a string where text can break to continue on the next line. The positions detected by the `BreakIterator` are potential line breaks. The actual line breaks displayed on the screen may not be the same.

The two examples that follow use the [`markBoundaries`](word.html#markBoundaries) method of 
[`BreakIteratorDemo.java`](examples/BreakIteratorDemo.java) to show the line boundaries detected by a `BreakIterator`. The `markBoundaries` method indicates line boundaries by printing carets (^) beneath the target string.

According to a `BreakIterator`, a line boundary occurs after the termination of a sequence of whitespace characters (space, tab, new line). In the following example, note that you can break the line at any of the boundaries detected:

```

She stopped.  She said, "Hello there," and then went on.
^   ^         ^   ^     ^      ^     ^ ^   ^    ^    ^  ^

```

Potential line breaks also occur immediately after a hyphen:

```

There are twenty-four hours in a day.
^     ^   ^      ^    ^     ^  ^ ^   ^

```

The next example breaks a long string of text into fixed-length lines with a method called `formatLines`. This method uses a `BreakIterator` to locate the potential line breaks. The `formatLines` method is short, simple, and, thanks to the `BreakIterator`, locale-independent. Here is the source code:

```

static void formatLines(
    String target, int maxLength,
    Locale currentLocale) {

    BreakIterator boundary = BreakIterator.
        getLineInstance(currentLocale);
    boundary.setText(target);
    int start = boundary.first();
    int end = boundary.next();
    int lineLength = 0;

    while (end != BreakIterator.DONE) {
        String word = target.substring(start,end);
        lineLength = lineLength + word.length();
        if (lineLength &gt;= maxLength) {
            System.out.println();
            lineLength = word.length();
        }
        System.out.print(word);
        start = end;
        end = boundary.next();
    }
}

```

The `BreakIteratorDemo` program invokes the `formatLines` method as follows:

```

String moreText =
    "She said, \"Hello there,\" and then " +
    "went on down the street. When she stopped " +
    "to look at the fur coats in a shop + "
    "window, her dog growled. \"Sorry Jake,\" " +
    "she said. \"I didn't know you would take " +
    "it personally.\"";

formatLines(moreText, 30, currentLocale);

```

The output from this call to `formatLines` is:

```

She said, "Hello there," and
then went on down the
street. When she stopped to
look at the fur coats in a
shop window, her dog
growled. "Sorry Jake," she
said. "I didn't know you
would take it personally."

```
