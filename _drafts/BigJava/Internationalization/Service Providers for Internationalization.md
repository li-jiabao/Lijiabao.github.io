
# Lesson: Service Providers for Internationalization

Service providers for internationalization enable the plug-in of locale-dependent data and services. Because locale-dependent data and services can be plugged-in, third parties are able to provide implementations of most locale-sensitive classes in the `java.text` and `java.util` packages.

A service is a set of programming interfaces and classes that provide access to a specific application's functionality or feature. A service provider interface (SPI) is the set of public interfaces and abstract classes that a service defines. A service provider implements the SPI. Service providers enable you to create extensible applications, which you can extend without modifying its original code base. You can enhance their functionality with new plug-ins or modules. For more information about service providers and extensible applications, see 
[Creating Extensible Applications](../../ext/basics/spi.html).

You can use service providers for internationalization to provide custom implementations of the following locale-sensitive classes:

  <li>
[`BreakIterator`](https://docs.oracle.com/javase/8/docs/api/java/text/BreakIterator.html) objects</li>
 
  <li>
[`Collator`](https://docs.oracle.com/javase/8/docs/api/java/text/Collator.html) objects</li>
 
  <li>Language code, country code, and variant name for the
[`Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html) class</li>
  - Time zone names
  - Currency symbols
  <li>
[`DateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormat.html) objects</li>
 
  <li>
[`DateFormatSymbols`](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormatSymbols.html) objects</li>
  <li>
[`NumberFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/NumberFormat.html) objects</li>
  <li>
[`DecimalFormatSymbols`](https://docs.oracle.com/javase/8/docs/api/java/text/DecimalFormatSymbols.html) objects</li>

The corresponding SPIs are contained both in `java.text.spi` and in `java.util.spi` packages:

<li>
[`CurrencyNameProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/CurrencyNameProvider.html)</li>
<li>
[`LocaleServiceProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/LocaleServiceProvider.html)</li>
<li>
[`TimeZoneNameProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/TimeZoneNameProvider.html)</li>

<li>
[`BreakIteratorProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/BreakIteratorProvider.html)</li>
<li>
[`CollatorProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/CollatorProvider.html)</li>
<li>
[`DateFormatProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/DateFormatProvider.html)</li>
<li>
[`DateFormatSymbolsProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/DateFormatSymbolsProvider.html)</li>
<li>
[`DecimalFormatSymbolsProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/DecimalFormatSymbolsProvider.html)</li>
<li>
[`NumberFormatProvider`](https://docs.oracle.com/javase/8/docs/api/java/text/spi/NumberFormatProvider.html)</li>

For example, if you want to provide a `NumberFormat` object for a new locale, implement the `java.text.spi.NumberFormatProvider` class and implement these methods:

- `getCurrencyInstance(Locale locale)`
- `getIntegerInstance(Locale locale)`
- `getNumberInstance(Locale locale)`
- `getPercentInstance(Locale locale)`

```

Locale loc = new Locale("da", "DK");
NumberFormat nf = NumberFormatProvider.getNumberInstance(loc);

```

These methods first check whether the Java runtime environment supports the requested locale; if so, the methods use that support. Otherwise, the methods invoke the `getAvailableLocales` methods of installed providers for the appropriate interface to find a provider that supports the requested locale.

For an in-depth example of how to use service providers for internationalization, see    
[Installing a Custom Resource Bundle as an Extension](../../i18n/serviceproviders/resourcebundlecontrolprovider.html). This section shows you how to implement the
[`ResourceBundleControlProvider`](https://docs.oracle.com/javase/8/docs/api/java/util/spi/ResourceBundleControlProvider.html) interface, which enables you to use any custom `ResourceBundle.Control` classes without any additional changes to the source code of your application.
