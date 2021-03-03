---
title: Isolating Locale-Specific Data
author: lijiabao
date: 2020-12-06 01:50:04.108116100 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Lesson: Isolating Locale-Specific Data

Locale-specific data must be tailored according to the conventions of the end user's language and region. The text displayed by a user interface is the most obvious example of locale-specific data. For example, an application with a Cancel button in the U.S. will have an Abbrechen button in Germany. In other countries this button will have other labels. Obviously you don't want to hardcode this button label. Wouldn't it be nice if you could automatically get the correct label for a given `Locale`? Fortunately you can, provided that you isolate the locale-specific objects in a `ResourceBundle`.

In this lesson you'll learn how to create and access `ResourceBundle` objects. If you're in a hurry to examine some coding examples, go ahead and check out the last two sections in this lesson. Then you can come back to the first two sections to get some conceptual information about `ResourceBundle` objects.

## [About the ResourceBundle Class](concept.html)

`ResourceBundle` objects contain locale-specific objects. When you need a locale-specific object, you fetch it from a `ResourceBundle`, which returns the object that matches the end user's `Locale`. This section explains how a `ResourceBundle` is related to a `Locale`, and describes the `ResourceBundle` subclasses.

## [Preparing to Use a ResourceBundle](prepare.html)

Before you create your `ResourceBundle` objects, you should do a little planning. First, identify the locale-specific objects in your program. Then organize them into categories and store them in different `ResourceBundle` objects accordingly.

## [Backing a ResourceBundle with Properties Files](propfile.html)

If your application contains `String` objects that need to be translated into various languages, you can store these `String` objects in a `PropertyResourceBundle`, which is backed up by a set of properties files. Since the properties files are simple text files, they can be created and maintained by your translators. You don't have to change the source code. In this section you'll learn how to set up the properties files that back up a `PropertyResourceBundle`.

## [Using a ListResourceBundle](list.html)

The `ListResourceBundle` class, which is a subclass of `ResourceBundle`, manages locale-specific objects with a list. A `ListResourceBundle` is backed by a class file, which means that you must code and compile a new source file each time support for an additional `Locale` is needed. However, `ListResourceBundle` objects are useful because unlike properties files, they can store any type of locale-specific object. By stepping through a sample program, this section demonstrates how to use a `ListResourceBundle`.

## [Customizing Resource Bundle Loading](control.html)

This section represents new capabilities to improve the `ResourceBundle.getBundle` factory flexibility. The `ResourceBundle.Control` class collaborates with the factory methods for loading resource bundles. This allows to consider every substantial step of the resource bundle-loading process and its cache control as a separate method.
