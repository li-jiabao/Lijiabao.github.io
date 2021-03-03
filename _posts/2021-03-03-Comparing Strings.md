---
title: Comparing Strings
author: lijiabao
date: 2020-12-06 01:50:48.517753600 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Comparing Strings

Applications that sort through text perform frequent string comparisons. For example, a report generator performs string comparisons when sorting a list of strings in alphabetical order.

If your application audience is limited to people who speak English, you can probably perform string comparisons with the `String.compareTo` method. The `String.compareTo` method performs a binary comparison of the Unicode characters within the two strings. For most languages, however, this binary comparison cannot be relied on to sort strings, because the Unicode values do not correspond to the relative order of the characters.

Fortunately the 
[`Collator`](https://docs.oracle.com/javase/8/docs/api/java/text/Collator.html) class allows your application to perform string comparisons for different languages. In this section, you'll learn how to use the `Collator` class when sorting text.

## [Performing Locale-Independent Comparisons](locale.html)

Collation rules define the sort sequence of strings. These rules vary with locale, because various natural languages sort words differently. Using the predefined collation rules provided by the `Collator` class, you can sort strings in a locale-independent manner.

## [Customizing Collation Rules](rule.html)

In some cases, the predefined collation rules provided by the `Collator` class may not work for you. For example, you may want to sort strings in a language whose locale is not supported by `Collator`. In this situation, you can define your own collation rules, and assign them to a `RuleBasedCollator` object.

## [Improving Collation Performance](perform.html)

With the `CollationKey` class, you may increase the efficiency of string comparisons. This class converts `String` objects to sort keys that follow the rules of a given `Collator`.
