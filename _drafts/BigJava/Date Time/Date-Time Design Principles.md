
# Date-Time Design Principles


The Date-Time API was developed using several design principles.

## Clear


The methods in the API are well defined and their behavior is clear and expected. For example, invoking a Date-Time method with a <tt>null</tt> parameter value typically triggers a <tt>NullPointerException</tt>.

## Fluent


The Date-Time API provides a fluent interface, making the code easy to read. Because most methods do not allow parameters with a <tt>null</tt> value and do not return a <tt>null</tt> value, method calls can be chained together and the resulting code can be quickly understood. For example:

```

LocalDate today = LocalDate.now();
LocalDate payday = today.with(TemporalAdjusters.lastDayOfMonth()).minusDays(2);

```

## Immutable


Most of the classes in the Date-Time API create objects that are
[immutable](../../essential/concurrency/immutable.html), meaning that, after the object is created, it cannot be modified. To alter the value of an immutable object, a new object must be constructed as a modified copy of the original. This also means that the Date-Time API is, by definition, thread-safe. This affects the API in that most of the methods used to create date or time objects are prefixed with `of`, `from`, or `with`, rather than constructors, and there are no `set` methods. For example:

```

LocalDate dateOfBirth = LocalDate.of(2012, Month.MAY, 14);
LocalDate firstBirthday = dateOfBirth.plusYears(1);

```

## Extensible


The Date-Time API is extensible wherever possible. For example, you can define your own time adjusters and queries, or build your own calendar system.
