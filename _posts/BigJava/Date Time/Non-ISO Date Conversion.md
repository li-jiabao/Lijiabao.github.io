
# Non-ISO Date Conversion


This tutorial does not discuss the
[<tt>java.time.chrono</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/chrono/package-summary.html) package in any detail. However, it might be useful to know that this package provides several predefined chronologies that are not ISO-based, such as Japanese, Hijrah, Minguo, and Thai Buddhist. You can also use this package to create your own chronology.


This section shows you how to convert between an ISO-based date and a date in one of the other predefined chronologies.

## Converting to a Non-ISO-Based Date


You can convert an ISO-based date to a date in another chronology by using the <tt>from(TemporalAccessor)</tt> method, such as
[<tt>JapaneseDate.from(TemporalAccessor)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/chrono/JapaneseDate.html#from-java.time.temporal.TemporalAccessor-). This method throws a <tt>DateTimeException</tt> if it is unable to convert the date to a valid instance. The following code converts a <tt>LocalDateTime</tt> instance to several predefined non-ISO calendar dates:

```

LocalDateTime date = LocalDateTime.of(2013, Month.JULY, 20, 19, 30);
JapaneseDate jdate     = JapaneseDate.from(date);
HijrahDate hdate       = HijrahDate.from(date);
MinguoDate mdate       = MinguoDate.from(date);
ThaiBuddhistDate tdate = ThaiBuddhistDate.from(date);

```


The 
[`<tt>StringConverter</tt>`](examples/StringConverter.java) example converts from a <tt>LocalDate</tt> to a <tt>ChronoLocalDate</tt> to a <tt>String</tt> and back. The <tt>toString</tt> method takes an instance of <tt>LocalDate</tt> and a <tt>Chronology</tt> and returns the converted string by using the provided <tt>Chronology</tt>. The <tt>DateTimeFormatterBuilder</tt> is used to build a string that can be used for printing the date:

```

/**
 * Converts a LocalDate (ISO) value to a ChronoLocalDate date
 * using the provided Chronology, and then formats the
 * ChronoLocalDate to a String using a DateTimeFormatter with a
 * SHORT pattern based on the Chronology and the current Locale.
 *
 * @param localDate - the ISO date to convert and format.
 * @param chrono - an optional Chronology. If null, then IsoChronology is used.
 */
public static String toString(LocalDate localDate, Chronology chrono) {
    if (localDate != null) {
        Locale locale = Locale.getDefault(Locale.Category.FORMAT);
        ChronoLocalDate cDate;
        if (chrono == null) {
            chrono = IsoChronology.INSTANCE;
        }
        try {
            cDate = chrono.date(localDate);
        } catch (DateTimeException ex) {
            System.err.println(ex);
            chrono = IsoChronology.INSTANCE;
            cDate = localDate;
        }
        DateTimeFormatter dateFormatter =
            DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT)
                             .withLocale(locale)
                             .withChronology(chrono)
                             .withDecimalStyle(DecimalStyle.of(locale));
        String pattern = "M/d/yyyy GGGGG";
        return dateFormatter.format(cDate);
    } else {
        return "";
    }
}

```


When the method is invoked with the following date for the predefined chronologies:

```

LocalDate date = LocalDate.of(1996, Month.OCTOBER, 29);
System.out.printf("%s%n",
     StringConverter.toString(date, JapaneseChronology.INSTANCE));
System.out.printf("%s%n",
     StringConverter.toString(date, MinguoChronology.INSTANCE));
System.out.printf("%s%n",
     StringConverter.toString(date, ThaiBuddhistChronology.INSTANCE));
System.out.printf("%s%n",
     StringConverter.toString(date, HijrahChronology.INSTANCE));

```


The output looks like this:

```

10/29/0008 H
10/29/0085 1
10/29/2539 B.E.
6/16/1417 1

```

## Converting to an ISO-Based Date


You can convert from a non-ISO date to a <tt>LocalDate</tt> instance using the static
[<tt>LocalDate.from</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#from-java.time.temporal.TemporalAccessor-) method, as shown in the following example:

```

LocalDate date = LocalDate.from(JapaneseDate.now());

```


Other temporal-based classes also provide this method, which throws a <tt>DateTimeException</tt> if the date cannot be converted.


The <tt>fromString</tt> method, from the
[`<tt>StringConverter</tt>`](examples/StringConverter.java) example, parses a <tt>String</tt> containing a non-ISO date and returns a <tt>LocalDate</tt> instance.

```

/**
 * Parses a String to a ChronoLocalDate using a DateTimeFormatter
 * with a short pattern based on the current Locale and the
 * provided Chronology, then converts this to a LocalDate (ISO)
 * value.
 *
 * @param text   - the input date text in the SHORT format expected
 *                 for the Chronology and the current Locale.
 *
 * @param chrono - an optional Chronology. If null, then IsoChronology
 *                 is used.
 */
public static LocalDate fromString(String text, Chronology chrono) {
    if (text != null &amp;&amp; !text.isEmpty()) {
        Locale locale = Locale.getDefault(Locale.Category.FORMAT);
        if (chrono == null) {
           chrono = IsoChronology.INSTANCE;
        }
        String pattern = "M/d/yyyy GGGGG";
        DateTimeFormatter df = new DateTimeFormatterBuilder().parseLenient()
                              .appendPattern(pattern)
                              .toFormatter()
                              .withChronology(chrono)
                              .withDecimalStyle(DecimalStyle.of(locale));
        TemporalAccessor temporal = df.parse(text);
        ChronoLocalDate cDate = chrono.date(temporal);
        return LocalDate.from(cDate);
    }
return null;
}

```


When the method is invoked with the following strings:

```

System.out.printf("%s%n", StringConverter.fromString("10/29/0008 H",
    JapaneseChronology.INSTANCE));
System.out.printf("%s%n", StringConverter.fromString("10/29/0085 1",
    MinguoChronology.INSTANCE));
System.out.printf("%s%n", StringConverter.fromString("10/29/2539 B.E.",
    ThaiBuddhistChronology.INSTANCE));
System.out.printf("%s%n", StringConverter.fromString("6/16/1417 1",
    HijrahChronology.INSTANCE));

```


The printed strings should all convert back to October 29th, 1996:

```

1996-10-29
1996-10-29
1996-10-29
1996-10-29

```
