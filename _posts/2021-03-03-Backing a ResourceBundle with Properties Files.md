---
title: Backing a ResourceBundle with Properties Files
author: lijiabao
date: 2020-12-06 01:50:10.080071700 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Backing a ResourceBundle with Properties Files

This section steps through a sample program named 
[`<code>PropertiesDemo`</code>](examples/PropertiesDemo.java).

## 1. Create the Default Properties File

A properties file is a simple text file. You can create and maintain a properties file with just about any text editor.

You should always create a default properties file. The name of this file begins with the base name of your `ResourceBundle` and ends with the `.properties` suffix. In the `PropertiesDemo` program the base name is `LabelsBundle`. Therefore the default properties file is called `LabelsBundle.properties`. This file contains the following lines:

```

# This is the default LabelsBundle.properties file
s1 = computer
s2 = disk
s3 = monitor
s4 = keyboard

```

Note that in the preceding file the comment lines begin with a pound sign (#). The other lines contain key-value pairs. The key is on the left side of the equal sign and the value is on the right. For instance, `s2` is the key that corresponds to the value `disk`. The key is arbitrary. We could have called `s2` something else, like `msg5` or `diskID`. Once defined, however, the key should not change because it is referenced in the source code. The values may be changed. In fact, when your localizers create new properties files to accommodate additional languages, they will translate the values into various languages.

## 2. Create Additional Properties Files as Needed

To support an additional `Locale`, your localizers will create a new properties file that contains the translated values. No changes to your source code are required, because your program references the keys, not the values.

For example, to add support for the German language, your localizers would translate the values in `LabelsBundle.properties` and place them in a file named `LabelsBundle_de.properties`. Notice that the name of this file, like that of the default file, begins with the base name `LabelsBundle` and ends with the `.properties` suffix. However, since this file is intended for a specific `Locale`, the base name is followed by the language code (`de`). The contents of `LabelsBundle_de.properties` are as follows:

```

# This is the LabelsBundle_de.properties file
s1 = Computer
s2 = Platte
s3 = Monitor
s4 = Tastatur

```

The `PropertiesDemo` sample program ships with three properties files:

```

LabelsBundle.properties
LabelsBundle_de.properties
LabelsBundle_fr.properties

```

## 3. Specify the Locale

The `PropertiesDemo` program creates the `Locale` objects as follows:

```

Locale[] supportedLocales = {
    Locale.FRENCH,
    Locale.GERMAN,
    Locale.ENGLISH
};

```

These `Locale` objects should match the properties files created in the previous two steps. For example, the `Locale.FRENCH` object corresponds to the `LabelsBundle_fr.properties` file. The `Locale.ENGLISH` has no matching `LabelsBundle_en.properties` file, so the default file will be used.

## 4. Create the ResourceBundle

This step shows how the `Locale`, the properties files, and the `ResourceBundle` are related. To create the `ResourceBundle`, invoke the `getBundle`method, specifying the base name and `Locale`:

```

ResourceBundle labels = ResourceBundle.getBundle("LabelsBundle", currentLocale);

```

The `getBundle` method first looks for a class file that matches the base name and the `Locale`. If it can't find a class file, it then checks for properties files. In the `PropertiesDemo` program we're backing the `ResourceBundle` with properties files instead of class files. When the `getBundle` method locates the correct properties file, it returns a `PropertyResourceBundle` object containing the key-value pairs from the properties file.

## 5. Fetch the Localized Text

To retrieve the translated value from the `ResourceBundle`, invoke the `getString` method as follows:

```

String value = labels.getString(key);

```

The `String` returned by `getString` corresponds to the key specified. The `String` is in the proper language, provided that a properties file exists for the specified `Locale`.

## 6. Iterate through All the Keys

This step is optional. When debugging your program, you might want to fetch values for all of the keys in a `ResourceBundle`. The `getKeys` method returns an `Enumeration` of all the keys in a `ResourceBundle`. You can iterate through the `Enumeration` and fetch each value with the `getString` method. The following lines of code, which are from the `PropertiesDemo` program, show how this is done:

```

ResourceBundle labels = ResourceBundle.getBundle("LabelsBundle", currentLocale);
Enumeration bundleKeys = labels.getKeys();

while (bundleKeys.hasMoreElements()) {
    String key = (String)bundleKeys.nextElement();
    String value = labels.getString(key);
    System.out.println("key = " + key + ", " + "value = " + value);
}

```

## 7. Run the Demo Program

Running the `PropertiesDemo` program generates the following output. The first three lines show the values returned by `getString` for various `Locale` objects. The program displays the last four lines when iterating through the keys with the `getKeys` method.

```

Locale = fr, key = s2, value = Disque dur
Locale = de, key = s2, value = Platte
Locale = en, key = s2, value = disk

key = s4, value = Clavier
key = s3, value = Moniteur
key = s2, value = Disque dur
key = s1, value = Ordinateur

```
