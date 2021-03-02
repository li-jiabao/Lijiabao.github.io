
# Lesson: Standard Calendar


The core of the Date-Time API is the
[java.time](https://docs.oracle.com/javase/8/docs/api/java/time/package-summary.html) package. The classes defined in <tt>java.time</tt> base their calendar system on the ISO calendar, which is the world standard for representing date and time. The ISO calendar follows the proleptic Gregorian rules. The Gregorian calendar was introduced in 1582; in the **proleptic** Gregorian calendar, dates are extended backwards from that time to create a consistent, unified timeline and to simplify date calculations.


This lesson covers the following topics:

## 
[Overview](overview.html)


This section compares the concepts of human time and machine time provides a table of the primary temporal-based classes in the <tt>java.time</tt> package.

## 
[DayOfWeek and Month Enums](enum.html)


This section discusses the enum that defines the days of the week (<tt>DayOfWeek</tt>) and the enum that defines months (<tt>Month</tt>).

## 
[Date Classes](date.html)


This section shows the temporal-based classes that deal only with dates, without time or time zones. The four classes are <tt>LocalDate</tt>, <tt>YearMonth</tt>, <tt>MonthDay</tt> and <tt>Year</tt>.

## 
[Date and Time Classes](datetime.html)


This section presents the <tt>LocalTime</tt> and <tt>LocalDateTime</tt> classes, which deal with time, and date and time, respectively, but without time zones.

## 
[Time Zone and Offset Classes](timezones.html)


This section discusses the temporal-based classes that store time zone (or time zone offset) information, <tt>ZonedDateTime</tt>, <tt>OffsetDateTime</tt>, and <tt>OffsetTime</tt>. The supporting classes, <tt>ZoneId</tt>, <tt>ZoneRules</tt>, and <tt>ZoneOffset</tt>, are also discussed.

## 
[Instant Class](instant.html)


This section discusses the <tt>Instant</tt> class, which represents an instantaneous moment on the timeline.

## 
[Parsing and Formatting](format.html)


This section provides an overview of how to use the predefined formatters to format and parse date and time values.

## 
[The Temporal Package](temporal.html)


This section presents an overview of the <tt>java.time.temporal</tt> package, which supports the temporal classes, fields (<tt>TemporalField</tt> and <tt>ChronoField</tt>) and units (<tt>TemporalUnit</tt> and <tt>ChronoUnit</tt>). This section also explains how to use a temporal adjuster to retrieve an adjusted time value, such as "the first Tuesday after April 11", and how to perform a temporal query.

## 
[Period and Duration](period.html)


This section explains how to calculate an amount of time, using both the <tt>Period</tt> and <tt>Duration</tt> classes, as well as the <tt>ChronoUnit.between</tt> method.

## 
[Clock](clock.html)


This section provides a brief overview of the <tt>Clock</tt> class. You can use this class to provide an alternative clock to the system clock.

## 
[Non-ISO Date Conversion](nonIso.html)


This section explains how to convert from a date in the ISO calendar system to a date in a non-ISO calendar system, such as a <tt>JapaneseDate</tt> or a <tt>ThaiBuddhistDate</tt>.

## 
[Legacy Date-Time Code](legacy.html)


This section offers some tips on how to convert older <tt>java.util.Date</tt> and <tt>java.util.Calendar</tt> code to the Date-Time API.

## 
[Summary](summary.html)


This section provides a summary of the Standard Calendar lesson.
