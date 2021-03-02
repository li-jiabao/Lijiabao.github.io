
# Dealing with Compound Messages

A compound message may contain several kinds of variables: dates, times, strings, numbers, currencies, and percentages. To format a compound message in a locale-independent manner, you construct a pattern that you apply to a `MessageFormat` object, and store this pattern in a `ResourceBundle`.

By stepping through a sample program, this section demonstrates how to internationalize a compound message. The sample program makes use of the 
[`MessageFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/MessageFormat.html) class. The full source code for this program is in the file called 
[`MessageFormatDemo.java`](examples/MessageFormatDemo.java). The German locale properties are in the file called 
[`MessageBundle_de_DE.properties`](examples/MessageBundle_de_DE.properties).

## 1. Identify the Variables in the Message

Suppose that you want to internationalize the following message:

Notice that we've underlined the variable data and have identified what kind of objects will represent this data.

## 2. Isolate the Message Pattern in a ResourceBundle

Store the message in a `ResourceBundle` named `MessageBundle`, as follows:

```

ResourceBundle messages =
   ResourceBundle.getBundle("MessageBundle", currentLocale);

```

This `ResourceBundle` is backed by a properties file for each `Locale`. Since the `ResourceBundle` is called `MessageBundle`, the properties file for U.S. English is named `MessageBundle_en_US.properties`. The contents of this file is as follows:

```

template = At {2,time,short} on {2,date,long}, \
    we detected {1,number,integer} spaceships on \
    the planet {0}.
planet = Mars

```

The first line of the properties file contains the message pattern. If you compare this pattern with the message text shown in step 1, you'll see that an argument enclosed in braces replaces each variable in the message text. Each argument starts with a digit called the argument number, which matches the index of an element in an `Object` array that holds the argument values. Note that in the pattern the argument numbers are not in any particular order. You can place the arguments anywhere in the pattern. The only requirement is that the argument number have a matching element in the array of argument values.

The next step discusses the argument value array, but first let's look at each of the arguments in the pattern. The following table provides some details about the arguments:
<th id="h1">Argument</th><th id="h2">Description</th>
<td headers="h1">`{2,time,short}`</td><td headers="h2">The time portion of a `Date` object. The `short` style specifies the `DateFormat.SHORT` formatting style.</td>
<td headers="h1">`{2,date,long}`</td><td headers="h2">The date portion of a `Date` object. The same `Date` object is used for both the date and time variables. In the `Object` array of arguments the index of the element holding the `Date` object is 2. (This is described in the next step.)</td>
<td headers="h1">`{1,number,integer}`</td><td headers="h2">A `Number` object, further qualified with the `integer` number style.</td>
<td headers="h1">`{0}`</td><td headers="h2">The `String` in the `ResourceBundle` that corresponds to the `planet` key.</td>

For a full description of the argument syntax, see the API documentation for the 
[`MessageFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/MessageFormat.html) class.

## 3. Set the Message Arguments

The following lines of code assign values to each argument in the pattern. The indexes of the elements in the `messageArguments` array match the argument numbers in the pattern. For example, the `Integer` element at index 1 corresponds to the `{1,number,integer}` argument in the pattern. Because it must be translated, the `String` object at element 0 will be fetched from the `ResourceBundle` with the `getString` method. Here is the code that defines the array of message arguments:

```

Object[] messageArguments = {
    messages.getString("planet"),
    new Integer(7),
    new Date()
};

```

## 4. Create the Formatter

Next, create a `MessageFormat` object. You set the `Locale` because the message contains `Date` and `Number` objects, which should be formatted in a locale-sensitive manner.

```

MessageFormat formatter = new MessageFormat("");
formatter.setLocale(currentLocale);

```

## 5. Format the Message Using the Pattern and the Arguments

This step shows how the pattern, message arguments, and formatter all work together. First, fetch the pattern `String` from the `ResourceBundle` with the `getString` method. The key to the pattern is `template`. Pass the pattern `String` to the formatter with the `applyPattern` method. Then format the message using the array of message arguments, by invoking the `format` method. The `String` returned by the `format` method is ready to be displayed. All of this is accomplished with just two lines of code:

```

formatter.applyPattern(messages.getString("template"));
String output = formatter.format(messageArguments);

```

## 6. Run the Demo Program

The demo program prints the translated messages for the English and German locales and properly formats the date and time variables. Note that the English and German verbs ("detected" and "entdeckt") are in different locations relative to the variables:

```

currentLocale = en_US
At 10:16 AM on July 31, 2009, we detected 7
spaceships on the planet Mars.
currentLocale = de_DE
Um 10:16 am 31. Juli 2009 haben wir 7 Raumschiffe
auf dem Planeten Mars entdeckt.

```
