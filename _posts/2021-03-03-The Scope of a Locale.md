---
title: The Scope of a Locale
author: lijiabao
date: 2020-12-06 01:49:59.791429400 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# The Scope of a Locale

The Java platform does not require you to use the same `Locale` throughout your program. If you wish, you can assign a different `Locale` to every locale-sensitive object in your program. This flexibility allows you to develop multilingual applications, which can display information in multiple languages.

However, most applications are not multi-lingual and their locale-sensitive objects rely on the default `Locale`. Set by the Java Virtual Machine when it starts up, the default `Locale` corresponds to the locale of the host platform. To determine the default `Locale` of your Java Virtual Machine, invoke the `Locale.getDefault` method.

It is possible to independently set the default locale for two types of uses: the **format** setting is used for formatting resources, and the **display** setting is used in menus and dialogs. Introduced in the Java SE 7 release, the 
[`Locale.getDefault(Locale.Category)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getDefault-java.util.Locale.Category-) method takes a 
[`Locale.Category`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.Category.html) parameter. Passing the `FORMAT` enum to the `getDefault(Locale.Category)` method returns the default locale for formatting resources. Similarly, passing the `DISPLAY` enum returns the default locale used by the UI. The corresponding 
[`setDefault(Locale.Category, Locale)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#setDefault-java.util.Locale.Category-java.util.Locale-) method allows setting the locale for the desired category. The no-argument `getDefault` method returns the `DISPLAY` default value.

On the Windows platform, these default values are initialized according to the "Standards and Formats" and "Display Language" settings in the Windows control panel.

You should not set the default `Locale` programmatically because it is shared by all locale-sensitive classes.

Distributed computing raises some interesting issues. For example, suppose you are designing an application server that will receive requests from clients in various countries. If the `Locale` for each client is different, what should be the `Locale` of the server? Perhaps the server is multithreaded, with each thread set to the `Locale` of the client it services. Or perhaps all data passed between the server and the clients should be locale-independent.

Which design approach should you take? If possible, the data passed between the server and the clients should be locale-independent. This simplifies the design of the server by making the clients responsible for displaying the data in a locale-sensitive manner. However, this approach won't work if the server must store the data in a locale-specific form. For example, the server might store Spanish, English, and French versions of the same data in different database columns. In this case, the server might want to query the client for its `Locale`, since the `Locale` may have changed since the last request.
