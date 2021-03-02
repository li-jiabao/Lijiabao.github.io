
# Using FilteredRowSet Objects

A `FilteredRowSet` object lets you cut down the number of rows that are visible in a `RowSet` object so that you can work with only the data that is relevant to what you are doing. You decide what limits you want to set on your data (how you want to "filter" the data) and apply that filter to a `FilteredRowSet` object. In other words, the `FilteredRowSet` object makes visible only the rows of data that fit within the limits you set. A `JdbcRowSet` object, which always has a connection to its data source, can do this filtering with a query to the data source that selects only the columns and rows you want to see. The query's `WHERE` clause defines the filtering criteria. A `FilteredRowSet` object provides a way for a disconnected `RowSet` object to do this filtering without having to execute a query on the data source, thus avoiding having to get a connection to the data source and sending queries to it.

For example, assume that the Coffee Break chain of coffee houses has grown to hundreds of stores throughout the United States of America, and all of them are listed in a table called `COFFEE_HOUSES`. The owner wants to measure the success of only the stores in California with a coffee house comparison application that does not require a persistent connection to the database system. This comparison will look at the profitability of selling merchandise versus selling coffee drinks plus various other measures of success, and it will rank California stores by coffee drink sales, merchandise sales, and total sales. Because the table `COFFEE_HOUSES` has hundreds of rows, these comparisons will be faster and easier if the amount of data being searched is cut down to only those rows where the value in the column `STORE_ID` indicates California.

This is exactly the kind of problem that a `FilteredRowSet` object addresses by providing the following capabilities:

- Ability to limit the rows that are visible according to set criteria
- Ability to select which data is visible without being connected to a data source

The following topics are covered:

