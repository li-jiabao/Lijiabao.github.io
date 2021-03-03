---
title: About the ResourceBundle Class
author: lijiabao
date: 2020-12-06 01:50:06.263753500 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# About the ResourceBundle Class

## How a ResourceBundle is Related to a Locale

Conceptually each `ResourceBundle` is a set of related subclasses that share the same base name. The list that follows shows a set of related subclasses. `ButtonLabel` is the base name. The characters following the base name indicate the language code, country code, and variant of a `Locale`. `ButtonLabel_en_GB`, for example, matches the `Locale` specified by the language code for English (`en`) and the country code for Great Britain (`GB`).

```

ButtonLabel
ButtonLabel_de
ButtonLabel_en_GB
ButtonLabel_fr_CA_UNIX

```

To select the appropriate `ResourceBundle`, invoke the `ResourceBundle.getBundle` method. The following example selects the `ButtonLabel` `ResourceBundle` for the `Locale` that matches the French language, the country of Canada, and the UNIX platform.

```

Locale currentLocale = new Locale("fr", "CA", "UNIX");
ResourceBundle introLabels = ResourceBundle.getBundle(
                                 "ButtonLabel", currentLocale);

```

If a `ResourceBundle` class for the specified `Locale` does not exist, `getBundle` tries to find the closest match. For example, if `ButtonLabel_fr_CA_UNIX` is the desired class and the default `Locale` is `en_US`, `getBundle` will look for classes in the following order:

```

ButtonLabel_fr_CA_UNIX
ButtonLabel_fr_CA
ButtonLabel_fr
ButtonLabel_en_US
ButtonLabel_en
ButtonLabel

```

Note that `getBundle` looks for classes based on the default `Locale` before it selects the base class (`ButtonLabel)`. If `getBundle` fails to find a match in the preceding list of classes, it throws a `MissingResourceException`. To avoid throwing this exception, you should always provide a base class with no suffixes.

## The ListResourceBundle and PropertyResourceBundle Subclasses

The abstract class `ResourceBundle` has two subclasses: `PropertyResourceBundle` and `ListResourceBundle`.

A `PropertyResourceBundle` is backed by a properties file. A properties file is a plain-text file that contains translatable text. Properties files are not part of the Java source code, and they can contain values for `String` objects only. If you need to store other types of objects, use a `ListResourceBundle` instead. The section [Backing a ResourceBundle with Properties Files](propfile.html) shows you how to use a `PropertyResourceBundle`.

The `ListResourceBundle` class manages resources with a convenient list. Each `ListResourceBundle` is backed by a class file. You can store any locale-specific object in a `ListResourceBundle`. To add support for an additional `Locale`, you create another source file and compile it into a class file. The section [Using a ListResource Bundle](list.html) has a coding example you may find helpful.

The `ResourceBundle` class is flexible. If you first put your locale-specific `String` objects in a `PropertyResourceBundle` and then later decided to use `ListResourceBundle` instead, there is no impact on your code. For example, the following call to `getBundle` will retrieve a `ResourceBundle` for the appropriate `Locale`, whether `ButtonLabel` is backed up by a class or by a properties file:

```

ResourceBundle introLabels = ResourceBundle.getBundle(
                                 "ButtonLabel", currentLocale);

```

## Key-Value Pairs

`ResourceBundle` objects contain an array of key-value pairs. You specify the key, which must be a `String`, when you want to retrieve the value from the `ResourceBundle`. The value is the locale-specific object. The keys in the following example are the `OkKey` and `CancelKey` strings:

```

class ButtonLabel_en extends ListResourceBundle {
    // English version
    public Object[][] getContents() {
        return contents;
    }
    static final Object[][] contents = {
        {"OkKey", "OK"},
        {"CancelKey", "Cancel"},
    };
}

```

To retrieve the `OK` `String` from the `ResourceBundle`, you would specify the appropriate key when invoking `getString`:

```

String okLabel = ButtonLabel.getString("OkKey");

```

A properties file contains key-value pairs. The key is on the left side of the equal sign, and the value is on the right. Each pair is on a separate line. The values may represent `String` objects only. The following example shows the contents of a properties file named `ButtonLabel.properties`:

```

OkKey = OK
CancelKey = Cancel

```
