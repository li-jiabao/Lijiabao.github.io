---
title: Handling Plurals
author: lijiabao
date: 2020-12-06 01:50:42.396554600 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Handling Plurals

The words in a message may vary if both plural and singular word forms are possible. With the `ChoiceFormat` class, you can map a number to a word or a phrase, allowing you to construct grammatically correct messages.

In English the plural and singular forms of a word are usually different. This can present a problem when you are constructing messages that refer to quantities. For example, if your message reports the number of files on a disk, the following variations are possible:

```

There are no files on XDisk.
There is one file on XDisk.
There are 2 files on XDisk.

```

The fastest way to solve this problem is to create a `MessageFormat` pattern like this:

```

There are {0,number} file(s) on {1}.

```

Unfortunately the preceding pattern results in incorrect grammar:

```

There are 1 file(s) on XDisk.

```

You can do better than that, provided that you use the 
[`ChoiceFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/ChoiceFormat.html) class. In this section you'll learn how to deal with plurals in a message by stepping through a sample program called 
[`ChoiceFormatDemo`](examples/ChoiceFormatDemo.java). This program also uses the `MessageFormat` class, which is discussed in the previous section, [Dealing with Compound Messages](messageFormat.html). <a name="step1" id="step1"></a>

## <a name="step1__1" id="step1__1">1. Define the Message Pattern</a>

First, identify the variables in the message:

Next, replace the variables in the message with arguments, creating a pattern that can be applied to a `MessageFormat` object:

```

There {0} on {1}.

```

The argument for the disk name, which is represented by`{1}`, is easy enough to deal with. You just treat it like any other `String` variable in a `MessageFormat` pattern. This argument matches the element at index 1 in the array of argument values. (See [step 7](#step7).)

Dealing with argument`{0}` is more complex, for a couple of reasons:

- The phrase that this argument replaces varies with the number of files. To construct this phrase at run time, you need to map the number of files to a particular `String`. For example, the number 1 will map to the `String` containing the phrase `is one file`. The `ChoiceFormat` class allows you to perform the necessary mapping.
- If the disk contains multiple files, the phrase includes an integer. The `MessageFormat` class lets you insert a number into a phrase.

## 2. Create a ResourceBundle

Because the message text must be translated, isolate it in a `ResourceBundle`:

```

ResourceBundle bundle = ResourceBundle.getBundle(
    "ChoiceBundle", currentLocale);

```

The sample program backs the `ResourceBundle` with properties files. The 
[`ChoiceBundle_en_US.properties`](examples/ChoiceBundle_en_US.properties)contains the following lines:

```

pattern = There {0} on {1}.
noFiles = are no files
oneFile = is one file
multipleFiles = are {2} files

```

The contents of this properties file show how the message will be constructed and formatted. The first line contains the pattern for `MessageFormat` . (See [step 1](#step1).) The other lines contain phrases that will replace argument `{0}` in the pattern. The phrase for the `multipleFiles` key contains the argument `{2}`, which will be replaced by a number.

Here is the French version of the properties file, 
[`ChoiceBundle_fr_FR.properties`](examples/ChoiceBundle_fr_FR.properties)

```

pattern = Il {0} sur {1}.
noFiles = n'y a pas de fichiers
oneFile = y a un fichier
multipleFiles = y a {2} fichiers

```

## 3. Create a Message Formatter

In this step you instantiate `MessageFormat` and set its `Locale`:

```

MessageFormat messageForm = new MessageFormat("");
messageForm.setLocale(currentLocale);

```

## 4. Create a Choice Formatter

The `ChoiceFormat` object allows you to choose, based on a `double` number, a particular `String`. The range of `double` numbers, and the `String` objects to which they map, are specified in arrays:

```

double[] fileLimits = {0,1,2};
String [] fileStrings = {
    bundle.getString("noFiles"),
    bundle.getString("oneFile"),
    bundle.getString("multipleFiles")
};

```

`ChoiceFormat` maps each element in the `double` array to the element in the `String` array that has the same index. In the sample code the 0 maps to the `String` returned by calling `bundle.getString("noFiles")`. By coincidence the index is the same as the value in the `fileLimits` array. If the code had set `fileLimits[0]` to seven, `ChoiceFormat` would map the number 7 to `fileStrings[0]`.

You specify the `double` and `String` arrays when instantiating `ChoiceFormat`:

```

ChoiceFormat choiceForm = new ChoiceFormat(fileLimits, fileStrings);

```

## 5. Apply the Pattern

Remember the pattern you constructed in step 1? It's time to retrieve the pattern from the `ResourceBundle` and apply it to the `MessageFormat` object:

```

String pattern = bundle.getString("pattern");
messageForm.applyPattern(pattern);

```

## 6. Assign the Formats

In this step you assign to the `MessageFormat` object the `ChoiceFormat` object created in step 4:

```

Format[] formats = {choiceForm, null, NumberFormat.getInstance()};
messageForm.setFormats(formats);

```

The `setFormats` method assigns `Format` objects to the arguments in the message pattern. You must invoke the `applyPattern` method before you call the `setFormats` method. The following table shows how the elements of the `Format` array correspond to the arguments in the message pattern:

## The `Format` Array of the `ChoiceFormatDemo` Program
<th id="h1">Array Element</th><th id="h2">Pattern Argument</th>
<td headers="h1">`choiceForm`</td><td headers="h2">`{0}`</td>
<td headers="h1">`null`</td><td headers="h2">`{1}`</td>
<td headers="h1">`NumberFormat.getInstance()`</td><td headers="h2">`{2}`</td>

## <a name="step7" id="step7">7. Set the Arguments and Format the Message</a>

At run time the program assigns the variables to the array of arguments it passes to the `MessageFormat` object. The elements in the array correspond to the arguments in the pattern. For example, `messageArgument[1]` maps to pattern argument `{1}`, which is a `String` containing the name of the disk. In the previous step the program assigned a `ChoiceFormat` object to argument `{0}` of the pattern. Therefore the number assigned to `messageArgument[0]` determines which `String` the `ChoiceFormat` object selects. If `messageArgument[0]` is greater than or equal to 2, the `String` containing the phrase `are {2} files` replaces argument `{0}` in the pattern. The number assigned to `messageArgument[2]` will be substituted in place of pattern argument `{2}`. Here's the code that tries this out:

```

Object[] messageArguments = {null, "XDisk", null};

for (int numFiles = 0; numFiles &lt; 4; numFiles++) {
    messageArguments[0] = new Integer(numFiles);
    messageArguments[2] = new Integer(numFiles);
    String result = messageForm.format(messageArguments);
    System.out.println(result);
}

```

## 8. Run the Demo Program

Compare the messages displayed by the program with the phrases in the `ResourceBundle` of step 2. Notice that the `ChoiceFormat` object selects the correct phrase, which the `MessageFormat` object uses to construct the proper message. The output of the `ChoiceFormatDemo` program is as follows:

```

currentLocale = en_US
There are no files on XDisk.
There is one file on XDisk.
There are 2 files on XDisk.
There are 3 files on XDisk.

currentLocale = fr_FR
Il n'y a pas des fichiers sur XDisk.
Il y a un fichier sur XDisk.
Il y a 2 fichiers sur XDisk.
Il y a 3 fichiers sur XDisk.

```
