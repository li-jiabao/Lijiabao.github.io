
# Time Zone and Offset Classes


A **time zone** is a region of the earth where the same standard time is used. Each time zone is described by an identifier and usually has the format **region/city** (<tt>Asia/Tokyo</tt>) and an offset from Greenwich/UTC time. For example, the offset for Tokyo is <tt>+09:00</tt>.

## ZoneId and ZoneOffset


The Date-Time API provides two classes for specifying a time zone or an offset:

- <tt>ZoneId</tt> specifies a time zone identifier and provides rules for converting between an <tt>Instant</tt> and a <tt>LocalDateTime</tt>.
- <tt>ZoneOffset</tt> specifies a time zone offset from Greenwich/UTC time.


Offsets from Greenwich/UTC time are usually defined in whole hours, but there are exceptions. The following code, from the
[`<tt>TimeZoneId</tt>`](examples/TimeZoneId.java) example, prints a list of all time zones that use offsets from Greenwich/UTC that are not defined in whole hours.

```

Set&lt;String&gt; allZones = ZoneId.getAvailableZoneIds();
LocalDateTime dt = LocalDateTime.now();

// Create a List using the set of zones and sort it.
List&lt;String&gt; zoneList = new ArrayList&lt;String&gt;(allZones);
Collections.sort(zoneList);

...

for (String s : zoneList) {
    ZoneId zone = ZoneId.of(s);
    ZonedDateTime zdt = dt.atZone(zone);
    ZoneOffset offset = zdt.getOffset();
    int secondsOfHour = offset.getTotalSeconds() % (60 * 60);
    String out = String.format("%35s %10s%n", zone, offset);

    // Write only time zones that do not have a whole hour offset
    // to standard out.
    if (secondsOfHour != 0) {
        System.out.printf(out);
    }
    ...
}

```


This example prints the following list to standard out:

```

      America/Caracas     -04:30
     America/St_Johns     -02:30
        Asia/Calcutta     +05:30
         Asia/Colombo     +05:30
           Asia/Kabul     +04:30
       Asia/Kathmandu     +05:45
        Asia/Katmandu     +05:45
         Asia/Kolkata     +05:30
         Asia/Rangoon     +06:30
          Asia/Tehran     +04:30
   Australia/Adelaide     +09:30
Australia/Broken_Hill     +09:30
     Australia/Darwin     +09:30
      Australia/Eucla     +08:45
        Australia/LHI     +10:30
  Australia/Lord_Howe     +10:30
      Australia/North     +09:30
      Australia/South     +09:30
 Australia/Yancowinna     +09:30
  Canada/Newfoundland     -02:30
         Indian/Cocos     +06:30
                 Iran     +04:30
              NZ-CHAT     +12:45
      Pacific/Chatham     +12:45
    Pacific/Marquesas     -09:30
      Pacific/Norfolk     +11:30

```


The <tt>TimeZoneId</tt> example also prints a list of all time zone IDs to a file called 
[`<tt>timeZones</tt>`](examples/timeZones).

## The Date-Time Classes


The Date-Time API provides three temporal-based classes that work with time zones:

- <tt>ZonedDateTime</tt> handles a date and time with a corresponding time zone with a time zone offset from Greenwich/UTC.
- <tt>OffsetDateTime</tt> handles a date and time with a corresponding time zone offset from Greenwich/UTC, without a time zone ID.
- <tt>OffsetTime</tt> handles time with a corresponding time zone offset from Greenwich/UTC, without a time zone ID.


When would you use <tt>OffsetDateTime</tt> instead of <tt>ZonedDateTime</tt>? If you are writing complex software that models its own rules for date and time calculations based on geographic locations, or if you are storing time-stamps in a database that track only absolute offsets from Greenwich/UTC time, then you might want to use <tt>OffsetDateTime</tt>. Also, XML and other network formats define date-time transfer as <tt>OffsetDateTime</tt> or <tt>OffsetTime</tt>.


Although all three classes maintain an offset from Greenwich/UTC time, only <tt>ZonedDateTime</tt> uses the
[<tt>ZoneRules</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/zone/ZoneRules.html), part of the <tt>java.time.zone</tt> package, to determine how an offset varies for a particular time zone. For example, most time zones experience a gap (typically of 1 hour) when moving the clock forward to daylight saving time, and a time overlap when moving the clock back to standard time and the last hour before the transition is repeated. The <tt>ZonedDateTime</tt> class accommodates this scenario, whereas the <tt>OffsetDateTime</tt> and <tt>OffsetTime</tt> classes, which do not have access to the <tt>ZoneRules</tt>, do not.

### ZonedDateTime


