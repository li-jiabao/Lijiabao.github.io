
# Numbers and Currencies

Programs store and operate on numbers in a locale-independent way. Before displaying or printing a number, a program must convert it to a `String` that is in a locale-sensitive format. For example, in France the number 123456.78 should be formatted as 123 456,78, and in Germany it should appear as 123.456,78. In this section, you will learn how to make your programs independent of the locale conventions for decimal points, thousands-separators, and other formatting properties.

## [Using Predefined Formats](numberFormat.html)

Using the factory methods provided by the `NumberFormat` class, you can get locale-specific formats for numbers, currencies, and percentages.

## [Formatting with Patterns](decimalFormat.html)

With the `DecimalFormat` class you specify a number's format with a `String` pattern. The `DecimalFormatSymbols` class allows you to modify formatting symbols such as decimal separators and minus signs.
