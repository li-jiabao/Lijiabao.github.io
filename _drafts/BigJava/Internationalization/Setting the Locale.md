
# Lesson: Setting the Locale

An internationalized program can display information differently throughout the world. For example, the program will display different messages in Paris, Tokyo, and New York. If the localization process has been fine-tuned, the program will display different messages in New York and London to account for the differences between American and British English. How does an internationalized program identify the appropriate language and region of its end users? Easy. It references a `Locale` object.

A `Locale` object is an identifier for a particular combination of language and region. If a class varies its behavior according to `Locale`, it is said to be **locale-sensitive**. For example, the `NumberFormat` class is locale-sensitive; the format of the number it returns depends on the `Locale`. Thus `NumberFormat` may return a number as 902 300 (France), or 902.300 (Germany), or 902,300 (United States). `Locale` objects are only identifiers. The real work, such as formatting and detecting word boundaries, is performed by the methods of the locale-sensitive classes.

The following sections explain how to work with `Locale` objects:

## [Creating a Locale](create.html)

When creating a `Locale` object, you usually specify a language code and a country code. A third parameter, the variant, is optional.

## [BCP 47 Extensions](extensions.html)


This section shows you how to add a Unicode locale extension or a private use extension to a <tt>Locale</tt>.

## [Identifying Available Locales](identify.html)

Locale-sensitive classes support only certain `Locale` definitions. This section shows you how to determine which `Locale` definitions are supported.

## [Language Tag Filtering and Lookup](matching.html)


This section describes the internationalization support for language tags, language tags filtering, and language tags lookup.

## [The Scope of a Locale](scope.html)

On the Java platform you do not specify a global `Locale` by setting an environment variable before running the application. Instead you either rely on the default Locale or assign a `Locale` to each locale-sensitive object.

## [Locale-Sensitive Services SPI](services.html)

This section explains how to enable plug-in of locale-dependent data and services. These SPIs (Service Provider Interface) provides support of more locales in addition to the currently available locales.
