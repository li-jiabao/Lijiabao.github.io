
# Period and Duration


When you write code to specify an amount of time, use the class or method that best meets your needs: the
[<tt>Duration</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Duration.html) class,
[<tt>Period</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html) class, or the
[<tt>ChronoUnit.between</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/ChronoUnit.html#between-java.time.temporal.Temporal-java.time.temporal.Temporal-) method. A <tt>Duration</tt> measures an amount of time using time-based values (seconds, nanoseconds). A <tt>Period</tt> uses date-based values (years, months, days).

## Duration


A <tt>Duration</tt> is most suitable in situations that measure machine-based time, such as code that uses an <tt>Instant</tt> object. A <tt>Duration</tt> object is measured in seconds or nanoseconds and does not use date-based constructs such as years, months, and days, though the class provides methods that convert to days, hours, and minutes. A <tt>Duration</tt> can have a negative value, if it is created with an end point that occurs before the start point.


The following code calculates, in nanoseconds, the duration between two instants:

```

Instant t1, t2;
...
long ns = Duration.between(t1, t2).toNanos();

```


The following code adds 10 seconds to an <tt>Instant</tt>:

```

Instant start;
...
Duration gap = Duration.ofSeconds(10);
Instant later = start.plus(gap);

```


A <tt>Duration</tt> is not connected to the timeline, in that it does not track time zones or daylight saving time. Adding a <tt>Duration</tt> equivalent to 1 day to a <tt>ZonedDateTime</tt> results in exactly 24 hours being added, regardless of daylight saving time or other time differences that might result.

## ChronoUnit


The <tt>ChronoUnit</tt> enum, discussed in the
[The Temporal Package](temporal.html), defines the units used to measure time. The <tt>ChronoUnit.between</tt> method is useful when you want to measure an amount of time in a single unit of time only, such as days or seconds. The <tt>between</tt> method works with all temporal-based objects, but it returns the amount in a single unit only. The following code calculates the gap, in milliseconds, between two time-stamps:

```

import java.time.Instant;
import java.time.temporal.Temporal;
import java.time.temporal.ChronoUnit;

Instant previous, current, gap;
...
current = Instant.now();
if (previous != null) {
    gap = ChronoUnit.MILLIS.between(previous,current);
}
...

```

## Period


To define an amount of time with date-based values (years, months, days), use the
[<tt>Period</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html) class. The <tt>Period</tt> class provides various <tt>get</tt> methods, such as
[<tt>getMonths</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html#getMonths--),
[<tt>getDays</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html#getDays--), and
[<tt>getYears</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html#getYears--), so that you can extract the amount of time from the period.


The total period of time is represented by all three units together: months, days, and years. To present the amount of time measured in a single unit of time, such as days, you can use the <tt>ChronoUnit.between</tt> method.


The following code reports how old you are, assuming that you were born on January 1, 1960. The <tt>Period</tt> class is used to determine the time in years, months, and days. The same period, in total days, is determined by using the <tt>ChronoUnit.between</tt> method and is displayed in parentheses:

```

LocalDate today = LocalDate.now();
LocalDate birthday = LocalDate.of(1960, Month.JANUARY, 1);

Period p = Period.between(birthday, today);
long p2 = ChronoUnit.DAYS.between(birthday, today);
System.out.println("You are " + p.getYears() + " years, " + p.getMonths() +
                   " months, and " + p.getDays() +
                   " days old. (" + p2 + " days total)");

```


The code produces output similar to the following:

```

You are 53 years, 4 months, and 29 days old. (19508 days total)

```


To calculate how long it is until your next birthday, you could use the following code from the 
[`<tt>Birthday</tt>`](examples/Birthday.java) example. The <tt>Period</tt> class is used to determine the value in months and days. The <tt>ChronoUnit.between</tt> method returns the value in total days and is displayed in parentheses.

```

LocalDate birthday = LocalDate.of(1960, Month.JANUARY, 1);

LocalDate nextBDay = birthday.withYear(today.getYear());

//If your birthday has occurred this year already, add 1 to the year.
if (nextBDay.isBefore(today) || nextBDay.isEqual(today)) {
    nextBDay = nextBDay.plusYears(1);
}

Period p = Period.between(today, nextBDay);
long p2 = ChronoUnit.DAYS.between(today, nextBDay);
System.out.println("There are " + p.getMonths() + " months, and " +
                   p.getDays() + " days until your next birthday. (" +
                   p2 + " total)");

```


The code produces output similar to the following:

```

There are 7 months, and 2 days until your next birthday. (216 total)

```


These calculations do not account for time zone differences. If you were, for example, born in Australia, but currently live in Bangalore, this slightly affects the calculation of your exact age. In this situation, use a <tt>Period</tt> in conjunction with the <tt>ZonedDateTime</tt> class. When you add a <tt>Period</tt> to a <tt>ZonedDateTime</tt>, the time differences are observed.
