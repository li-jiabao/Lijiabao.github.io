
# The Temporal Package


The 
[<tt>java.time.temporal</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/package-summary.html) package provides a collection of interfaces, classes, and enums that support date and time code and, in particular, date and time calculations.


These interfaces are intended to be used at the lowest level. Typical application code should declare variables and parameters in terms of the concrete type, such as <tt>LocalDate</tt> or <tt>ZonedDateTime</tt>, and not in terms of the <tt>Temporal</tt> interface. This is exactly the same as declaring a variable of type <tt>String</tt>, and not of type <tt>CharSequence</tt>.

## Temporal and TemporalAccessor


The
[<tt>Temporal</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/Temporal.html) interface provides a framework for accessing temporal-based objects, and is implemented by the temporal-based classes, such as <tt>Instant</tt>, <tt>LocalDateTime</tt>, and <tt>ZonedDateTime</tt>. This interface provides methods to add or subtract units of time, making time-based arithmetic easy and consistent across the various date and time classes. The
[<tt>TemporalAccessor</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAccessor.html) interface provides a read-only version of the <tt>Temporal</tt> interface.


Both <tt>Temporal</tt> and <tt>TemporalAccessor</tt> objects are defined in terms of fields, as specified in the 
[<tt>TemporalField</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalField.html) interface. The 
[<tt>ChronoField</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoField.html) enum is a concrete implementation of the <tt>TemporalField</tt> interface and provides a rich set of defined constants, such as <tt>DAY_OF_WEEK</tt>, <tt>MINUTE_OF_HOUR</tt>, and <tt>MONTH_OF_YEAR</tt>.


The units for these fields are specified by the 
[<tt>TemporalUnit</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalUnit.html) interface. The <tt>ChronoUnit</tt> enum implements the <tt>TemporalUnit</tt> interface. The field <tt>ChronoField.DAY_OF_WEEK</tt> is a combination of <tt>ChronoUnit.DAYS</tt> and <tt>ChronoUnit.WEEKS</tt>. The <tt>ChronoField</tt> and <tt>ChronoUnit</tt> enums are discussed in the following sections.


The arithmetic-based methods in the <tt>Temporal</tt> interface require parameters defined in terms of
[<tt>TemporalAmount</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAmount.html) values. The <tt>Period</tt> and <tt>Duration</tt> classes (discussed in 
[Period and Duration](period.html)) implement the <tt>TemporalAmount</tt> interface.

## ChronoField and IsoFields


The
[<tt>ChronoField</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoField.html) enum, which implements the <tt>TemporalField</tt> interface, provides a rich set of constants for accessing date and time values. A few examples are <tt>CLOCK_HOUR_OF_DAY</tt>, <tt>NANO_OF_DAY</tt>, and <tt>DAY_OF_YEAR</tt>. This enum can be used to express conceptual aspects of time, such as the third week of the year, the 11th hour of the day, or the first Monday of the month. When you encounter a <tt>Temporal</tt> of unknown type, you can use the
[<tt>TemporalAccessor.isSupported(TemporalField)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAccessor.html#isSupported-java.time.temporal.TemporalField-) method to determine if the <tt>Temporal</tt> supports a particular field. The following line of code returns <tt>false</tt>, indicating that <tt>LocalDate</tt> does not support <tt>ChronoField.CLOCK_HOUR_OF_DAY</tt>:

```

boolean isSupported = LocalDate.now().isSupported(ChronoField.CLOCK_HOUR_OF_DAY);

```


Additional fields, specific to the ISO-8601 calendar system, are defined in the
[<tt>IsoFields</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/IsoFields.html) class. The following examples show how to obtain the value of a field using both <tt>ChronoField</tt> and <tt>IsoFields</tt>:

```

time.get(ChronoField.MILLI_OF_SECOND)
int qoy = date.get(IsoFields.QUARTER_OF_YEAR);

```


Two other classes define additional fields that may be useful,
[<tt>WeekFields</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/WeekFields.html) and
[<tt>JulianFields</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/JulianFields.html).

## ChronoUnit


The
[<tt>ChronoUnit</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoUnit.html) enum implements the <tt>TemporalUnit</tt> interface, and provides a set of standard units based on date and time, from milliseconds to millennia. Note that not all <tt>ChronoUnit</tt> objects are supported by all classes. For example, the <tt>Instant</tt> class does not support <tt>ChronoUnit.MONTHS</tt> or <tt>ChronoUnit.YEARS</tt>. The
[<tt>TemporalAccessor.isSupported(TemporalUnit)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAccessor.html#isSupported-java.time.temporal.TemporalUnit-) method can be used to verify whether a class supports a particular time unit. The following call to <tt>isSupported</tt> returns <tt>false</tt>, confirming that the <tt>Instant</tt> class does not support <tt>ChronoUnit.DAYS</tt>.

```

boolean isSupported = instant.isSupported(ChronoUnit.DAYS);

```
