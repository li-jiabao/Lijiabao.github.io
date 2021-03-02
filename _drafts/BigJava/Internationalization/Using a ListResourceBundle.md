
# Using a ListResourceBundle

This section illustrates the use of a `ListResourceBundle` object with a sample program called 
[`<code>ListDemo`</code>](examples/ListDemo.java). The text that follows explains each step involved in creating the `ListDemo` program, along with the `ListResourceBundle` subclasses that support it.

## 1. Create the ListResourceBundle Subclasses

A `ListResourceBundle` is backed up by a class file. Therefore the first step is to create a class file for every supported `Locale`. In the `ListDemo` program the base name of the `ListResourceBundle` is `StatsBundle`. Since `ListDemo` supports three `Locale` objects, it requires the following three class files:

```

StatsBundle_en_CA.class
StatsBundle_fr_FR.class
StatsBundle_ja_JP.class

```

The `StatsBundle` class for Japan is defined in the source code that follows. Note that the class name is constructed by appending the language and country codes to the base name of the `ListResourceBundle`. Inside the class the two-dimensional `contents` array is initialized with the key-value pairs. The keys are the first element in each pair: `GDP`, `Population`, and `Literacy`. The keys must be `String` objects and they must be the same in every class in the `StatsBundle` set. The values can be any type of object. In this example the values are two `Integer` objects and a `Double` object.

```

import java.util.*;
public class StatsBundle_ja_JP extends ListResourceBundle {
    public Object[][] getContents() {
        return contents;
    }

    private Object[][] contents = {
        { "GDP", new Integer(21300) },
        { "Population", new Integer(125449703) },
        { "Literacy", new Double(0.99) },
    };
}

```

## 2. Specify the Locale

The `ListDemo` program defines the `Locale` objects as follows:

```

Locale[] supportedLocales = {
    new Locale("en", "CA"),
    new Locale("ja", "JP"),
    new Locale("fr", "FR")
};

```

Each `Locale` object corresponds to one of the `StatsBundle` classes. For example, the Japanese `Locale`, which was defined with the `ja` and `JP` codes, matches `StatsBundle_ja_JP.class`.

## 3. Create the ResourceBundle

To create the `ListResourceBundle`, invoke the `getBundle` method. The following line of code specifies the base name of the class (`StatsBundle`) and the `Locale`:

```

ResourceBundle stats = ResourceBundle.getBundle("StatsBundle", currentLocale);

```

The `getBundle` method searches for a class whose name begins with `StatsBundle` and is followed by the language and country codes of the specified `Locale`. If the `currentLocale` is created with the `ja` and `JP` codes, `getBundle` returns a `ListResourceBundle` corresponding to the class `StatsBundle_ja_JP`, for example.

## 4. Fetch the Localized Objects

Now that the program has a `ListResourceBundle` for the appropriate `Locale`, it can fetch the localized objects by their keys. The following line of code retrieves the literacy rate by invoking `getObject` with the `Literacy` key parameter. Since `getObject` returns an object, cast it to a `Double`:

```

Double lit = (Double)stats.getObject("Literacy");

```

## 5. Run the Demo Program

`ListDemo` program prints the data it fetched with the `getBundle` method:

```

Locale = en_CA
GDP = 24400
Population = 28802671
Literacy = 0.97

Locale = ja_JP
GDP = 21300
Population = 125449703
Literacy = 0.99

Locale = fr_FR
GDP = 20200
Population = 58317450
Literacy = 0.99

```
