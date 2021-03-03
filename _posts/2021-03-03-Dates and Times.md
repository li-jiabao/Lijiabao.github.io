---
title: Dates and Times
author: lijiabao
date: 2020-12-06 01:50:28.803931100 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Dates and Times

`Date` objects represent dates and times. You cannot display or print a `Date` object without first converting it to a `String` that is in the proper format. Just what is the "proper" format? First, the format should conform to the conventions of the end user's `Locale`. For example, Germans recognize `20.4.09` as a valid date, but Americans expect that same date to appear as `4/20/09`. Second, the format should include the necessary information. For instance, a program that measures network performance may report on elapsed milliseconds. An online appointment calendar probably won't display milliseconds, but it will show the days of the week.

This section explains how to format dates and times in various ways and in a locale-sensitive manner. If you follow these techniques your programs will display dates and times in the appropriate `Locale`, but your source code will remain independent of any specific `Locale`.

## [Using Predefined Formats](dateFormat.html)

The `DateFormat` class provides predefined formatting styles that are locale-specific and easy to use.

## [Customizing Formats](simpleDateFormat.html)

With the `SimpleDateFormat` class, you can create customized, locale-specific formats.

## [Changing Date Format Symbols](dateFormatSymbols.html)

Using the `DateFormatSymbols` class, you can change the symbols that represent the names of months, days of the week, and other formatting elements.
