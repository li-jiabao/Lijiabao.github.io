
# DayOfWeek and Month Enums


The Date-Time API provides enums for specifying days of the week and months of the year.

## DayOfWeek


The 
[<tt>DayOfWeek</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/DayOfWeek.html) enum consists of seven constants that describe the days of the week: <tt>MONDAY</tt> through <tt>SUNDAY</tt>. The integer values of the <tt>DayOfWeek</tt> constants range from 1 (Monday) through 7 (Sunday). Using the defined constants (<tt>DayOfWeek.FRIDAY</tt>) makes your code more readable.


This enum also provides a number of methods, similar to the methods provided by the temporal-based classes. For example, the following code adds 3 days to "Monday" and prints the result. The output is "THURSDAY":

```

System.out.printf("%s%n", DayOfWeek.MONDAY.plus(3));

```


By using the
[<tt>getDisplayName(TextStyle, Locale)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/DayOfWeek.html#getDisplayName-java.time.format.TextStyle-java.util.Locale-) method, you can retrieve a string to identify the day of the week in the user's locale. The
[<tt>TextStyle</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/format/TextStyle.html) enum enables you to specify what sort of string you want to display: <tt>FULL</tt>, <tt>NARROW</tt> (typically a single letter), or <tt>SHORT</tt> (an abbreviation). The <tt>STANDALONE</tt> <tt>TextStyle</tt> constants are used in some languages where the output is different when used as part of a date than when it is used by itself. The following example prints the three primary forms of the <tt>TextStyle</tt> for "Monday":

```

DayOfWeek dow = DayOfWeek.MONDAY;
Locale locale = Locale.getDefault();
System.out.println(dow.getDisplayName(TextStyle.FULL, locale));
System.out.println(dow.getDisplayName(TextStyle.NARROW, locale));
System.out.println(dow.getDisplayName(TextStyle.SHORT, locale));

```


This code has the following output for the <tt>en</tt> locale:

```

Monday
M
Mon

```

## Month


The
[<tt>Month</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Month.html) enum includes constants for the twelve months, <tt>JANUARY</tt> through <tt>DECEMBER</tt>. As with the <tt>DayOfWeek</tt> enum, the <tt>Month</tt> enum is strongly typed, and the integer value of each constant corresponds to the ISO range from 1 (January) through 12 (December). Using the defined constants (<tt>Month.SEPTEMBER</tt>) makes your code more readable.


The <tt>Month</tt> enum also includes a number of methods. The following line of code uses the <tt>maxLength</tt> method to print the maximum possible number of days in the month of February. The output is "29":

```

System.out.printf("%d%n", Month.FEBRUARY.maxLength());

```


The <tt>Month</tt> enum also implements the
[<tt>getDisplayName(TextStyle, Locale)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/Month.html#getDisplayName-java.time.format.TextStyle-java.util.Locale-) method to retrieve a string to identify the month in the user's locale using the specified <tt>TextStyle</tt>. If a particular <tt>TextStyle</tt> is not defined, then a string representing the numeric value of the constant is returned. The following code prints the month of August using the three primary text styles:

```

Month month = Month.AUGUST;
Locale locale = Locale.getDefault();
System.out.println(month.getDisplayName(TextStyle.FULL, locale));
System.out.println(month.getDisplayName(TextStyle.NARROW, locale));
System.out.println(month.getDisplayName(TextStyle.SHORT, locale));

```


This code has the following output for the <tt>en</tt> locale:

```

August
A
Aug

```
