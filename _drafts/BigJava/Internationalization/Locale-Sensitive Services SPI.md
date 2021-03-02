
# Locale-Sensitive Services SPI

This feature enables the plug-in of locale-dependent data and services. In this way, third parties are able to provide implementations of most locale-sensitive classes in the `java.text` and `java.util` packages.

The implementation of SPIs **(Service Provider Interface)** is based on abstract classes and Java interfaces that are implemented by the service provider. At runtime the Java class loading mechanism is used to dynamically locate and load classes that implement the SPI.<br />

You can use the locale-sensitive services SPI to provide the following locale sensitive implementations:

- `BreakIterator` objects
- `Collator` objects
- Language code, Country code, and Variant name for the `Locale` class
- Time Zone names
- Currency symbols
- `DateFormat` objects
- `DateFormatSymbol` objects
- `NumberFormat` objects
- `DecimalFormatSymbols` objects

The corresponding SPIs are contained both in `java.text.spi` and in `java.util.spi` packages:

- `CurrencyNameProvider`
- `LocaleServiceProvider`
- `TimeZoneNameProvider`
- `CalendarDataProvider`

- `BreakIteratorProvider`
- `CollatorProvider`
- `DateFormatProvider`
- `DateFormatSymbolsProvider`
- `DecimalFormatSymbolsProvider`
- `NumberFormatProvider`

For example, if you would like to provide a `NumberFormat` object for a new locale, you have to implement the `java.text.spi.NumberFormatProvider` class. You need to extend this class and implement its methods:

- `getCurrencyInstance(Locale locale)`
- `getIntegerInstance(Locale locale)`
- `getNumberInstance(Locale locale)`
- `getPercentInstance(Locale locale)`

```

Locale loc = new Locale("da", "DK");
NumberFormat nf = NumberFormatProvider.getNumberInstance(loc);

```

These methods first check whether the Java runtime environment supports the requested locale; if so, they use that support. Otherwise, the methods call the `getAvailableLocales()` methods of installed providers for the appropriate interface to find a provider that supports the requested locale.
