---
title: Creating a Locale
author: lijiabao
date: 2020-12-06 01:49:51.184068700 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Creating a Locale

There are several ways to create a `Locale` object. Regardless of the technique used, creation can be as simple as specifying the language code. However, you can further distinguish the locale by setting the region (also referred to as "country") and variant codes. If you are using the JDK 7 release or later, you can also specify the script code and Unicode locale extensions.

The four ways to create a `Locale` object are:

- [`Locale.Builder` Class](#builder)
- [`Locale` Constructors](#constructors)
- [`Locale.forLanguageTag` Factory Method](#factory)
- [`Locale` Constants](#constants)

## <a name="builder" id="builder">`LocaleBuilder` Class</a>

The
[`Locale.Builder`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.Builder.html) utility class can be used to construct a `Locale` object that conforms to the IETF BCP 47 syntax. For example, to specify the French language and the country of Canada, you could invoke the `Locale.Builder` constructor and then chain the setter methods as follows:

```

Locale aLocale = new Locale.Builder().setLanguage("fr").setRegion("CA").build();

```

The next example creates `Locale` objects for the English language in the United States and Great Britain:

```

Locale bLocale = new Locale.Builder().setLanguage("en").setRegion("US").build();
Locale cLocale = new Locale.Builder().setLanguage("en").setRegion("GB").build();

```

The final example creates a `Locale` object for the Russian language:

```

Locale dLocale = new Locale.Builder().setLanguage("ru").setScript("Cyrl").build();

```

## <a name="constructors" id="constructors">`Locale` Constructors</a>

There are three constructors available in the `Locale` class for creating a `Locale` object:

<li>
[`Locale(String language)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#Locale-java.lang.String-)</li>
<li>
[`Locale(String language, String country)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#Locale-java.lang.String-java.lang.String-)</li>
<li>
[`Locale(String language, String country, String variant)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#Locale-java.lang.String-java.lang.String-java.lang.String-)</li>

The following examples create `Locale` objects for the French language in Canada, the English language in the U.S. and Great Britain, and the Russian language.

```

aLocale = new Locale("fr", "CA");
bLocale = new Locale("en", "US");
cLocale = new Locale("en", "GB");
dLocale = new Locale("ru");

```

It is not possible to set a script code on a `Locale` object in a release earlier than JDK 7.

## <a name="factory" id="factory">`forLanguageTag` Factory Method</a>

If you have a language tag string that conforms to the IETF BCP 47 standard, you can use the
[`forLanguageTag(String)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#forLanguageTag-java.lang.String-) factory method, which was introduced in the Java SE 7 release. For example:

```

Locale aLocale = Locale.forLanguageTag("en-US");
Locale bLocale = Locale.forLanguageTag("ja-JP-u-ca-japanese");

```

## <a name="constants" id="constants">`Locale` Constants</a>

For your convenience the `Locale` class provides
[constants](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#field_summary) for some languages and countries. For example:

```

cLocale = Locale.JAPAN;
dLocale = Locale.CANADA_FRENCH;

```

When you specify a language constant, the region portion of the `Locale` is undefined. The next three statements create equivalent `Locale` objects:

```

j1Locale = Locale.JAPANESE;
j2Locale = new Locale.Builder().setLanguage("ja").build();
j3Locale = new Locale("ja");

```

The `Locale` objects created by the following three statements are also equivalent:

```

j4Locale = Locale.JAPAN;
j5Locale = new Locale.Builder().setLanguage("ja").setRegion("JP").build();
j6Locale = new Locale("ja", "JP");

```

## Codes

The following sections discuss the language code and the optional script, region, and variant codes.

### Language Code

The language code is either two or three lowercase letters that conform to the ISO 639 standard. You can find a full list of the ISO 639 codes at
[http://www.loc.gov/standards/iso639-2/php/code_list.php](http://www.loc.gov/standards/iso639-2/php/code_list.php).

The following table lists a few of the language codes.

<th id="h1">Language Code</th><th id="h2">Description</th>
|------
<td headers="h1">`de`</td><td headers="h2">German</td>
<td headers="h1">`en`</td><td headers="h2">English</td>
<td headers="h1">`fr`</td><td headers="h2">French</td>
<td headers="h1">`ru`</td><td headers="h2">Russian</td>
<td headers="h1">`ja`</td><td headers="h2">Japanese</td>
<td headers="h1">`jv`</td><td headers="h2">Javanese</td>
<td headers="h1">`ko`</td><td headers="h2">Korean</td>
<td headers="h1">`zh`</td><td headers="h2">Chinese</td>

### Script Code

The script code begins with an uppercase letter followed by three lowercase letters and conforms to the ISO 15924 standard. You can find a full list of the ISO 15924 codes at
[http://unicode.org/iso15924/iso15924-codes.html](http://unicode.org/iso15924/iso15924-codes.html).

The following table lists a few of the script codes.

<th id="h101">Script Code</th><th id="h102">Description</th>
|------
<td headers="h101">`Arab`</td><td headers="h102">Arabic</td>
<td headers="h101">`Cyrl`</td><td headers="h102">Cyrillic</td>
<td headers="h101">`Kana`</td><td headers="h102">Katakana</td>
<td headers="h101">`Latn`</td><td headers="h102">Latin</td>

There are three methods for retrieving the script information for a `Locale`:

<li>
[`getScript()`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getScript--) &#150; returns the 4-letter script code for a `Locale` object. If no script is defined for the locale, an empty string is returned.</li>
<li>
[`getDisplayScript()`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getDisplayScript--) &#150; returns a name for the locale's script that is appropriate for display to the user. If possible, the name will be localized for the default locale. So, for example, if the script code is "Latn," the diplay script name returned would be the string "Latin" for an English language locale.</li>
<li>
[`getDisplayScript(Locale)`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#getDisplayScript-java.util.Locale-) &#150; returns the display name of the specified `Locale` localized, if possible, for the locale.</li>

### Region Code

The region (country) code consists of either two or three uppercase letters that conform to the ISO 3166 standard, or three numbers that conform to the UN M.49 standard. A copy of the codes can be found at
[http://www.chemie.fu-berlin.de/diverse/doc/ISO_3166.html](http://www.chemie.fu-berlin.de/diverse/doc/ISO_3166.html).

The following table contains several sample country and region codes.
<th id="h201">A-2 Code</th><th id="h202">A-3 Code</th><th id="h203">Numeric Code</th><th id="h204">Description</th>
<td headers="h201">`AU`</td><td headers="h202">`AUS`</td><td headers="h203">`036`</td><td headers="h204">Australia</td>
<td headers="h201">`BR`</td><td headers="h202">`BRA`</td><td headers="h203">`076`</td><td headers="h204">Brazil</td>
<td headers="h201">`CA`</td><td headers="h202">`CAN`</td><td headers="h203">`124`</td><td headers="h204">Canada</td>
<td headers="h201">`CN`</td><td headers="h202">`CHN`</td><td headers="h203">`156`</td><td headers="h204">China</td>
<td headers="h201">`DE`</td><td headers="h202">`DEU`</td><td headers="h203">`276`</td><td headers="h204">Germany</td>
<td headers="h201">`FR`</td><td headers="h202">`FRA`</td><td headers="h203">`250`</td><td headers="h204">France</td>
<td headers="h201">`IN`</td><td headers="h202">`IND`</td><td headers="h203">`356`</td><td headers="h204">India</td>
<td headers="h201">`RU`</td><td headers="h202">`RUS`</td><td headers="h203">`643`</td><td headers="h204">Russian Federation</td>
<td headers="h201">`US`</td><td headers="h202">`USA`</td><td headers="h203">`840`</td><td headers="h204">United States</td>

### <a name="variant-code" id="variant-code">Variant Code</a>

The optional `variant` code can be used to further distinguish your `Locale`. For example, the variant code can be used to indicate dialectical differences that are not covered by the region code.

As of the Java SE 7 release, which conforms to the IETF BCP 47 standard, the variant code is used specifically to indicate additional variations that define a language or its dialects. The IETF BCP 47 standard imposes syntactic restrictions on the variant subtag. You can see a list of variant codes (search for **variant**) at
[http://www.iana.org/assignments/language-subtag-registry](http://www.iana.org/assignments/language-subtag-registry).

For example, Java SE uses the variant code to support the Thai language. By convention, a `NumberFormat` object for the `th` and `th_TH` locales will use common Arabic digit shapes, or Arabic numerals, to format Thai numbers. However, a `NumberFormat` for the `th_TH_TH` locale uses Thai digit shapes. The excerpt from
[`ThaiDigits.java`](examples/ThaiDigits.java) demonstrates this:

```

String outputString = new String();
Locale[] thaiLocale = {
             new Locale("th"),
             new Locale("th", "TH"),
             new Locale("th", "TH", "TH")
         };

for (Locale locale : thaiLocale) {
    NumberFormat nf = NumberFormat.getNumberInstance(locale);
    outputString = outputString + locale.toString() + ": ";
    outputString = outputString + nf.format(573.34) + "\n";
}

```

The following is a screenshot of this sample:
