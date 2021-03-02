
# Performing Locale-Independent Comparisons

Collation rules define the sort sequence of strings. These rules vary with locale, because various natural languages sort words differently. You can use the predefined collation rules provided by the `Collator` class to sort strings in a locale-independent manner.

To instantiate the `Collator` class invoke the `getInstance` method. Usually, you create a `Collator` for the default `Locale`, as in the following example:

```

Collator myDefaultCollator = Collator.getInstance();

```

You can also specify a particular `Locale` when you create a `Collator`, as follows:

```

Collator myFrenchCollator = Collator.getInstance(Locale.FRENCH);

```

The `getInstance` method returns a `RuleBasedCollator`, which is a concrete subclass of `Collator`. The `RuleBasedCollator` contains a set of rules that determine the sort order of strings for the locale you specify. These rules are predefined for each locale. Because the rules are encapsulated within the `RuleBasedCollator`, your program won't need special routines to deal with the way collation rules vary with language.

You invoke the `Collator.compare` method to perform a locale-independent string comparison. The `compare` method returns an integer less than, equal to, or greater than zero when the first string argument is less than, equal to, or greater than the second string argument. The following table contains some sample calls to `Collator.compare`:
<th id="h1">Example</th><th id="h2">Return Value</th><th id="h3">Explanation</th>
<td headers="h1">`myCollator.compare("abc", "def")`</td><td headers="h2">`-1`</td><td headers="h3">`"abc"` is less than "def"</td>
<td headers="h1">`myCollator.compare("rtf", "rtf")`</td><td headers="h2">`0`</td><td headers="h3">the two strings are equal</td>
<td headers="h1">`myCollator.compare("xyz", "abc")`</td><td headers="h2">`1`</td><td headers="h3">"xyz" is greater than "abc"</td>

You use the `compare` method when performing sort operations. The sample program called 
[`CollatorDemo`](examples/CollatorDemo.java) uses the `compare` method to sort an array of English and French words. This program shows what can happen when you sort the same list of words with two different collators:

```

Collator fr_FRCollator = Collator.getInstance(new Locale("fr","FR"));
Collator en_USCollator = Collator.getInstance(new Locale("en","US"));

```

The method for sorting, called `sortStrings`, can be used with any `Collator`. Notice that the `sortStrings` method invokes the `compare` method:

```

public static void sortStrings(Collator collator, String[] words) {
    String tmp;
    for (int i = 0; i &lt; words.length; i++) {
        for (int j = i + 1; j &lt; words.length; j++) { 
            if (collator.compare(words[i], words[j]) &gt; 0) {
                tmp = words[i];
                words[i] = words[j];
                words[j] = tmp;
            }
        }
    }
}

```

The English `Collator` sorts the words as follows:

```

peach
p&#233;ch&#233;
p&#234;che
sin

```

According to the collation rules of the French language, the preceding list is in the wrong order. In French p&#233;ch&#233; should follow p&#234;che in a sorted list. The French `Collator` sorts the array of words correctly, as follows:

```

peach
p&#234;che
p&#233;ch&#233;
sin

```
