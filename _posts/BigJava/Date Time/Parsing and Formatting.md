
# Parsing and Formatting


The temporal-based classes in the Date-Time API provide <tt>parse</tt> methods for parsing a string that contains date and time information. These classes also provide <tt>format</tt> methods for formatting temporal-based objects for display. In both cases, the process is similar: you provide a pattern to the <tt>DateTimeFormatter</tt> to create a formatter object. This formatter is then passed to the <tt>parse</tt> or <tt>format</tt> method.


The <tt>DateTimeFormatter</tt> class provides numerous
[predefined formatters](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#predefined), or you can define your own.


The <tt>parse</tt> and the <tt>format</tt> methods throw an exception if a problem occurs during the conversion process. Therefore, your parse code should catch the <tt>DateTimeParseException</tt> error and your format code should catch the <tt>DateTimeException</tt> error. For more information on exception handing, see
[Catching and Handling Exceptions](../../essential/exceptions/handling.html).


The <tt>DateTimeFormatter</tt> class is both immutable and thread-safe; it can (and should) be assigned to a static constant where appropriate.

## Parsing


The one-argument
[<tt>parse(CharSequence)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#parse-java.lang.CharSequence-) method in the <tt>LocalDate</tt> class uses the <tt>ISO_LOCAL_DATE</tt> formatter. To specify a different formatter, you can use the two-argument
[<tt>parse(CharSequence, DateTimeFormatter)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#parse-java.lang.CharSequence-java.time.format.DateTimeFormatter-) method. The following example uses the predefined <tt>BASIC_ISO_DATE</tt> formatter, which uses the format <tt>19590709</tt> for July 9, 1959.

```

String in = ...;
LocalDate date = LocalDate.parse(in, DateTimeFormatter.BASIC_ISO_DATE);

```


You can also define a formatter using your own pattern. The following code, from the
[`<tt>Parse</tt>`](examples/Parse.java) example, creates a formatter that applies a format of "MMM d yyyy". This format specifies three characters to represent the month, one digit to represent day of the month, and four digits to represent the year. A formatter created using this pattern would recognize strings such as "Jan 3 2003" or "Mar 23 1994". However, to specify the format as "MMM dd yyyy", with two characters for day of the month, then you would have to always use two characters, padding with a zero for a one-digit date: "Jun 03 2003".

```

String input = ...;
try {
    DateTimeFormatter formatter =
                      DateTimeFormatter.ofPattern("MMM d yyyy");
    LocalDate date = LocalDate.parse(input, formatter);
    System.out.printf("%s%n", date);
}
catch (DateTimeParseException exc) {
    System.out.printf("%s is not parsable!%n", input);
    throw exc;      // Rethrow the exception.
}
// 'date' has been successfully parsed

```


The documentation for the <tt>DateTimeFormatter</tt> class specifies the
[full list of symbols](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#patterns) that you can use to specify a pattern for formatting or parsing.


The <tt>StringConverter</tt> example on the
[Non-ISO Date Conversion](nonIso.html) page provides another example of a date formatter.

## Formatting


The
[<tt>format(DateTimeFormatter)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#format-java.time.format.DateTimeFormatter-) method converts a temporal-based object to a string representation using the specified format. The following code, from the
[`<tt>Flight</tt>`](examples/Flight.java) example, converts an instance of <tt>ZonedDateTime</tt> using the format "MMM d yyy  hh:mm a". The date is defined in the same manner as was used for the previous parsing example, but this pattern also includes the hour, minutes, and a.m. and p.m. components.

```

ZoneId leavingZone = ...;
ZonedDateTime departure = ...;

try {
    DateTimeFormatter format = DateTimeFormatter.ofPattern("MMM d yyyy  hh:mm a");
    String out = departure.format(format);
    System.out.printf("LEAVING:  %s (%s)%n", out, leavingZone);
}
catch (DateTimeException exc) {
    System.out.printf("%s can't be formatted!%n", departure);
    throw exc;
}

```


The output for this example, which prints both the arrival and departure time, is as follows:

```

LEAVING:  Jul 20 2013  07:30 PM (America/Los_Angeles)
ARRIVING: Jul 21 2013  10:20 PM (Asia/Tokyo)

```
