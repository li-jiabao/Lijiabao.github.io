---
title: Internationalizing the Sample Program
author: lijiabao
date: 2020-12-06 01:49:45.267511300 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Internationalizing the Sample Program

If you look at the internationalized source code, you'll notice that the hardcoded English messages have been removed. Because the messages are no longer hardcoded and because the language code is specified at run time, the same executable can be distributed worldwide. No recompilation is required for localization. The program has been internationalized.

You may be wondering what happened to the text of the messages or what the language and country codes mean. Don't worry. You'll learn about these concepts as you step through the process of internationalizing the sample program.

## 1. Create the Properties Files

A properties file stores information about the characteristics of a program or environment. A properties file is in plain-text format. You can create the file with just about any text editor.

In the example the properties files store the translatable text of the messages to be displayed. Before the program was internationalized, the English version of this text was hardcoded in the `System.out.println` statements. The default properties file, which is called `MessagesBundle.properties`, contains the following lines:

```

greetings = Hello
farewell = Goodbye
inquiry = How are you?

```

Now that the messages are in a properties file, they can be translated into various languages. No changes to the source code are required. The French translator has created a properties file called `MessagesBundle_fr_FR.properties`, which contains these lines:

```

greetings = Bonjour.
farewell = Au revoir.
inquiry = Comment allez-vous?

```

Notice that the values to the right side of the equal sign have been translated but that the keys on the left side have not been changed. These keys must not change, because they will be referenced when your program fetches the translated text.

The name of the properties file is important. For example, the name of the `MessagesBundle_fr_FR.properties` file contains the `fr` language code and the `FR` country code. These codes are also used when creating a `Locale` object.

## 2. Define the Locale

The `Locale` object identifies a particular language and country. The following statement defines a `Locale` for which the language is English and the country is the United States:

```

aLocale = new Locale("en","US");

```

The next example creates `Locale` objects for the French language in Canada and in France:

```

caLocale = new Locale("fr","CA");
frLocale = new Locale("fr","FR");

```

The program is flexible. Instead of using hardcoded language and country codes, the program gets them from the command line at run time:

```

String language = new String(args[0]);
String country = new String(args[1]);
currentLocale = new Locale(language, country);

```

`Locale` objects are only identifiers. After defining a `Locale`, you pass it to other objects that perform useful tasks, such as formatting dates and numbers. These objects are **locale-sensitive** because their behavior varies according to `Locale`. A `ResourceBundle` is an example of a locale-sensitive object.

## 3. Create a ResourceBundle

`ResourceBundle` objects contain locale-specific objects. You use `ResourceBundle` objects to isolate locale-sensitive data, such as translatable text. In the sample program the `ResourceBundle` is backed by the properties files that contain the message text we want to display.

The `ResourceBundle` is created as follows:

```

messages = ResourceBundle.getBundle("MessagesBundle", currentLocale);

```

The arguments passed to the `getBundle` method identify which properties file will be accessed. The first argument, `MessagesBundle`, refers to this family of properties files:

```

MessagesBundle_en_US.properties
MessagesBundle_fr_FR.properties
MessagesBundle_de_DE.properties

```

The `Locale`, which is the second argument of `getBundle`, specifies which of the `MessagesBundle` files is chosen. When the `Locale` was created, the language code and the country code were passed to its constructor. Note that the language and country codes follow `MessagesBundle` in the names of the properties files.

Now all you have to do is get the translated messages from the `ResourceBundle`.

## 4. Fetch the Text from the ResourceBundle

The properties files contain key-value pairs. The values consist of the translated text that the program will display. You specify the keys when fetching the translated messages from the `ResourceBundle` with the `getString` method. For example, to retrieve the message identified by the greetings key, you invoke `getString` as follows:

```

String msg1 = messages.getString("greetings");

```

The sample program uses the key `greetings` because it reflects the content of the message, but it could have used another `String`, such as `s1` or `msg1`. Just remember that the key is hardcoded in the program and it must be present in the properties files. If your translators accidentally modify the keys in the properties files, `getString` won't be able to find the messages.

## Conclusion

That's it. As you can see, internationalizing a program isn't too difficult. It requires some planning and a little extra coding, but the benefits are enormous. To provide you with an overview of the internationalization process, the sample program in this lesson was intentionally kept simple. As you read the lessons that follow, you'll learn about the more advanced internationalization features of the Java programming language.
