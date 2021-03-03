---
title: Customizing Collation Rules
author: lijiabao
date: 2020-12-06 01:50:52.532034300 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Customizing Collation Rules

The previous section discussed how to use the predefined rules for a locale to compare strings. These collation rules determine the sort order of strings. If the predefined collation rules do not meet your needs, you can design your own rules and assign them to a `RuleBasedCollator` object.

Customized collation rules are contained in a `String` object that is passed to the `RuleBasedCollator` constructor. Here's a simple example:

```

String simpleRule = "&lt; a &lt; b &lt; c &lt; d";
RuleBasedCollator simpleCollator =  new RuleBasedCollator(simpleRule);

```

For the `simpleCollator` object in the previous example, `a` is less than `b`, which is less that `c`, and so forth. The `simpleCollator.compare` method references these rules when comparing strings. The full syntax used to construct a collation rule is more flexible and complex than this simple example. For a full description of the syntax, refer to the API documentation for the 
[`RuleBasedCollator`](https://docs.oracle.com/javase/8/docs/api/java/text/RuleBasedCollator.html) class.

The example that follows sorts a list of Spanish words with two collators. Full source code for this example is in 
[`RulesDemo.java`](examples/RulesDemo.java).

The `RulesDemo` program starts by defining collation rules for English and Spanish. The program will sort the Spanish words in the traditional manner. When sorting by the traditional rules, the letters ch and ll and their uppercase equivalents each have their own positions in the sort order. These character pairs compare as if they were one character. For example, ch sorts as a single letter, following cz in the sort order. Note how the rules for the two collators differ:

```

String englishRules = (
    "&lt; a,A &lt; b,B &lt; c,C &lt; d,D &lt; e,E &lt; f,F " +
    "&lt; g,G &lt; h,H &lt; i,I &lt; j,J &lt; k,K &lt; l,L " +
    "&lt; m,M &lt; n,N &lt; o,O &lt; p,P &lt; q,Q &lt; r,R " +
    "&lt; s,S &lt; t,T &lt; u,U &lt; v,V &lt; w,W &lt; x,X " +
    "&lt; y,Y &lt; z,Z");

String smallnTilde = new String("\u00F1");    // &#241;
String capitalNTilde = new String("\u00D1");  // &#209;

String traditionalSpanishRules = (
    "&lt; a,A &lt; b,B &lt; c,C " +
    "&lt; ch, cH, Ch, CH " +
    "&lt; d,D &lt; e,E &lt; f,F " +
    "&lt; g,G &lt; h,H &lt; i,I &lt; j,J &lt; k,K &lt; l,L " +
    "&lt; ll, lL, Ll, LL " +
    "&lt; m,M &lt; n,N " +
    "&lt; " + smallnTilde + "," + capitalNTilde + " " +
    "&lt; o,O &lt; p,P &lt; q,Q &lt; r,R " +
    "&lt; s,S &lt; t,T &lt; u,U &lt; v,V &lt; w,W &lt; x,X " +
    "&lt; y,Y &lt; z,Z");

```

The following lines of code create the collators and invoke the sort routine:

```

try {
    RuleBasedCollator enCollator = new RuleBasedCollator(englishRules);
    RuleBasedCollator spCollator =
        new RuleBasedCollator(traditionalSpanishRules);

    sortStrings(enCollator, words);
    printStrings(words);
    System.out.println();

    sortStrings(spCollator, words);
    printStrings(words);
} catch (ParseException pe) {
    System.out.println("Parse exception for rules");
}

```

The sort routine, called `sortStrings`, is generic. It will sort any array of words according to the rules of any `Collator` object:

```

public static void sortStrings(Collator collator, String[] words) {
    String tmp;
    for (int i = 0; i &lt; words.length; i++) {
        for (int j = i + 1; j &lt; words.length; j++) {
            if (collator.compare(words[i], words[j]) &gt; 0) {
                tmp = words[i];
                words[i] = words[j];
                words[j] = tmp;
            }
        }
    }
}

```

When sorted with the English collation rules, the array of words is as follows:

```

chalina
curioso
llama
luz

```

Compare the preceding list with the following, which is sorted according to the traditional Spanish rules of collation:

```

curioso
chalina
luz
llama

```
