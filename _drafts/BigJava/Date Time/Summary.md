
# Summary


The <tt>java.time</tt> package contains many classes that your programs can use to represent time and date. This is a very rich API. The key entry points for ISO-based dates are as follows:

- The <tt>Instant</tt> class provides a machine view of the timeline.
- The <tt>LocalDate</tt>, <tt>LocalTime</tt>, and <tt>LocalDateTime</tt> classes provide a human view of date and time without any reference to time zone.
- The <tt>ZoneId</tt>, <tt>ZoneRules</tt>, and <tt>ZoneOffset</tt> classes describe time zones, time zone offsets, and time zone rules.
- The <tt>ZonedDateTime</tt> class represents date and time with a time zone. The <tt>OffsetDateTime</tt> and <tt>OffsetTime</tt> classes represent date and time, or time, respectively. These classes take a time zone offset into account.
- The <tt>Duration</tt> class measures an amount of time in seconds and nanoseconds.
- The <tt>Period</tt> class measures an amount of time using years, months, and days.


Other non-ISO calendar systems can be represented using the <tt>java.time.chrono</tt> package. This package is beyond the scope of this tutorial, though the
[Non-ISO Date Conversion](nonIso.html) page provides information about converting an ISO-based date to another calendar system.


The Date Time API was developed as part of the Java community process under the designation of JSR 310. For more information, see
[JSR 310: Date and Time API](http://jcp.org/en/jsr/detail?id=310).
