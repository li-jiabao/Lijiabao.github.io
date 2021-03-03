---
title: Detecting Text Boundaries
author: lijiabao
date: 2020-12-06 01:51:09.673571800 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Detecting Text Boundaries

Applications that manipulate text need to locate boundaries within the text. For example, consider some of the common functions of a word processor: highlighting a character, cutting a word, moving the cursor to the next sentence, and wrapping a word at a line ending. To perform each of these functions, the word processor must be able to detect the logical boundaries in the text. Fortunately you don't have to write your own routines to perform boundary analysis. Instead, you can take advantage of the methods provided by the 
[`BreakIterator` ](https://docs.oracle.com/javase/8/docs/api/java/text/BreakIterator.html) class.

## [About the BreakIterator Class](about.html)

This section discusses the instantiation methods and the imaginary cursor of the `BreakIterator` class.

## [Character Boundaries](char.html)

In this section you'll learn about the difference between user and Unicode characters, and how to locate user characters with a `BreakIterator`.

## [Word Boundaries](word.html)

If your application needs to select or locate words within text, you'll find it helpful to use a `BreakIterator`.

## [Sentence Boundaries](sentence.html)

Determining sentence boundaries can be problematic, because of the ambiguous use of sentence terminators in many written languages. This section examines some of the problems you may encounter, and how the `BreakIterator` deals with them.

## [Line Boundaries](line.html)

This section describes how to locate potential line breaks in a text string with a `BreakIterator`.
