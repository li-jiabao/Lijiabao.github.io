---
title: Improving Collation Performance
author: lijiabao
date: 2020-12-06 01:50:54.430633500 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Improving Collation Performance

Sorting long lists of strings is often time consuming. If your sort algorithm compares strings repeatedly, you can speed up the process by using the `CollationKey` class.

A 
[`CollationKey`](https://docs.oracle.com/javase/8/docs/api/java/text/CollationKey.html) object represents a sort key for a given `String` and `Collator`. Comparing two `CollationKey` objects involves a bitwise comparison of sort keys and is faster than comparing `String` objects with the `Collator.compare` method. However, generating `CollationKey` objects requires time. Therefore if a `String` is to be compared just once, `Collator.compare` offers better performance.

The example that follows uses a `CollationKey` object to sort an array of words. Source code for this example is in 
[`KeysDemo.java`](examples/KeysDemo.java).

The `KeysDemo` program creates an array of `CollationKey` objects in the `main` method. To create a `CollationKey`, you invoke the `getCollationKey` method on a `Collator` object. You cannot compare two `CollationKey` objects unless they originate from the same `Collator`. The `main` method is as follows:

```

static public void main(String[] args) {
    Collator enUSCollator = Collator.getInstance(new Locale("en","US"));
    String [] words = {
        "peach",
        "apricot",
        "grape",
        "lemon"
    };

    CollationKey[] keys = new CollationKey[words.length];

    for (int k = 0; k &lt; keys.length; k ++) {
        keys[k] = enUSCollator. getCollationKey(words[k]);
    }

    sortArray(keys);
    printArray(keys);
}

```

The `sortArray` method invokes the `CollationKey.compareTo` method. The `compareTo` method returns an integer less than, equal to, or greater than zero if the `keys[i]` object is less than, equal to, or greater than the `keys[j]` object. Note that the program compares the `CollationKey` objects, not the `String` objects from the original array of words. Here is the code for the `sortArray` method:

```

public static void sortArray(CollationKey[] keys) {
    CollationKey tmp;

    for (int i = 0; i &lt; keys.length; i++) {
        for (int j = i + 1; j &lt; keys.length; j++) {
            if (keys[i].compareTo(keys[j]) &gt; 0) {
                tmp = keys[i];
                keys[i] = keys[j];
                keys[j] = tmp; 
            }
        }
    }
}

```

The `KeysDemo` program sorts an array of `CollationKey` objects, but the original goal was to sort an array of `String` objects. To retrieve the `String` representation of each `CollationKey`, the program invokes `getSourceString` in the `displayWords` method, as follows:

```

static void displayWords(CollationKey[] keys) {
    for (int i = 0; i &lt; keys.length; i++) {
        System.out.println(keys[i].getSourceString());
    }
}

```

The `displayWords` method prints the following lines:

```

apricot
grape
lemon
peach

```
