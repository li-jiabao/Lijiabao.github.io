
# Temporal Adjuster


The 
[<tt>TemporalAdjuster</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAdjuster.html) interface, in the <tt>java.time.temporal</tt> package, provides methods that take a <tt>Temporal</tt> value and return an adjusted value. The adjusters can be used with any of the temporal-based types.


If an adjuster is used with a <tt>ZonedDateTime</tt>, then a new date is computed that preserves the original time and time zone values.

## Predefined Adjusters


The
[<tt>TemporalAdjusters</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAdjusters.html) class (note the plural) provides a set of predefined adjusters for finding the first or last day of the  month, the first or last day of the year, the last Wednesday of the month, or the first Tuesday after a specific date, to name a few examples. The predefined adjusters are defined as static methods and are designed to be used with the
[static import](../../java/package/usepkgs.html#staticimport) statement.


The following example uses several <tt>TemporalAdjusters</tt> methods, in conjunction with the <tt>with</tt> method defined in the temporal-based classes, to compute new dates based on the original date of 15 October 2000:

```

LocalDate date = LocalDate.of(2000, Month.OCTOBER, 15);
DayOfWeek dotw = date.getDayOfWeek();
System.out.printf("%s is on a %s%n", date, dotw);

System.out.printf("first day of Month: %s%n",
                  date.with(TemporalAdjusters.firstDayOfMonth()));
System.out.printf("first Monday of Month: %s%n",
                  date.with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY)));
System.out.printf("last day of Month: %s%n",
                  date.with(TemporalAdjusters.lastDayOfMonth()));
System.out.printf("first day of next Month: %s%n",
                  date.with(TemporalAdjusters.firstDayOfNextMonth()));
System.out.printf("first day of next Year: %s%n",
                  date.with(TemporalAdjusters.firstDayOfNextYear()));
System.out.printf("first day of Year: %s%n",
                  date.with(TemporalAdjusters.firstDayOfYear()));

```


This produces the following output:

```

2000-10-15 is on a SUNDAY
first day of Month: 2000-10-01
first Monday of Month: 2000-10-02
last day of Month: 2000-10-31
first day of next Month: 2000-11-01
first day of next Year: 2001-01-01
first day of Year: 2000-01-01

```

## Custom Adjusters


You can also create your own custom adjuster. To do this, you create a class that implements the <tt>TemporalAdjuster</tt> interface with a
[<tt>adjustInto(Temporal)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAdjuster.html#adjustInto-java.time.temporal.Temporal-) method. The
[`<tt>PaydayAdjuster</tt>`](examples/PaydayAdjuster.java) class from the
[`<tt>NextPayday</tt>`](examples/NextPayday.java) example is a custom adjuster. The <tt>PaydayAdjuster</tt> evaluates the passed-in date and returns the next payday, assuming that payday occurs twice a month: on the 15th, and again on the last day of the month. If the computed date occurs on a weekend, then the previous Friday is used. The current calendar year is assumed.

```

/**
 * The adjustInto method accepts a Temporal instance 
 * and returns an adjusted LocalDate. If the passed in
 * parameter is not a LocalDate, then a DateTimeException is thrown.
 */
public Temporal adjustInto(Temporal input) {
    LocalDate date = LocalDate.from(input);
    int day;
    if (date.getDayOfMonth() &lt; 15) {
        day = 15;
    } else {
        day = date.with(TemporalAdjusters.lastDayOfMonth()).getDayOfMonth();
    }
    date = date.withDayOfMonth(day);
    if (date.getDayOfWeek() == DayOfWeek.SATURDAY ||
        date.getDayOfWeek() == DayOfWeek.SUNDAY) {
        date = date.with(TemporalAdjusters.previous(DayOfWeek.FRIDAY));
    }

    return input.with(date);
}

```


The adjuster is invoked in the same manner as a predefined adjuster, using the <tt>with</tt> method. The following line of code is from the <tt>NextPayday</tt> example:

```

LocalDate nextPayday = date.with(new PaydayAdjuster());

```


In 2013, both June 15 and June 30 occur on the weekend. Running the <tt>NextPayday</tt> example with the respective dates of June 3 and June 18 (in 2013), gives the following results:

```

Given the date:  2013 Jun 3
the next payday: 2013 Jun 14

Given the date:  2013 Jun 18
the next payday: 2013 Jun 28

```
