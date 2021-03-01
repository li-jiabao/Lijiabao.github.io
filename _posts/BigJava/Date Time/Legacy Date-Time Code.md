
# Legacy Date-Time Code


Prior to the Java SE 8 release, the Java date and time mechanism was provided by the
[`java.util.Date`](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html),
[`java.util.Calendar`](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html), and
[`java.util.TimeZone`](https://docs.oracle.com/javase/8/docs/api/java/util/TimeZone.html) classes, as well as their subclasses, such as
[<tt>java.util.GregorianCalendar</tt>](https://docs.oracle.com/javase/8/docs/api/java/util/GregorianCalendar.html). These classes had several drawbacks, including:

- The <tt>Calendar</tt> class was not type safe.
- Because the classes were mutable, they could not be used in multithreaded applications.
- Bugs in application code were common due to the unusual numbering of months and the lack of type safety.

## Interoperability with Legacy Code


Perhaps you have legacy code that uses the <tt>java.util</tt> date and time classes and you would like to take advantage of the <tt>java.time</tt> functionality with minimal changes to your code.


Added to the JDK 8 release are several methods that allow conversion between <tt>java.util</tt> and <tt>java.time</tt> objects:

<li>
[<tt>Calendar.toInstant()</tt>](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html#toInstant--) converts the <tt>Calendar</tt> object to an <tt>Instant</tt>.</li>
<li>
[<tt>GregorianCalendar.toZonedDateTime()</tt>](https://docs.oracle.com/javase/8/docs/api/java/util/GregorianCalendar.html#toZonedDateTime--) converts a <tt>GregorianCalendar</tt> instance to a <tt>ZonedDateTime</tt>.</li>
<li>
[<tt>GregorianCalendar.from(ZonedDateTime)</tt>](https://docs.oracle.com/javase/8/docs/api/java/util/GregorianCalendar.html#from-java.time.ZonedDateTime-) creates a <tt>GregorianCalendar</tt> object using the default locale from a <tt>ZonedDateTime</tt> instance.</li>
<li>
[<tt>Date.from(Instant)</tt>](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html#from-java.time.Instant-) creates a <tt>Date</tt> object from an <tt>Instant</tt>.</li>
<li>
[<tt>Date.toInstant()</tt>](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html#toInstant--) converts a <tt>Date</tt> object to an <tt>Instant</tt>.</li>
<li>
[<tt>TimeZone.toZoneId()</tt>](https://docs.oracle.com/javase/8/docs/api/java/util/TimeZone.html#toZoneId--) converts a <tt>TimeZone</tt> object to a <tt>ZoneId</tt>.</li>


The following example converts a <tt>Calendar</tt> instance to a <tt>ZonedDateTime</tt> instance. Note that a time zone must be supplied to convert from an <tt>Instant</tt> to a <tt>ZonedDateTime</tt>:

```

Calendar now = Calendar.getInstance();
ZonedDateTime zdt = ZonedDateTime.ofInstant(now.toInstant(), ZoneId.systemDefault()));

```


The following example shows conversion between a <tt>Date</tt> and an <tt>Instant</tt>:

```

Instant inst = date.toInstant();

Date newDate = Date.from(inst);

```


The following example converts from a <tt>GregorianCalendar</tt> to a <tt>ZonedDateTime</tt>, and then from a <tt>ZonedDateTime</tt> to a <tt>GregorianCalendar</tt>. Other temporal-based classes are created using the <tt>ZonedDateTime</tt> instance:

```

GregorianCalendar cal = ...;

TimeZone tz = cal.getTimeZone();
int tzoffset = cal.get(Calendar.ZONE_OFFSET);

ZonedDateTime zdt = cal.toZonedDateTime();

GregorianCalendar newCal = GregorianCalendar.from(zdt);

LocalDateTime ldt = zdt.toLocalDateTime();
LocalDate date = zdt.toLocalDate();
LocalTime time = zdt.toLocalTime();

```

## Mapping java.util Date and Time Functionality to java.time


Because the Java implementation of date and time has been completely redesigned in the Java SE 8 release, you cannot swap one method for another method. If you want to use the rich functionality offered by the <tt>java.time</tt> package,  your easiest solution is to use the <tt>toInstant</tt> or <tt>toZonedDateTime</tt> methods listed in the previous section. However, if you do not want to use that approach or it is not sufficient for your needs, then you must rewrite your date-time code.


The table introduced on the
[Overview](overview.html) page is a good place to begin evaluating which <tt>java.time</tt> classes meet your needs.


There is no one-to-one mapping correspondence between the two APIs, but the following table gives you a general idea of which functionality in the <tt>java.util</tt> date and time classes maps to the <tt>java.time</tt> APIs.
<th id="h1">java.util Functionality</th><th id="h2">java.time Functionality</th><th id="h3">Comments</th>
<td headers="h1"><tt>java.util.Date</tt></td><td headers="h2"><tt>java.time.Instant</tt></td><td headers="h3">The <tt>Instant</tt> and <tt>Date</tt> classes are similar. Each class:<br />- Represents an instantaneous point of time on the timeline (UTC)<br />- Holds a time independent of a time zone<br />- Is represented as epoch-seconds (since 1970-01-01T00:00:00Z) plus nanoseconds<br />The <tt>Date.from(Instant)</tt> and <tt>Date.toInstant()</tt> methods allow conversion between these classes.</td>
<td headers="h1"><tt>java.util.GregorianCalendar</tt></td><td headers="h2"><tt>java.time.ZonedDateTime</tt></td><td headers="h3">The <tt>ZonedDateTime</tt> class is the replacement for <tt>GregorianCalendar</tt>. It provides the following similar functionality.<br />Human time representation is as follows:<br />&#160;&#160;<tt>LocalDate</tt>: year, month, day<br />&#160;&#160;<tt>LocalTime</tt>: hours, minutes, seconds, nanoseconds<br />&#160;&#160;<tt>ZoneId</tt>: time zone<br />&#160;&#160;<tt>ZoneOffset</tt>: current offset from GMT<br />The <tt>GregorianCalendar.from(ZonedDateTime)</tt> and <tt>GregorianCalendar.to(ZonedDateTime)</tt> methods faciliate conversions between these classes.</td>
<td headers="h1"><tt>java.util.TimeZone</tt></td><td headers="h2"><tt>java.time.ZoneId</tt> or <tt>java.time.ZoneOffset</tt></td><td headers="h3">The <tt>ZoneId</tt> class specifies a time zone identifier and has access to the rules used each time zone. The <tt>ZoneOffset</tt> class specifies only an offset from Greenwich/UTC. For more information, see[Time Zone and Offset Classes](timezones.html).</td>
<td headers="h1"><tt>GregorianCalendar</tt> with the date set to <tt>1970-01-01</tt></td><td headers="h2"><tt>java.time.LocalTime</tt></td><td headers="h3">Code that sets the date to 1970-01-01 in a <tt>GregorianCalendar</tt> instance in order to use the time components can be replaced with an instance of <tt>LocalTime</tt>.</td>
<td headers="h1"><tt>GregorianCalendar</tt> with time set to <tt>00:00.</tt></td><td headers="h2"><tt>java.time.LocalDate</tt></td><td headers="h3">Code that sets the time to 00:00 in a <tt>GregorianCalendar</tt> instance in order to use the date components can be replaced with an instance of <tt>LocalDate</tt>. (This <tt>GregorianCalendar</tt> approach was flawed, as midnight does not occur in some countries once a year due to the transition to daylight saving time.)</td>

## Date and Time Formatting


Although the <tt>java.time.format.DateTimeFormatter</tt> provides a powerful mechanism for formatting date and time values, you can also use the <tt>java.time</tt> temporal-based classes directly with <tt>java.util.Formatter</tt> and <tt>String.format</tt>, using the same pattern-based formatting that you use with the <tt>java.util</tt> date and time classes.
