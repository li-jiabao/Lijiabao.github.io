---
title: Preparing to Use a ResourceBundle
author: lijiabao
date: 2020-12-06 01:50:08.043665800 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Preparing to Use a ResourceBundle

## Identifying the Locale-Specific Objects

If your application has a user interface, it contains many locale-specific objects. To get started, you should go through your source code and look for objects that vary with `Locale`. Your list might include objects instantiated from the following classes:

- `String`
- `Image`
- `Color`
- `AudioClip`

You'll notice that this list doesn't contain objects representing numbers, dates, times, or currencies. The display format of these objects varies with `Locale`, but the objects themselves do not. For example, you format a `Date` according to `Locale`, but you use the same `Date` object regardless of `Locale`. Instead of isolating these objects in a `ResourceBundle`, you format them with special locale-sensitive formatting classes. You'll learn how to do this in the
[Dates and Times](../format/dateintro.html) section of the 
[Formatting](../format/index.html) lesson.

In general, the objects stored in a `ResourceBundle` are predefined and ship with the product. These objects are not modified while the program is running. For instance, you should store a `Menu` label in a `ResourceBundle` because it is locale-specific and will not change during the program session. However, you should not isolate in a `ResourceBundle` a `String` object the end user enters in a `TextField`. Data such as this `String` may vary from day to day. It is specific to the program session, not to the `Locale` in which the program runs.

Usually most of the objects you need to isolate in a `ResourceBundle` are `String` objects. However, not all `String` objects are locale-specific. For example, if a `String` is a protocol element used by interprocess communication, it doesn't need to be localized, because the end users never see it.

The decision whether to localize some `String` objects is not always clear. Log files are a good example. If a log file is written by one program and read by another, both programs are using the log file as a buffer for communication. Suppose that end users occasionally check the contents of this log file. Shouldn't the log file be localized? On the other hand, if end users rarely check the log file, the cost of translation may not be worthwhile. Your decision to localize this log file depends on a number of factors: program design, ease of use, cost of translation, and supportability.

## Organizing ResourceBundle Objects

You can organize your `ResourceBundle` objects according to the category of objects they contain. For example, you might want to load all of the GUI labels for an order entry window into a `ResourceBundle` called `OrderLabelsBundle`. Using multiple `ResourceBundle` objects offers several advantages:

- Your code is easier to read and to maintain.
- You'll avoid huge `ResourceBundle` objects, which may take too long to load into memory.
- You can reduce memory usage by loading each `ResourceBundle` only when needed.