The 
[ZonedDateTime](https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html) class, in effect, combines the
[<tt>LocalDateTime</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html) class with the
[<tt>ZoneId</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/ZoneId.html) class. It is used to represent a full date (year, month, day) and time (hour, minute, second, nanosecond) with a time zone (region/city, such as <tt>Europe/Paris</tt>).


The following code, from the
[`<tt>Flight</tt>`](examples/Flight.java) example, defines the departure time for a flight from San Francisco to Tokyo as a <tt>ZonedDateTime</tt> in the America/Los Angeles time zone.  The <tt>withZoneSameInstant</tt> and <tt>plusMinutes</tt> methods are used to create an instance of <tt>ZonedDateTime</tt> that represents the projected arrival time in Tokyo, after the 650 minute flight. The <tt>ZoneRules.isDaylightSavings</tt> method determines whether it is daylight saving time when the flight arrives in Tokyo.


A <tt>DateTimeFormatter</tt> object is used to format the <tt>ZonedDateTime</tt> instances for printing:

```

DateTimeFormatter format = DateTimeFormatter.ofPattern("MMM d yyyy  hh:mm a");

// Leaving from San Francisco on July 20, 2013, at 7:30 p.m.
LocalDateTime leaving = LocalDateTime.of(2013, Month.JULY, 20, 19, 30);
ZoneId leavingZone = ZoneId.of("America/Los_Angeles"); 
ZonedDateTime departure = ZonedDateTime.of(leaving, leavingZone);

try {
    String out1 = departure.format(format);
    System.out.printf("LEAVING:  %s (%s)%n", out1, leavingZone);
} catch (DateTimeException exc) {
    System.out.printf("%s can't be formatted!%n", departure);
    throw exc;
}

// Flight is 10 hours and 50 minutes, or 650 minutes
ZoneId arrivingZone = ZoneId.of("Asia/Tokyo"); 
<strong>ZonedDateTime arrival = departure.withZoneSameInstant(arrivingZone)
                                 .plusMinutes(650);</strong>

try {
    String out2 = arrival.format(format);
    System.out.printf("ARRIVING: %s (%s)%n", out2, arrivingZone);
} catch (DateTimeException exc) {
    System.out.printf("%s can't be formatted!%n", arrival);
    throw exc;
}

if (arrivingZone.getRules().isDaylightSavings(arrival.toInstant())) 
    System.out.printf("  (%s daylight saving time will be in effect.)%n",
                      arrivingZone);
else
    System.out.printf("  (%s standard time will be in effect.)%n",
                      arrivingZone);

```


This produces the following output:

```

LEAVING:  Jul 20 2013  07:30 PM (America/Los_Angeles)
ARRIVING: Jul 21 2013  10:20 PM (Asia/Tokyo)
  (Asia/Tokyo standard time will be in effect.)

```

### OffsetDateTime


The 
[OffsetDateTime](https://docs.oracle.com/javase/8/docs/api/java/time/OffsetDateTime.html) class, in effect, combines the 
[<tt>LocalDateTime</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html) class with the
[<tt>ZoneOffset</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/ZoneOffset.html) class. It is used to represent a full date (year, month, day) and time (hour, minute, second, nanosecond) with an offset from Greenwich/UTC time (+/-hours:minutes, such as <tt>+06:00</tt> or <tt>-08:00</tt>).


The following example uses <tt>OffsetDateTime</tt> with the <tt>TemporalAdjuster.lastDay</tt> method to find the last Thursday in July 2013.

```

// Find the last Thursday in July 2013.
LocalDateTime localDate = LocalDateTime.of(2013, Month.JULY, 20, 19, 30);
ZoneOffset offset = ZoneOffset.of("-08:00");

OffsetDateTime offsetDate = OffsetDateTime.of(localDate, offset);
OffsetDateTime lastThursday =
        offsetDate.with(TemporalAdjusters.lastInMonth(DayOfWeek.THURSDAY));
System.out.printf("The last Thursday in July 2013 is the %sth.%n",
                   lastThursday.getDayOfMonth());

```


The output from running this code is:

```

The last Thursday in July 2013 is the 25th.

```

### OffsetTime


The 
[OffsetTime](https://docs.oracle.com/javase/8/docs/api/java/time/OffsetTime.html) class, in effect, combines the
[<tt>LocalTime</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalTime.html) class with the
[<tt>ZoneOffset</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/ZoneOffset.html) class. It is used to represent time (hour, minute, second, nanosecond) with an offset from Greenwich/UTC time (+/-hours:minutes, such as <tt>+06:00</tt> or <tt>-08:00</tt>).


The <tt>OffsetTime</tt> class is used in the same situations as the <tt>OffsetDateTime</tt> class, but when tracking the date is not needed.
