---
title: Sentence Boundaries
author: lijiabao
date: 2020-12-06 01:51:19.826798600 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Sentence Boundaries

You can use a `BreakIterator` to determine sentence boundaries. You start by creating a `BreakIterator` with the `getSentenceInstance` method:

```

BreakIterator sentenceIterator =
    BreakIterator.getSentenceInstance(currentLocale);

```

To show the sentence boundaries, the program uses the `markBoundaries` method, which is discussed in the section [Word Boundaries](word.html). The `markBoundaries` method prints carets (^) beneath a string to indicate boundary positions. Here are some examples:

```

She stopped.  She said, "Hello there," and then went on.
^             ^                                         ^

He's vanished!  What will we do?  It's up to us.
^               ^                 ^             ^

Please add 1.5 liters to the tank.
^                                 ^

```