- [Defining Filtering Criteria in Predicate Objects](#defining-filtering-criteria-in-predicate-object)
- [Creating FilteredRowSet Objects](#creating-filteredrowset-object)
- [Creating and Setting Predicate Objects](#creating-and-setting-predicate-object)
- [Setting FilteredRowSet Objects with New Predicate Objects to Filter Data Further](#filter-data-further)
- [Updating FilteredRowSet Objects](#updating-filteredrowset-object)
- [Inserting or Updating Rows](#inserting-or-updating-row)
- [Removing All Filters so All Rows Are Visible](#removing-all-filters)
- [Deleting Rows](#deleting-row)

## <a name="defining-filtering-criteria-in-predicate-object" id="defining-filtering-criteria-in-predicate-object">Defining Filtering Criteria in Predicate Objects</a>

To set the criteria for which rows in a `FilteredRowSet` object will be visible, you define a class that implements the `Predicate` interface. An object created with this class is initialized with the following:

- The high end of the range within which values must fall
- The low end of the range within which values must fall
- The column name or column number of the column with the value that must fall within the range of values set by the high and low boundaries

Note that the range of values is inclusive, meaning that a value at the boundary is included in the range. For example, if the range has a high of 100 and a low of 50, a value of 50 is considered to be within the range. A value of 49 is not. Likewise, 100 is within the range, but 101 is not.

In line with the scenario where the owner wants to compare California stores, an implementation of the `Predicate` interface that filters for Coffee Break coffee houses located in California must be written. There is no one right way to do this, which means there is a lot of latitude in how the implementation is written. For example, you could name the class and its members whatever you want and implement a constructor and the three evaluate methods in any way that accomplishes the desired results.

The table listing all of the coffee houses, named `COFFEE_HOUSES`, has hundreds of rows. To make things more manageable, this example uses a table with far fewer rows, which is enough to demonstrate how filtering is done.

A value in the column `STORE_ID` is an `int` value that indicates, among other things, the state in which the coffee house is located. A value beginning with 10, for example, means that the state is California. `STORE_ID` values beginning with 32 indicate Oregon, and those beginning with 33 indicate the state of Washington.

The following class [`StateFilter`](gettingstarted.html) implements the `Predicate` interface:

```

public class StateFilter implements Predicate {

    private int lo;
    private int hi;
    private String colName = null;
    private int colNumber = -1;

    public StateFilter(int lo, int hi, int colNumber) {
        this.lo = lo;
        this.hi = hi;
        this.colNumber = colNumber;
    }

    public StateFilter(int lo, int hi, String colName) {
        this.lo = lo;
        this.hi = hi;
        this.colName = colName;
    }

    public boolean evaluate(Object value, String columnName) {
        boolean evaluation = true;
        if (columnName.equalsIgnoreCase(this.colName)) {
            int columnValue = ((Integer)value).intValue();
            if ((columnValue &gt;= this.lo)
                &amp;&amp;
                (columnValue &lt;= this.hi)) {
                evaluation = true;
            } else {
                evaluation = false;
            }
        }
        return evaluation;
    }

    public boolean evaluate(Object value, int columnNumber) {

        boolean evaluation = true;

        if (this.colNumber == columnNumber) {
            int columnValue = ((Integer)value).intValue();
            if ((columnValue &gt;= this.lo)
                &amp;&amp;
                (columnValue &lt;= this.hi)) {
                evaluation = true;
            } else {
                evaluation = false;
            }
        }
        return evaluation;
    }


    public boolean evaluate(RowSet rs) {
    
        CachedRowSet frs = (CachedRowSet)rs;
        boolean evaluation = false;

        try {
            int columnValue = -1;

            if (this.colNumber &gt; 0) {
                columnValue = frs.getInt(this.colNumber);
            } else if (this.colName != null) {
                columnValue = frs.getInt(this.colName);
            } else {
                return false;
            }

            if ((columnValue &gt;= this.lo)
                &amp;&amp;
                (columnValue &lt;= this.hi)) {
                evaluation = true;
            }
        } catch (SQLException e) {
            JDBCTutorialUtilities.printSQLException(e);
            return false;
        } catch (NullPointerException npe) {
            System.err.println("NullPointerException caught");
            return false;
        }
        return evaluation;
    }
}

```

This is a very simple implementation that checks the value in the column specified by either `colName` or `colNumber` to see if it is in the range of `lo` to `hi`, inclusive. The following line of code, from [`FilteredRowSetSample`](gettingstarted.html), creates a filter that allows only the rows where the `STORE_ID` column value indicates a value between 10000 and 10999, which indicates a California location:

```

StateFilter myStateFilter = new StateFilter(10000, 10999, 1);

```

Note that the `StateFilter` class just defined applies to one column. It is possible to have it apply to two or more columns by making each of the parameters arrays instead of single values. For example, the constructor for a `Filter` object could look like the following:

```

public Filter2(Object [] lo, Object [] hi, Object [] colNumber) {
    this.lo = lo;
    this.hi = hi;
    this.colNumber = colNumber;
}

```

The first element in the `colNumber` object gives the first column in which the value will be checked against the first element in `lo` and the first element in `hi`. The value in the second column indicated by `colNumber` will be checked against the second elements in `lo` and `hi`, and so on. Therefore, the number of elements in the three arrays should be the same. The following code is what an implementation of the method `evaluate(RowSet rs)` might look like for a `Filter2` object, in which the parameters are arrays:

```

public boolean evaluate(RowSet rs) {
    CachedRowSet crs = (CachedRowSet)rs;
    boolean bool1;
    boolean bool2;
    for (int i = 0; i &lt; colNumber.length; i++) {

        if ((rs.getObject(colNumber[i] &gt;= lo [i]) &amp;&amp;
            (rs.getObject(colNumber[i] &lt;= hi[i]) {
            bool1 = true;
        } else {
            bool2 = true;
        }

        if (bool2) {
            return false;
        } else {
            return true;
        }
    }
}

```

The advantage of using a `Filter2` implementation is that you can use parameters of any `Object` type and can check one column or multiple columns without having to write another implementation. However, you must pass an `Object` type, which means that you must convert a primitive type to its `Object` type. For example, if you use an `int` value for `lo` and `hi`, you must convert the `int` value to an `Integer` object before passing it to the constructor. `String` objects are already `Object` types, so you do not have to convert them.

## <a name="creating-filteredrowset-object" id="creating-filteredrowset-object">Creating FilteredRowSet Objects</a>

Use an instance of `RowSetFactory`, which is created from the class `RowSetProvider`, to create a `FilteredRowSet` object. The following is from [`FilteredRowSetSample`](gettingstarted.html):

```

    RowSetFactory factory = RowSetProvider.newFactory();
    try (FilteredRowSet frs = factory.createFilteredRowSet()) {
      // ...

```

Like other disconnected `RowSet` objects, the `frs` object must populate itself with data from a tabular data source, which is a relational database in the reference implementation. The following code fragment from [`FilteredRowSetSample`](gettingstarted.html) sets the properties necessary to connect to a database to execute its command. Note that this code uses the `DriverManager` class to make a connection, which is done for convenience. Usually, it is better to use a `DataSource` object that has been registered with a naming service that implements the Java Naming and Directory Interface (JNDI). The following example is from [`FilteredRowSetSample`](gettingstarted.html):

```

    RowSetFactory factory = RowSetProvider.newFactory();
    try (FilteredRowSet frs = factory.createFilteredRowSet()){
      frs.setCommand("SELECT * FROM COFFEE_HOUSES");
      frs.setUsername(settings.userName);
      frs.setPassword(settings.password);
      frs.setUrl(settings.urlString);
      frs.execute();
      // ...

```

The following line of code populates the `frs` object with the data stored in the `COFFEE_HOUSE` table:

```

frs.execute();

```

The method `execute` does all kinds of things in the background by calling on the `RowSetReader` object for `frs`, which creates a connection, executes the command for `frs`, populates `frs` with the data from the `ResultSet` object that is produced, and closes the connection. Note that if the table `COFFEE_HOUSES` had more rows than the `frs` object could hold in memory at one time, the `CachedRowSet` paging methods would have been used.

In the scenario, the Coffee Break owner would have done the preceding tasks in the office and then imported or downloaded the information stored in the `frs` object to the coffee house comparison application. From now on, the `frs` object will operate independently without the benefit of a connection to the data source.

## <a name="creating-and-setting-predicate-object" id="creating-and-setting-predicate-object">Creating and Setting Predicate Objects</a>

Now that the `FilteredRowSet` object `frs` contains the list of Coffee Break establishments, you can set selection criteria for narrowing down the number of rows in the `frs` object that are visible.

The following line of code uses the `StateFilter` class defined previously to create the object `myStateFilter`, which checks the column `STORE_ID` to determine which stores are in California (a store is in California if its ID number is between 10000 and 10999, inclusive):

```

StateFilter myStateFilter = new StateFilter(10000, 10999, 1);

```

The following line sets `myStateFilter` as the filter for `frs`.

```

frs.setFilter(myStateFilter);

```

To do the actual filtering, you call the method `next`, which in the reference implementation calls the appropriate version of the `Predicate.evaluate` method that you have implemented previously.

If the return value is `true`, the row will be visible; if the return value is `false`, the row will not be visible.

## <a name="filter-data-further" id="filter-data-further">Setting FilteredRowSet Objects with New Predicate Objects to Filter Data Further</a>

You set multiple filters serially. The first time you call the method `setFilter` and pass it a `Predicate` object, you have applied the filtering criteria in that filter. After calling the method `next` on each row, which makes visible only those rows that satisfy the filter, you can call `setFilter` again, passing it a different `Predicate` object. Even though only one filter is set at a time, the effect is that both filters apply cumulatively.

For example, the owner has retrieved a list of the Coffee Break stores in California by setting `stateFilter` as the `Predicate` object for `frs`. Now the owner wants to compare the stores in two California cities, San Francisco (SF in the table `COFFEE_HOUSES`) and Los Angeles (LA in the table). The first thing to do is to write a `Predicate` implementation that filters for stores in either SF or LA:

```

public class CityFilter implements Predicate {

    private String[] cities;
    private String colName = null;
    private int colNumber = -1;

    public CityFilter(String[] citiesArg, String colNameArg) {
        this.cities = citiesArg;
        this.colNumber = -1;
        this.colName = colNameArg;
    }

    public CityFilter(String[] citiesArg, int colNumberArg) {
        this.cities = citiesArg;
        this.colNumber = colNumberArg;
        this.colName = null;
    }

    public boolean evaluate Object valueArg, String colNameArg) {

        if (colNameArg.equalsIgnoreCase(this.colName)) {
            for (int i = 0; i &lt; this.cities.length; i++) {
                if (this.cities[i].equalsIgnoreCase((String)valueArg)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean evaluate(Object valueArg, int colNumberArg) {

        if (colNumberArg == this.colNumber) {
            for (int i = 0; i &lt; this.cities.length; i++) {
                if (this.cities[i].equalsIgnoreCase((String)valueArg)) {
                    return true;
                }
            }
        }
        return false;
    }


    public boolean evaluate(RowSet rs) {

        if (rs == null) return false;

        try {
            for (int i = 0; i &lt; this.cities.length; i++) {

                String cityName = null;

                if (this.colNumber &gt; 0) {
                    cityName = (String)rs.getObject(this.colNumber);
                } else if (this.colName != null) {
                    cityName = (String)rs.getObject(this.colName);
                } else {
                    return false;
                }

                if (cityName.equalsIgnoreCase(cities[i])) {
                    return true;
                }
            }
        } catch (SQLException e) {
            return false;
        }
        return false;
    }
}

```

The following code fragment from [`FilteredRowSetSample`](gettingstarted.html) sets the new filter and iterates through the rows in `frs`, printing out the rows where the `CITY` column contains either SF or LA. Note that `frs` currently contains only rows where the store is in California, so the criteria of the `Predicate` object `state` are still in effect when the filter is changed to another `Predicate` object. The code that follows sets the filter to the `CityFilter` object `city`. The `CityFilter` implementation uses arrays as parameters to the constructors to illustrate how that can be done:

```

  public void testFilteredRowSet() throws SQLException {
    
    StateFilter myStateFilter = new StateFilter(10000, 10999, 1);
    String[] cityArray = { "SF", "LA" };

    CityFilter myCityFilter = new CityFilter(cityArray, 2);
	
	RowSetFactory factory = RowSetProvider.newFactory();

    try (FilteredRowSet frs = factory.createFilteredRowSet()){
      frs.setCommand("SELECT * FROM COFFEE_HOUSES");
      frs.setUsername(settings.userName);
      frs.setPassword(settings.password);
      frs.setUrl(settings.urlString);
      frs.execute();

      System.out.println("\nBefore filter:");
      FilteredRowSetSample.viewTable(this.con);

      System.out.println("\nSetting state filter:");
      frs.beforeFirst();
      frs.setFilter(myStateFilter);
      this.viewFilteredRowSet(frs);

      System.out.println("\nSetting city filter:");
      frs.beforeFirst();
      frs.setFilter(myCityFilter);
      this.viewFilteredRowSet(frs);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

The output should contain a row for each store that is in San Francisco, California or Los Angeles, California. If there were a row in which the `CITY` column contained LA and the `STORE_ID` column contained 40003, it would not be included in the list because it had already been filtered out when the filter was set to `state`. (40003 is not in the range of 10000 to 10999.)

## <a name="updating-filteredrowset-object" id="updating-filteredrowset-object">Updating FilteredRowSet Objects</a>

You can make a change to a `FilteredRowSet` object but only if that change does not violate any of the filtering criteria currently in effect. For example, you can insert a new row or change one or more values in an existing row if the new value or values are within the filtering criteria.

## <a name="inserting-or-updating-row" id="inserting-or-updating-row">Inserting or Updating Rows</a>

Assume that two new Coffee Break coffee houses have just opened and the owner wants to add them to the list of all coffee houses. If a row to be inserted does not meet the cumulative filtering criteria in effect, it will be blocked from being added.

The current state of the `frs` object is that the `StateFilter` object was set and then the `CityFilter` object was set. As a result, `frs` currently makes visible only those rows that satisfy the criteria for both filters. And, equally important, you cannot add a row to the `frs` object unless it satisfies the criteria for both filters. The following code fragment attempts to insert two new rows into the `frs` object, one row in which the values in the `STORE_ID` and `CITY` columns both meet the criteria, and one row in which the value in `STORE_ID` does not pass the filter but the value in the `CITY` column does:

```

frs.moveToInsertRow();
frs.updateInt("STORE_ID", 10101);
frs.updateString("CITY", "SF");
frs.updateLong("COF_SALES", 0);
frs.updateLong("MERCH_SALES", 0);
frs.updateLong("TOTAL_SALES", 0);
frs.insertRow();

frs.updateInt("STORE_ID", 33101);
frs.updateString("CITY", "SF");
frs.updateLong("COF_SALES", 0);
frs.updateLong("MERCH_SALES", 0);
frs.updateLong("TOTAL_SALES", 0);
frs.insertRow();
frs.moveToCurrentRow();

```

If you were to iterate through the `frs` object using the method `next`, you would find a row for the new coffee house in San Francisco, California, but not for the store in San Francisco, Washington.

## <a name="removing-all-filters" id="removing-all-filters">Removing All Filters so All Rows Are Visible</a>

The owner can add the store in Washington by nullifying the filter. With no filter set, all rows in the `frs` object are once more visible, and a store in any location can be added to the list of stores. The following line of code unsets the current filter, effectively nullifying both of the `Predicate` implementations previously set on the `frs` object.

```

frs.setFilter(null);

```

## <a name="deleting-row" id="deleting-row">Deleting Rows</a>

If the owner decides to close down or sell one of the Coffee Break coffee houses, the owner will want to delete it from the `COFFEE_HOUSES` table. The owner can delete the row for the underperforming coffee house as long as the row is visible.

For example, given that the method `setFilter` has just been called with the argument null, there is no filter set on the `frs` object. This means that all rows are visible and can therefore be deleted. However, after the `StateFilter` object `myStateFilter` was set, which filtered out any state other than California, only stores located in California could be deleted. When the `CityFilter` object `myCityFilter` was set for the `frs` object, only coffee houses in San Francisco, California or Los Angeles, California could be deleted because they were in the only rows visible.
