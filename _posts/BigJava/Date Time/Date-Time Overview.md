
# Lesson: Date-Time Overview


Time seems to be a simple subject; even an inexpensive watch can provide a reasonably accurate date and time. However, with closer examination, you realize the subtle complexities and many factors that affect your understanding of time. For example, the result of adding one month to January 31 is different for a leap year than for other years. Time zones also add complexity. For example, a country may go in and out of daylight saving time at short notice, or more than once a year or it may skip daylight saving time entirely for a given year.


The Date-Time API uses the calendar system defined in
[ISO-8601](http://www.iso.org/iso/home/standards/iso8601.htm) as the default calendar. This calendar is based on the Gregorian calendar system and is used globally as the defacto standard for representing date and time. The core classes in the Date-Time API have names such as <tt>LocalDateTime</tt>, <tt>ZonedDateTime</tt>, and <tt>OffsetDateTime</tt>. All of these use the ISO calendar system. If you want to use an alternative calendar system, such as Hijrah or Thai Buddhist, the <tt>java.time.chrono</tt> package allows you to use one of the predefined calendar systems. Or you can create your own.


The Date-Time API uses the
[Unicode Common Locale Data Repository (CLDR)](http://cldr.unicode.org).  This repository supports the world's languages and contains the world's largest collection of locale data available. The information in this repository has been localized to hundreds of languages. The Date-Time API also uses the
[Time-Zone Database (TZDB)](http://www.iana.org/time-zones). This database provides information about every time zone change globally since 1970, with history for primary time zones since the concept was introduced.
