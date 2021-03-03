---
title: About the BreakIterator Class
author: lijiabao
date: 2020-12-06 01:51:11.873167900 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# About the BreakIterator Class

The `BreakIterator` class is locale-sensitive, because text boundaries vary with language. For example, the syntax rules for line breaks are not the same for all languages. To determine which locales the `BreakIterator` class supports, invoke the `getAvailableLocales` method, as follows:

```

Locale[] locales = BreakIterator.getAvailableLocales();

```

You can analyze four kinds of boundaries with the `BreakIterator` class: character, word, sentence, and potential line break. When instantiating a `BreakIterator`, you invoke the appropriate factory method:

- `getCharacterInstance`
- `getWordInstance`
- `getSentenceInstance`
- `getLineInstance`

Each instance of `BreakIterator` can detect just one type of boundary. If you want to locate both character and word boundaries, for example, you create two separate instances.

A `BreakIterator` has an imaginary cursor that points to the current boundary in a string of text. You can move this cursor within the text with the `previous` and the `next` methods. For example, if you've created a `BreakIterator` with `getWordInstance`, the cursor moves to the next word boundary in the text every time you invoke the `next` method. The cursor-movement methods return an integer indicating the position of the boundary. This position is the index of the character in the text string that would follow the boundary. Like string indexes, the boundaries are zero-based. The first boundary is at 0, and the last boundary is the length of the string. The following figure shows the word boundaries detected by the `next` and `previous` methods in a line of text:

You should use the `BreakIterator` class only with natural-language text. To tokenize a programming language, use the `StreamTokenizer` class.

The sections that follow give examples for each type of boundary analysis. The coding examples are from the source code file named 
[`BreakIteratorDemo.java`](examples/BreakIteratorDemo.java).
