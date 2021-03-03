---
title: Word Boundaries
author: lijiabao
date: 2020-12-06 01:51:17.099126900 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Word Boundaries

You invoke the `getWordIterator` method to instantiate a `BreakIterator` that detects word boundaries:

```

BreakIterator wordIterator =
    BreakIterator.getWordInstance(currentLocale);

```

You'll want to create such a `BreakIterator` when your application needs to perform operations on individual words. These operations might be common word- processing functions, such as selecting, cutting, pasting, and copying. Or, your application may search for words, and it must be able to distinguish entire words from simple strings.

When a `BreakIterator` analyzes word boundaries, it differentiates between words and characters that are not part of words. These characters, which include spaces, tabs, punctuation marks, and most symbols, have word boundaries on both sides.

<a name="markBoundaries" id="markBoundaries"></a> The example that follows, which is from the program 
[`BreakIteratorDemo`](examples/BreakIteratorDemo.java), marks the word boundaries in some text. The program creates the `BreakIterator` and then calls the `markBoundaries` method:

```

Locale currentLocale = new Locale ("en","US");

BreakIterator wordIterator =
    BreakIterator.getWordInstance(currentLocale);

String someText = "She stopped. " +
    "She said, \"Hello there,\" and then went " +
    "on.";

markBoundaries(someText, wordIterator);

```

The `markBoundaries` method is defined in `BreakIteratorDemo.java`. This method marks boundaries by printing carets (^) beneath the target string. In the code that follows, notice the `while` loop where `markBoundaries` scans the string by calling the `next` method:

```

static void markBoundaries(String target, BreakIterator iterator) {

    StringBuffer markers = new StringBuffer();
    markers.setLength(target.length() + 1);
    for (int k = 0; k &lt; markers.length(); k++) {
        markers.setCharAt(k,' ');
    }

    iterator.setText(target);
    int boundary = iterator.first();

    while (boundary != BreakIterator.DONE) {
        markers.setCharAt(boundary,'^');
        boundary = iterator.next();
    }

    System.out.println(target);
    System.out.println(markers);
}

```

The output of the `markBoundaries` method follows. Note where the carets (^) occur in relation to the punctuation marks and spaces:

```

She stopped.  She said, "Hello there," and then
^  ^^      ^^ ^  ^^   ^^^^    ^^    ^^^^  ^^   ^

went on.
^   ^^ ^^

```

The `BreakIterator` class makes it easy to select words from within text. You don't have to write your own routines to handle the punctuation rules of various languages; the `BreakIterator` class does this for you.

The `extractWords` method in the following example extracts and prints words for a given string. Note that this method uses `Character.isLetterOrDigit` to avoid printing "words" that contain space characters.

```

static void extractWords(String target, BreakIterator wordIterator) {

    wordIterator.setText(target);
    int start = wordIterator.first();
    int end = wordIterator.next();

    while (end != BreakIterator.DONE) {
        String word = target.substring(start,end);
        if (Character.isLetterOrDigit(word.charAt(0))) {
            System.out.println(word);
        }
        start = end;
        end = wordIterator.next();
    }
}

```

The `BreakIteratorDemo` program invokes `extractWords`, passing it the same target string used in the previous example. The `extractWords` method prints out the following list of words:

```

She
stopped
She
said
Hello
there
and
then
went
on

```
