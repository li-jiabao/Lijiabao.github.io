---
title: BCP 47 Extensions
author: lijiabao
date: 2020-12-06 01:49:53.307740300 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# BCP 47 Extensions

The Java SE 7 release conforms to the IETF BCP 47 standard, which supports adding extensions to a `Locale`. Any single character can be used to denote an extension, but there are two predefined extension codes: `'u'` specifies a **Unicode locale extension**, and `'x'` specifies a **private use extension**.

Unicode locale extensions are defined by the Unicode [Common Locale Data Repository (CLDR)](http://cldr.unicode.org/) project. They are used to specify information that is non-language-specific such as calendars or currency. A private use extension may be used to specify any other information, such as platform (for example, Windows, UNIX, or Linux), or release information (for example, 6u23 or JDK 7).

An extension is specified as a key/value pair, where the key is a single character (typically `'u'` or `'x'`). A well-formed value has the following format:

```

SUBTAG ('-' SUBTAG)*

```

In this format:

```

SUBTAG = [0-9a-zA-Z]{2,8}    **For key='u'**
SUBTAG = [0-9a-zA-Z]{1,8}    **For key='x'**

```

Note that a single-character value is allowed for the private use extension. However, there is a 2-character minimum for values in the Unicode locale extension.

Extension strings are case-insensitive, but the `Locale` class maps all keys and values to lowercase.

The 
[`getExtensionKeys()`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getExtensionKeys--) method returns the set of extension keys, if any, for a `Locale`. The 
[`getExtension(key)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getExtension-char-) method returns the value string for the specified key, if any.

## <a name="unicode" id="unicode">Unicode Locale Extensions</a>

As previously mentioned, a Unicode locale extension is specified by the `'u'` key code or the `UNICODE_LOCALE_EXTENSION` constant. The value itself is also specified by a key/type pair. Legal values are defined in the [Key/Type Definitions](http://www.unicode.org/reports/tr35/#Key_Type_Definitions) table on the [Unicode](http://www.unicode.org) website. A key code is specified by two alphabetic characters. The following table lists the Unicode locale extension keys:
<th id="h1">Key Code</th><th id="h2">Description</th>
<td headers="h1">ca</td><td headers="h2">calendar algorithm</td>
<td headers="h1">co</td><td headers="h2">collation type</td>
<td headers="h1">ka</td><td headers="h2">collation parameters</td>
<td headers="h1">cu</td><td headers="h2">currency type</td>
<td headers="h1">nu</td><td headers="h2">number type</td>
<td headers="h1">va</td><td headers="h2">common variant type</td>

Specifying a Unicode locale extension, such as number format, does not guarantee that the locale services for the underlying platform will honor that request.

The following table shows some examples of key/type pairs for a Unicode locale extension.
<th id="h101">Key/Type pair</th><th id="h102">Description</th>
<td headers="h101">ca-buddhist</td><td headers="h102">Thai Buddhist calendar</td>
<td headers="h101">co-pinyin</td><td headers="h102">Pinyin ordering for Latin</td>
<td headers="h101">cu-usd</td><td headers="h102">U.S. dollars</td>
<td headers="h101">nu-jpanfin</td><td headers="h102">Japanese financial numerals</td>
<td headers="h101">tz-aldav</td><td headers="h102">Europe/Andorra</td>

The following string represents the German language locale for the country of Germany using a phonebook style of ordering for the Linux platform. This example also contains an attribute named `"email"`.

```

de-DE-u-email-co-phonebk-x-linux

```

The following `Locale` methods can be used to access information about the Unicode locale extensionss. These methods are described using the previous German locale example.

<li>
[`getUnicodeLocaleKeys()`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getUnicodeLocaleKeys--) &#150; Returns the Unicode locale key codes or an empty set if the locale has none. For the German example, this would return a set containing the single string `"co"`.</li>
<li>
[`getUnicodeLocaleType(String)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getUnicodeLocaleType-java.lang.String-) &#150; Returns the Unicode locale type associated with the Unicode locale key code. Invoking `getUnicodeLocaleType("co")` for the German example would return the string `"phonebk"`.</li>
<li>
[`getUnicodeLocaleAttributes()`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getUnicodeLocaleAttributes--) &#150; Returns the set of Unicode locale attributes associated with this locale, if any. In the German example, this would return a set containing the single string `"email"`.</li>

## <a name="private" id="private">Private Use Extensions</a>

The private use extension, specified by the `'x'` key code or the `PRIVATE_USE_EXTENSION` constant, can be anything, as long as the value is well formed.

The following are examples of possible private use extensions:

```

x-jdk-1-7
x-linux

```
