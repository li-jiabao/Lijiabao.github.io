---
title: Language Tag Filtering and Lookup
author: lijiabao
date: 2020-12-06 01:49:57.477438400 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Language Tag Filtering and Lookup


The Java Programming language contains internationalization support for language tags, language tag filtering, and language tag lookup. These features are specified by 


[IETF BCP 47](http://tools.ietf.org/html/bcp47)
, which contains  


[RFC 5646 "Tags for Identifying Languages"](http://tools.ietf.org/html/rfc5646)
and 

[RFC 4647 "Matching of Language Tags."](http://tools.ietf.org/html/rfc4647)
This lesson describes how this support is provided in the JDK. 


## <a name="what-tags" id="what-tags">What Are Language Tags?</a>


Language tags are specially formatted strings that provide information about a particular language. A language tag might be something simple (such as "en" for English), something complex (such as "zh-cmn-Hans-CN" for Chinese, Mandarin, Simplified script, as used in China), or something in between (such as "sr-Latn", for Serbian written using Latin script). Language tags consist of "subtags" separated by hyphens; this terminology is used throughout the API documentation.



The 


[`java.util.Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html) class provides support for language tags. A `Locale` contains several different fields: language (such as "en" for English, or "ja" for Japanese), script (such as "Latn" for Latin or "Cyrl" for Cyrillic), country (such as "US" for United States or "FR" for France), variant (which indicates some variant of a locale), and extensions (which provides a map of single character keys to `String` values, indicating extensions apart from language identification).
To create a `Locale` object from a language tag `String`, invoke 


[`Locale.forLanguageTag(String)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#forLanguageTag-java.lang.String-), passing in the language tag as its only argument. Doing so creates and returns a new `Locale`
 object for use in your application.



Example 1:


```

package languagetagdemo;

import java.util.Locale;

public class LanguageTagDemo {
     public static void main(String[] args) {
         Locale l = Locale.forLanguageTag("en-US");
     }
}

```


Note that the Locale API only requires that your language tag be syntactically well-formed. It does not perform any extra validation (such as checking to see if the tag is registered in the IANA Language Subtag Registry).


## <a name="what-ranges" id="what-ranges">What Are Language Ranges?</a>


Language ranges (represented by class 

[`java.util.Locale.LanguageRange`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.LanguageRange.html)) identify sets of language tags that share specific attributes. Language ranges are classified as either basic or extended, and are similar to language tags in that they consist of subtags separated by hyphens. Examples of basic language ranges include "en" (English), "ja-JP" (Japanese, Japan), and "*" (a special language range which matches any language tag). Examples of extended language ranges include "*-CH" (any language, Switzerland), "es-*" (Spanish, any regions), and "zh-Hant-*" (Traditional Chinese, any region).



Furthermore, language ranges may be stored in Language Priority Lists, which enable users to prioritize their language preferences in a weighted list. Language Priority Lists are expressed by placing `LanguageRange` objects into a 

[`java.util.List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html), which can then be passed to the `Locale` methods that accept a `List` of `LanguageRange` objects.


## <a name="creating-range" id="creating-range">Creating a Language Range</a>


The 
[`Locale.LanguageRange`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.LanguageRange.html) class provides two different constructors for creating language ranges:


   - `public Locale.LanguageRange(String range)`

   - `public Locale.LanguageRange(String range, double weight)`


The only difference between them is that the second version allows a weight to be specified; this weight will be considered if the range is placed into a Language Priority List.



`Locale.LanguageRange` also specifies some constants to be used with these constructors:


    - `public static final double MAX_WEIGHT`

    - `public static final double MIN_WEIGHT`


The `MAX_WEIGHT` constant holds a value of 1.0, which indicates that it is a good fit for the user. The `MIN_WEIGHT` constant holds a value of 0.0, indicating that it is not.



Example 2:


```

package languagetagdemo;

import java.util.Locale;

public class LanguageTagDemo {

     public static void main(String[] args) {
         // Create Locale
         Locale l = Locale.forLanguageTag("en-US");

         // Define Some LanguageRange Objects
         Locale.LanguageRange range1 = new Locale.LanguageRange("en-US",Locale.LanguageRange.MAX_WEIGHT);
         Locale.LanguageRange range2 = new Locale.LanguageRange("en-GB*",0.5);
         Locale.LanguageRange range3 = new Locale.LanguageRange("fr-FR",Locale.LanguageRange.MIN_WEIGHT);
     }
}

```


Example 2 creates three language ranges: English (United States), English (Great Britain), and French (France). These ranges are weighted to express the user's preferences, in order from most preferred to least preferred.


## <a name="creating-lpl" id="creating-lpl">Creating a Language Priority List</a>


You can create a Language Priority List from a list of language ranges
by using the 
[`LanguageRange.parse(String)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.LanguageRange.html#parse-java.lang.String-) method. This method accepts a
list of comma-separated language ranges, performs a syntactic check for
each language range in the given ranges, and then returns the newly
created Language Priority List.



For detailed information about the required format of the "ranges"
parameter, see the API specification for this method.



Example 3:


```

package languagetagdemo;

import java.util.Locale;

import java.util.List;

public class LanguageTagDemo {

    public static void main(String[] args) {

        // Create Locale

        Locale l = Locale.forLanguageTag("en-US");

        // Create a Language Priority List

        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";

        List&lt;Locale.LanguageRange&gt; languageRanges = Locale.LanguageRange.parse(ranges)

    }
}

```


Example 3 creates the same three language ranges as Example 2, but
stores them in a `String` object, which is passed to the `parse(String)`
method. The returned `List` of `LanguageRange` objects is the Language
Priority List.


## <a name="filtering" id="filtering">Filtering Language Tags</a>


Language tag filtering is the process of matching a set of language tags against a user's Language Priority List. The result of filtering will be a complete list of all matching results. The `Locale` class defines two filter methods that return a list of `Locale` objects. Their signatures are as follows:


    <li>
[`public static List&lt;Locale&gt; filter (List&lt;Locale.LanguageRange&gt; priorityList, Collection&lt;Locale&gt; locales)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#filter-java.util.List-java.util.Collection-)    </li>
    <li>
[`public static List&lt;Locale&gt; filter (List&lt;Locale.LanguageRange&gt; priorityList, Collection&lt;Locale&gt; locales, Locale.FilteringMode mode)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#filter-java.util.List-java.util.Collection-java.util.Locale.FilteringMode-)</li>


In both methods, the first argument specifies the user's Language Priority List as described in the previous section.



The second argument specifies a `Collection` of `Locale` objects to match against. 
The match itself will take place according to the rules specified by RFC 4647.



The third argument (if provided) specifies the "filtering mode" to use. The 
[`Locale.FilteringMode`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.FilteringMode.html) enum provides a number of different values to choose from, such as `AUTOSELECT_FILTERING` (for basic language range filtering) or `EXTENDED_FILTERING` (for extended language range filtering).



Example 4 provides a demonstration of language tag filtering.



Example 4:


```

package languagetagdemo;

import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;

public class LanguageTagDemo {

    public static void main(String[] args) {

        // Create a collection of Locale objects to filter
        Collection&lt;Locale&gt; locales = new ArrayList&lt;&gt;();
        locales.add(Locale.forLanguageTag("en-GB"));
        locales.add(Locale.forLanguageTag("ja"));
        locales.add(Locale.forLanguageTag("zh-cmn-Hans-CN"));
        locales.add(Locale.forLanguageTag("en-US"));

        // Express the user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List&lt;Locale.LanguageRange&gt; languageRanges = Locale.LanguageRange.parse(ranges);

        // Now filter the Locale objects, returning any matches
        List&lt;Locale&gt; results = Locale.filter(languageRanges,locales);

        // Print out the matches
        for(Locale l : results){
        System.out.println(l.toString());
        }
    }
}

```


The output of this program is:



en_US<br />
en_GB



This returned list is ordered according to the weights specified in the user's Language Priority List.



The `Locale` class also defines `filterTags` methods for filtering language tags as `String` objects.



The method signatures are as follows:


   <li>
[`public static List&lt;String&gt; filterTags (List&lt;Locale.LanguageRange&gt; priorityList, Collection&lt;String&gt; tags)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#filterTags-java.util.List-java.util.Collection-)</li>

   <li>
[`public static List&lt;String&gt; filterTags (List&lt;Locale.LanguageRange&gt; priorityList, Collection&lt;String&gt; tags, Locale.FilteringMode mode)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#filterTags-java.util.List-java.util.Collection-java.util.Locale.FilteringMode-)
</li>


Example 5 provides the same search as Example 4, but uses `String` objects instead of `Locale` objects.



Example 5:


```

package languagetagdemo;

import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;

public class LanguageTagDemo {

    public static void main(String[] args) {

        // Create a collection of String objects to match against
        Collection&lt;String&gt; tags = new ArrayList&lt;&gt;();
        tags.add("en-GB");
        tags.add("ja");
        tags.add("zh-cmn-Hans-CN");
        tags.add("en-US");

        // Express user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List&lt;Locale.LanguageRange&gt; languageRanges = Locale.LanguageRange.parse(ranges);

        // Now search the locales for the best match
        List&lt;String&gt; results = Locale.filterTags(languageRanges,tags);

        // Print out the matches
        for(String s : results){
            System.out.println(s);
        }
    }
} 



```


As before, the search will match and return "en-US" and "en-GB" (in that order).


## <a name="lookup" id="lookup">Performing Language Tag Lookup</a>


In contrast to language tag filtering, language tag lookup is the process of matching language ranges to sets of language tags and returning the one language tag that best matches the range. RFC4647 states that: "Lookup produces the single result that best matches the user's preferences from the list of available tags, so it is useful in cases in which a single item is required (and for which only a single item can be returned). For example, if a process were to insert a human-readable error message into a protocol header, it might select the text based on the user's language priority list. Since the process can return only one item, it is forced to choose a single item and it has to return some item, even if none of the content's language tags match the language priority list supplied by the user."



Example 6:


```

package languagetagdemo;

import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;

public class LanguageTagDemo {

    public static void main(String[] args) {

        // Create a collection of Locale objects to search
        Collection&lt;Locale&gt; locales = new ArrayList&lt;&gt;();
        locales.add(Locale.forLanguageTag("en-GB"));
        locales.add(Locale.forLanguageTag("ja"));
        locales.add(Locale.forLanguageTag("zh-cmn-Hans-CN"));
        locales.add(Locale.forLanguageTag("en-US"));

        // Express the user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List&lt;Locale.LanguageRange&gt; languageRanges = Locale.LanguageRange.parse(ranges);

        // Find the BEST match, and return just one result
        Locale result = Locale.lookup(languageRanges,locales);
        System.out.println(result.toString());
    }
}


```


In contrast to the filtering examples, the lookup demo in Example 6 returns the one object that is the best match (`en-US` in this case). For completenes, Example 7 shows how to perform the same lookup using `String` objects.



Example 7:


```

package languagetagdemo;

import java.util.Locale;
import java.util.Collection;
import java.util.List;
import java.util.ArrayList;

public class LanguageTagDemo {

    public static void main(String[] args) {
        // Create a collection of String objects to match against
        Collection&lt;String&gt; tags = new ArrayList&lt;&gt;();
        tags.add("en-GB");
        tags.add("ja");
        tags.add("zh-cmn-Hans-CN");
        tags.add("en-US");

        // Express user's preferences with a Language Priority List
        String ranges = "en-US;q=1.0,en-GB;q=0.5,fr-FR;q=0.0";
        List&lt;Locale.LanguageRange&gt; languageRanges = Locale.LanguageRange.parse(ranges);

        // Find the BEST match, and return just one result
        String result = Locale.lookupTag(languageRanges, tags);
        System.out.println(result);
    }
} 

```


This example returns the single object that best matches the user's Language Priority List.

