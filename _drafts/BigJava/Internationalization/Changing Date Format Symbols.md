
# Changing Date Format Symbols

The `format` method of the `SimpleDateFormat` class returns a `String` composed of digits and symbols. For example, in the `String` "Friday, April 10, 2009," the symbols are "Friday" and "April." If the symbols encapsulated in `SimpleDateFormat` don't meet your needs, you can change them with the 
[`DateFormatSymbols`](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormatSymbols.html). You can change symbols that represent names for months, days of the week, and time zones, among others. The following table lists the `DateFormatSymbols` methods that allow you to modify the symbols:
<th id="h1">Setter Method</th><th id="h2">Example of a Symbol the Method Modifies</th>
<td headers="h1">`setAmPmStrings`</td><td headers="h2">PM</td>
<td headers="h1">`setEras`</td><td headers="h2">AD</td>
<td headers="h1">`setMonths`</td><td headers="h2">December</td>
<td headers="h1">`setShortMonths`</td><td headers="h2">Dec</td>
<td headers="h1">`setShortWeekdays`</td><td headers="h2">Tue</td>
<td headers="h1">`setWeekdays`</td><td headers="h2">Tuesday</td>
<td headers="h1">`setZoneStrings`</td><td headers="h2">PST</td>

The following example invokes `setShortWeekdays` to change the short names of the days of the week from lowercase to uppercase characters. The full source code for this example is in 
[`DateFormatSymbolsDemo`](examples/DateFormatSymbolsDemo.java). The first element in the array argument of `setShortWeekdays` is a null `String`. Therefore the array is one-based rather than zero-based. The `SimpleDateFormat` constructor accepts the modified `DateFormatSymbols` object as an argument. Here is the source code:

```

Date today;
String result;
SimpleDateFormat formatter;
DateFormatSymbols symbols;
String[] defaultDays;
String[] modifiedDays;

symbols = new DateFormatSymbols( new Locale("en", "US"));
defaultDays = symbols.getShortWeekdays();

for (int i = 0; i &lt; defaultDays.length; i++) {
    System.out.print(defaultDays[i] + " ");
}
System.out.println();

String[] capitalDays = {
    "", "SUN", "MON",
    "TUE", "WED", "THU",
    "FRI", "SAT"
};
symbols.setShortWeekdays(capitalDays);

modifiedDays = symbols.getShortWeekdays();
for (int i = 0; i &lt; modifiedDays.length; i++) {
    System.out.print(modifiedDays[i] + " ");
}
System.out.println();
System.out.println();

formatter = new SimpleDateFormat("E", symbols);
today = new Date();
result = formatter.format(today);
System.out.println("Today's day of the week: " + result);

```

The preceding code generates this output:

```

 Sun Mon Tue Wed Thu Fri Sat 
 SUN MON TUE WED THU FRI SAT 

Today's day of the week: MON

```
