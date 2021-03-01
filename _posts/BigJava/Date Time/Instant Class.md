
# Instant Class


One of the core classes of the Date-Time API is the
[<tt>Instant</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html) class, which represents the start of a nanosecond on the timeline. This class is useful for generating a time stamp to represent machine time.

```

import java.time.Instant;

Instant timestamp = Instant.now();

```


A value returned from the <tt>Instant</tt> class counts time beginning from the first second of January 1, 1970 (<tt>1970-01-01T00:00:00Z</tt>) also called the
[<tt>EPOCH</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#EPOCH). An instant that occurs before the epoch has a negative value, and an instant that occurs after the epoch has a positive value.


The other constants provided by the <tt>Instant</tt> class are
[<tt>MIN</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#MIN), representing the smallest possible (far past) instant, and
[<tt>MAX</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#MAX), representing the largest (far future) instant.


Invoking <tt>toString</tt> on an <tt>Instant</tt> produces output like the following:

```

2013-05-30T23:38:23.085Z

```


This format follows the
[ISO-8601](http://www.iso.org/iso/home/standards/iso8601.htm) standard for representing date and time.


The <tt>Instant</tt> class provides a variety of methods for manipulating an <tt>Instant</tt>. There are <tt>plus</tt> and <tt>minus</tt> methods for adding or subtracting time. The following code adds 1 hour to the current time:

```

Instant oneHourLater = Instant.now().plusHours(1);

```


There are methods for comparing instants, such as
[<tt>isAfter</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#isAfter-java.time.Instant-) and
[<tt>isBefore</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#isBefore-java.time.Instant-). The 
[<tt>until</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html#until-java.time.temporal.Temporal-java.time.temporal.TemporalUnit-) method returns how much time exists between two <tt>Instant</tt> objects. The following line of code reports how many seconds have occurred since the beginning of the Java epoch.

```

long secondsFromEpoch = Instant.ofEpochSecond(0L).until(Instant.now(),
                        ChronoUnit.SECONDS);

```


The <tt>Instant</tt> class does not work with human units of time, such as years, months, or days. If you want to perform calculations in those units, you can convert an <tt>Instant</tt> to another class, such as <tt>LocalDateTime</tt> or <tt>ZonedDateTime</tt>, by binding the <tt>Instant</tt> with a time zone. You can then access the value in the desired units. The following code converts an <tt>Instant</tt> to a <tt>LocalDateTime</tt> object using the
[<tt>ofInstant</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html#ofInstant-java.time.Instant-java.time.ZoneId-) method and the default time zone, and then prints out the date and time in a more readable form:

```

Instant timestamp;
...
LocalDateTime ldt = LocalDateTime.ofInstant(timestamp, ZoneId.systemDefault());
System.out.printf("%s %d %d at %d:%d%n", ldt.getMonth(), ldt.getDayOfMonth(),
                  ldt.getYear(), ldt.getHour(), ldt.getMinute());

```


The output will be similar to the following:

```

MAY 30 2013 at 18:21

```


Either a <tt>ZonedDateTime</tt> or an <tt>OffsetTimeZone</tt> object can be converted to an <tt>Instant</tt> object, as each maps to an exact moment on the timeline. However, the reverse is not true. To convert an <tt>Instant</tt> object to a <tt>ZonedDateTime</tt> or an <tt>OffsetDateTime</tt> object requires supplying time zone, or time zone offset, information.
