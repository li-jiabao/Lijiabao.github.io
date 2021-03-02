
# Clock


Most temporal-based objects provide a no-argument <tt>now()</tt> method that provides the current date and time using the system clock and the default time zone. These temporal-based objects also provide a one-argument <tt>now(Clock)</tt> method that allows you to pass in an alternative
[<tt>Clock</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html).


The current date and time depends on the time-zone and, for globalized applications, a <tt>Clock</tt> is necessary to ensure that the date/time is created with the correct time-zone. So, although the use of the <tt>Clock</tt> class is optional, this feature allows you to test your code for other time zones, or by using a fixed clock, where time does not change.


The <tt>Clock</tt> class is abstract, so you cannot create an instance of it. The following factory methods can be useful for testing.

<li>
[<tt>Clock.offset(Clock, Duration)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html#offset-java.time.Clock-java.time.Duration-) returns a clock that is offset by the specified <tt>Duration</tt>.
</li>
<li>
[<tt>Clock.systemUTC()</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html#systemUTC--) returns a clock representing the Greenwich/UTC time zone.
</li>
<li>
[<tt>Clock.fixed(Instant, ZoneId)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html#fixed-java.time.Instant-java.time.ZoneId-) always returns the same <tt>Instant</tt>. For this clock, time stands still.
</li>
