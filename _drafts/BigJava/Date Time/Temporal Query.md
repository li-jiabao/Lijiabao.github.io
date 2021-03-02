
# Temporal Query


A 
[<tt>TemporalQuery</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQuery.html) can be used to retrieve information from a temporal-based object.

## Predefined Queries


The
[<tt>TemporalQueries</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQueries.html) class (note the plural) provides several predefined queries, including methods that are useful when the application cannot identify the type of temporal-based object. As with the adjusters, the predefined queries are defined as static methods and are designed to be used with the
[static import](../../java/package/usepkgs.html#staticimport) statement.


The
[<tt>precision</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQueries.html#precision--) query, for example, returns the smallest <tt>ChronoUnit</tt> that can be returned by a particular temporal-based object. The following example uses the <tt>precision</tt> query on several types of temporal-based objects:

```

TemporalQueries query = TemporalQueries.precision();
System.out.printf("LocalDate precision is %s%n",
                  LocalDate.now().query(query));
System.out.printf("LocalDateTime precision is %s%n",
                  LocalDateTime.now().query(query));
System.out.printf("Year precision is %s%n",
                  Year.now().query(query));
System.out.printf("YearMonth precision is %s%n",
                  YearMonth.now().query(query));
System.out.printf("Instant precision is %s%n",
                  Instant.now().query(query));

```

The output looks like the following:

```

LocalDate precision is Days
LocalDateTime precision is Nanos
Year precision is Years
YearMonth precision is Months
Instant precision is Nanos

```

## Custom Queries


You can also create your own custom queries. One way to do this is to create a class that implements the <tt>TemporalQuery</tt> interface with the 
[<tt>queryFrom(TemporalAccessor)</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQuery.html#queryFrom-java.time.temporal.TemporalAccessor-) method. The
[`<tt>CheckDate</tt>`](examples/CheckDate.java) example implements two custom queries. The first custom query can be found in the 
[`<tt>FamilyVacations</tt>`](examples/FamilyVacations.java) class, which implements the 
[<tt>TemporalQuery</tt>](https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalQuery.html) interface. The <tt>queryFrom</tt> method compares the passed-in date against scheduled vacation dates and returns <tt>TRUE</tt> if it falls within those date ranges.

```

// Returns true if the passed-in date occurs during one of the
// family vacations. Because the query compares the month and day only,
// the check succeeds even if the Temporal types are not the same.
public Boolean queryFrom(TemporalAccessor date) {
    int month = date.get(ChronoField.MONTH_OF_YEAR);
    int day   = date.get(ChronoField.DAY_OF_MONTH);

    // Disneyland over Spring Break
    if ((month == Month.APRIL.getValue()) &amp;&amp; ((day &gt;= 3) &amp;&amp; (day &lt;= 8)))
        return Boolean.TRUE;

    // Smith family reunion on Lake Saugatuck
    if ((month == Month.AUGUST.getValue()) &amp;&amp; ((day &gt;= 8) &amp;&amp; (day &lt;= 14)))
        return Boolean.TRUE;

    return Boolean.FALSE;
}

```


The second custom query is implemented in the 
[`<tt>FamilyBirthdays</tt>`](examples/FamilyBirthdays.java) class. This class provides an <tt>isFamilyBirthday</tt> method that compares the passed-in date against several birthdays and returns <tt>TRUE</tt> if there is a match.

```

// Returns true if the passed-in date is the same as one of the
// family birthdays. Because the query compares the month and day only,
// the check succeeds even if the Temporal types are not the same.
public static Boolean isFamilyBirthday(TemporalAccessor date) {
    int month = date.get(ChronoField.MONTH_OF_YEAR);
    int day   = date.get(ChronoField.DAY_OF_MONTH);

    // Angie's birthday is on April 3.
    if ((month == Month.APRIL.getValue()) &amp;&amp; (day == 3))
        return Boolean.TRUE;

    // Sue's birthday is on June 18.
    if ((month == Month.JUNE.getValue()) &amp;&amp; (day == 18))
        return Boolean.TRUE;

    // Joe's birthday is on May 29.
    if ((month == Month.MAY.getValue()) &amp;&amp; (day == 29))
        return Boolean.TRUE;

    return Boolean.FALSE;
}

```


The <tt>FamilyBirthday</tt> class does not implement the <tt>TemporalQuery</tt> interface and can be used as part of a
[lambda expression](../../java/javaOO/lambdaexpressions.html). The following code, from the <tt>CheckDate</tt> example, shows how to invoke both custom queries.

```

// Invoking the query without using a lambda expression.
Boolean isFamilyVacation = date.query(new FamilyVacations());

// Invoking the query using a lambda expression.
Boolean isFamilyBirthday = date.query(FamilyBirthdays::isFamilyBirthday);

if (isFamilyVacation.booleanValue() || isFamilyBirthday.booleanValue())
    System.out.printf("%s is an important date!%n", date);
else
    System.out.printf("%s is not an important date.%n", date);

```
