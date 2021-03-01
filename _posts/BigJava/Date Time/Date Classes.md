
# Date Classes


The Date-Time API provides four classes that deal exclusively with date information, without respect to time or time zone. The use of these classes are suggested by the class names: <tt>LocalDate</tt>, <tt>YearMonth</tt>, <tt>MonthDay</tt>, and <tt>Year</tt>.

## LocalDate


A
[<tt>LocalDate</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html) represents a year-month-day in the ISO calendar and is useful for representing a date without a time. You might use a <tt>LocalDate</tt> to track a significant event, such as a birth date or wedding date. The following examples use the <tt>of</tt> and <tt>with</tt> methods to create instances of <tt>LocalDate</tt>:

```

LocalDate date = LocalDate.of(2000, Month.NOVEMBER, 20);
LocalDate nextWed = date.with(TemporalAdjusters.next(DayOfWeek.WEDNESDAY));

```


For more information about the <tt>TemporalAdjuster</tt> interface, see
[Temporal Adjuster](adjusters.html).


In addition to the usual methods, the <tt>LocalDate</tt> class offers getter methods for obtaining information about a given date. The
[<tt>getDayOfWeek</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#getDayOfWeek--) method returns the day of the week that a particular date falls on. For example, the following line of code returns "MONDAY":

```

DayOfWeek dotw = LocalDate.of(2012, Month.JULY, 9).getDayOfWeek();

```


The following example uses a <tt>TemporalAdjuster</tt> to retrieve the first Wednesday after a specific date.

```

LocalDate date = LocalDate.of(2000, Month.NOVEMBER, 20);
TemporalAdjuster adj = TemporalAdjusters.next(DayOfWeek.WEDNESDAY);
LocalDate nextWed = date.with(adj);
System.out.printf("For the date of %s, the next Wednesday is %s.%n",
                  date, nextWed);

```


Running the code produces the following:

```

For the date of 2000-11-20, the next Wednesday is 2000-11-22.

```


The 
[Period and Duration](period.html) section also has examples using the <tt>LocalDate</tt> class.

## YearMonth


The 
[<tt>YearMonth</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/YearMonth.html) class represents the month of a specific year. The following example uses the <tt>YearMonth.lengthOfMonth()</tt> method to determine the number of days for several year and month combinations.

```

YearMonth date = YearMonth.now();
System.out.printf("%s: %d%n", date, date.lengthOfMonth());

YearMonth date2 = YearMonth.of(2010, Month.FEBRUARY);
System.out.printf("%s: %d%n", date2, date2.lengthOfMonth());

YearMonth date3 = YearMonth.of(2012, Month.FEBRUARY);
System.out.printf("%s: %d%n", date3, date3.lengthOfMonth());

```


The output from this code looks like the following:

```

2013-06: 30
2010-02: 28
2012-02: 29

```

## MonthDay

The
[<tt>MonthDay</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/MonthDay.html) class represents the day of a particular month, such as New Year's Day on January 1.


The following example uses the 
[<tt>MonthDay.isValidYear</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/MonthDay.html#isValidYear-int-) method to determine if February 29 is valid for the year 2010. The call returns <tt>false</tt>, confirming that 2010 is not a leap year.

```

MonthDay date = MonthDay.of(Month.FEBRUARY, 29);
boolean validLeapYear = date.isValidYear(2010);

```

## Year


The
[<tt>Year</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Year.html) class represents a year. The following example uses the 
[<tt>Year.isLeap</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Year.html#isLeap--) method to determine if the given year is a leap year. The call returns <tt>true</tt>, confirming that 2012 is a leap year.

```

boolean validLeapYear = Year.of(2012).isLeap();

```
