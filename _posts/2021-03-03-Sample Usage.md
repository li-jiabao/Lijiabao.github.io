---
title: Sample Usage
author: lijiabao
date: 2020-12-06 01:51:03.574960900 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Sample Usage

This page contains some code snippets that show you several commono scenarios.

## Creating a `String` from a Code Point

```

String newString(int codePoint) {
    return new String(Character.toChars(codePoint));
}

```

## Creating a `String` from a Code Point - Optimized for BMP Characters

The `Character.toChars` method creates an temporary array that is used once and then discarded. If this negatively affects performance, you can use the following approach that is optimizied for BMP characters (characters that are represented by a single `char` value). In this method, `toChars` is invoked only for supplementary characters.

```

String newString(int codePoint) {
    if (Character.charCount(codePoint) == 1) {
        return String.valueOf(codePoint);
    } else {
        return new String(Character.toChars(codePoint));
    }
}

```

## Creating `String` Objects in Bulk

To create a large number of strings, the bulk version of the previous snippet reuses the array used by the `toChars` method. This method creates a separate `String` instance for each code point and is optimized for BMP characters.

```

String[] newStrings(int[] codePoints) {
    String[] result = new String[codePoints.length];
    char[] codeUnits = new char[2];
    for (int i = 0; i &lt; codePoints.length; i++) {
        int count = Character.toChars(codePoints[i], codeUnits, 0);
        result[i] = new String(codeUnits, 0, count);
    }
    return result;
}

```

## Generating Messages

The formatting API supports supplementary characters. The following example is a simple way to generate a message.

```

// recommended
System.out.printf("Character %c is invalid.%n", codePoint);

```

This following approach is simple and avoids concatenation, which makes the text more difficult to localize as not all languages insert numeric values into a string in the same order as English.

```

// not recommended
System.out.println("Character " + String.valueOf(char) + " is invalid.");

```
