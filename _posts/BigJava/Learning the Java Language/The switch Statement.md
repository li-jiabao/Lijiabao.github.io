
# The switch Statement

Unlike `if-then` and `if-then-else` statements, the `switch` statement can have a number of possible execution paths. A `switch` works with the `byte`, `short`, `char`, and `int` primitive data types. It also works with *enumerated types* (discussed in 
[Enum Types](../javaOO/enum.html)), the 
[`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) class, and a few special classes that wrap certain primitive types: 
[`Character`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html), 
[`Byte`](https://docs.oracle.com/javase/8/docs/api/java/lang/Byte.html), 
[`Short`](https://docs.oracle.com/javase/8/docs/api/java/lang/Short.html), and 
[`Integer`](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html) (discussed in 
[Numbers and Strings](../data/index.html)).

The following code example, 
[`SwitchDemo`](examples/SwitchDemo.java), declares an `int` named `month` whose value represents a month. The code displays the name of the month, based on the value of `month`, using the `switch` statement.

```


public class SwitchDemo {
    public static void main(String[] args) {

        int month = 8;
        String monthString;
        switch (month) {
            case 1:  monthString = "January";
                     break;
            case 2:  monthString = "February";
                     break;
            case 3:  monthString = "March";
                     break;
            case 4:  monthString = "April";
                     break;
            case 5:  monthString = "May";
                     break;
            case 6:  monthString = "June";
                     break;
            case 7:  monthString = "July";
                     break;
            case 8:  monthString = "August";
                     break;
            case 9:  monthString = "September";
                     break;
            case 10: monthString = "October";
                     break;
            case 11: monthString = "November";
                     break;
            case 12: monthString = "December";
                     break;
            default: monthString = "Invalid month";
                     break;
        }
        System.out.println(monthString);
    }
}

```

In this case, `August` is printed to standard output.

The body of a `switch` statement is known as a *switch block*. A statement in the `switch` block can be labeled with one or more `case` or `default` labels. The `switch` statement evaluates its expression, then executes all statements that follow the matching `case` label.

You could also display the name of the month with `if-then-else` statements:

```

int month = 8;
if (month == 1) {
    System.out.println("January");
} else if (month == 2) {
    System.out.println("February");
}
...  // and so on

```

Deciding whether to use `if-then-else` statements or a `switch` statement is based on readability and the expression that the statement is testing. An `if-then-else` statement can test expressions based on ranges of values or conditions, whereas a `switch` statement tests expressions based only on a single integer, enumerated value, or `String` object.

Another point of interest is the `break` statement. Each `break` statement terminates the enclosing `switch` statement. Control flow continues with the first statement following the `switch` block. The `break` statements are necessary because without them, statements in `switch` blocks **fall through**: All statements after the matching `case` label are executed in sequence, regardless of the expression of subsequent `case` labels, until a `break` statement is encountered. The program 
[`SwitchDemoFallThrough`](examples/SwitchDemoFallThrough.java) shows statements in a `switch` block that fall through. The program displays the month corresponding to the integer `month` and the months that follow in the year:

```


public class SwitchDemoFallThrough {

    public static void main(String[] args) {
        java.util.ArrayList&lt;String&gt; futureMonths =
            new java.util.ArrayList&lt;String&gt;();

        int month = 8;

        switch (month) {
            case 1:  futureMonths.add("January");
            case 2:  futureMonths.add("February");
            case 3:  futureMonths.add("March");
            case 4:  futureMonths.add("April");
            case 5:  futureMonths.add("May");
            case 6:  futureMonths.add("June");
            case 7:  futureMonths.add("July");
            case 8:  futureMonths.add("August");
            case 9:  futureMonths.add("September");
            case 10: futureMonths.add("October");
            case 11: futureMonths.add("November");
            case 12: futureMonths.add("December");
                     break;
            default: break;
        }

        if (futureMonths.isEmpty()) {
            System.out.println("Invalid month number");
        } else {
            for (String monthName : futureMonths) {
               System.out.println(monthName);
            }
        }
    }
}

```

This is the output from the code:

```

August
September
October
November
December

```

Technically, the final `break` is not required because flow falls out of the `switch` statement. Using a `break` is recommended so that modifying the code is easier and less error prone. The `default` section handles all values that are not explicitly handled by one of the `case` sections.

The following code example, 
[`SwitchDemo2`](examples/SwitchDemo2.java), shows how a statement can have multiple `case` labels. The code example calculates the number of days in a particular month:

```


class SwitchDemo2 {
    public static void main(String[] args) {

        int month = 2;
        int year = 2000;
        int numDays = 0;

        switch (month) {
            case 1: case 3: case 5:
            case 7: case 8: case 10:
            case 12:
                numDays = 31;
                break;
            case 4: case 6:
            case 9: case 11:
                numDays = 30;
                break;
            case 2:
                if (((year % 4 == 0) &amp;&amp; 
                     !(year % 100 == 0))
                     || (year % 400 == 0))
                    numDays = 29;
                else
                    numDays = 28;
                break;
            default:
                System.out.println("Invalid month.");
                break;
        }
        System.out.println("Number of Days = "
                           + numDays);
    }
}

```

This is the output from the code:

```

Number of Days = 29

```

## Using Strings in switch Statements

In Java SE 7 and later, you can use a `String` object in the `switch` statement's expression. The following code example, 
[`StringSwitchDemo`](examples/StringSwitchDemo.java), displays the number of the month based on the value of the `String` named `month`:

```


public class StringSwitchDemo {

    public static int getMonthNumber(String month) {

        int monthNumber = 0;

        if (month == null) {
            return monthNumber;
        }

        switch (month.toLowerCase()) {
            case "january":
                monthNumber = 1;
                break;
            case "february":
                monthNumber = 2;
                break;
            case "march":
                monthNumber = 3;
                break;
            case "april":
                monthNumber = 4;
                break;
            case "may":
                monthNumber = 5;
                break;
            case "june":
                monthNumber = 6;
                break;
            case "july":
                monthNumber = 7;
                break;
            case "august":
                monthNumber = 8;
                break;
            case "september":
                monthNumber = 9;
                break;
            case "october":
                monthNumber = 10;
                break;
            case "november":
                monthNumber = 11;
                break;
            case "december":
                monthNumber = 12;
                break;
            default: 
                monthNumber = 0;
                break;
        }

        return monthNumber;
    }

    public static void main(String[] args) {

        String month = "August";

        int returnedMonthNumber =
            StringSwitchDemo.getMonthNumber(month);

        if (returnedMonthNumber == 0) {
            System.out.println("Invalid month");
        } else {
            System.out.println(returnedMonthNumber);
        }
    }
}

```

The output from this code is `8`.

The `String` in the `switch` expression is compared with the expressions associated with each `case` label as if the 
[`String.equals`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#equals-java.lang.Object-) method were being used. In order for the `StringSwitchDemo` example to accept any month regardless of case, `month` is converted to lowercase (with the 
[`toLowerCase`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#toLowerCase--) method), and all the strings associated with the `case` labels are in lowercase.

**Note**: This example checks if the expression in the `switch` statement is `null`. Ensure that the expression in any `switch` statement is not null to prevent a `NullPointerException` from being thrown.
