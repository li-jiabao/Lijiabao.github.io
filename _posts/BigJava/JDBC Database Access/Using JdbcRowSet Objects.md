
# Using JdbcRowSet Objects

A `JdbcRowSet` object is an enhanced `ResultSet` object. It maintains a connection to its data source, just as a `ResultSet` object does. The big difference is that it has a set of properties and a listener notification mechanism that make it a JavaBeans component.

One of the main uses of a `JdbcRowSet` object is to make a `ResultSet` object scrollable and updatable when it does not otherwise have those capabilities.

This section covers the following topics:

- [Creating JdbcRowSet Objects](#creating-jdbcrowset-object)
- [Default JdbcRowSet Objects](#default-jdbcrowset-objects)
- [Setting Properties](#setting-properties)
- [Using JdbcRowSet Objects](#using-jdbcrowset-object)
- [Code Sample](#code-sample)

## <a name="creating-jdbcrowset-object" id="creating-jdbcrowset-object">Creating JdbcRowSet Objects</a>

Create a `JdbcRowSet` object by using an instance of `RowSetFactory`, which is created from the class `RowSetProvider`. The following example is from `[JdbcRowSetSample](gettingstarted.html)`:

```

    RowSetFactory factory = RowSetProvider.newFactory();

    try (JdbcRowSet jdbcRs = factory.createJdbcRowSet()) {
      jdbcRs.setUrl(this.settings.urlString);
      jdbcRs.setUsername(this.settings.userName);
      jdbcRs.setPassword(this.settings.password);
      jdbcRs.setCommand("select * from COFFEES");
      jdbcRs.execute();
      // ...

```

The `RowSetFactory` interface contains methods to create the different types of `RowSet` implementations:

- `createCachedRowSet`
- `createFilteredRowSet`
- `createJdbcRowSet`
- `createJoinRowSet`
- `createWebRowSet`

## <a name="default-jdbcrowset-objects" id="default-jdbcrowset-objects">Default JdbcRowSet Objects</a>

When you create a `JdbcRowSet` object with an instance of `RowSetFactory`, the new `JdbcRowSet` object will have the following properties:

- `type`: `ResultSet.TYPE_SCROLL_INSENSITIVE` (has a scrollable cursor)
- `concurrency`: `ResultSet.CONCUR_UPDATABLE` (can be updated)
- `escapeProcessing`: `true` (the driver will do escape processing; when escape processing is enabled, the driver will scan for any escape syntax and translate it into code that the particular database understands)
- `maxRows`: `0` (no limit on the number of rows)
- `maxFieldSize`: `0` (no limit on the number of bytes for a column value; applies only to columns that store `BINARY`, `VARBINARY`, `LONGVARBINARY`, `CHAR`, `VARCHAR`, and `LONGVARCHAR` values)
- `queryTimeout`: `0` (has no time limit for how long it takes to execute a query)
- `showDeleted`: `false` (deleted rows are not visible)
- `transactionIsolation`: `Connection.TRANSACTION_READ_COMMITTED` (reads only data that has been committed)
- `typeMap`: `null` (the type map associated with a `Connection` object used by this `RowSet` object is `null`)

The main thing you must remember from this list is that a `JdbcRowSet` and all other `RowSet` objects are scrollable and updatable unless you set different values for those properties.

## <a name="setting-properties" id="setting-properties">Setting Properties</a>

The section [Default JdbcRowSet Objects](#default-jdbcrowset-objects) lists the properties that are set by default when a new `JdbcRowSet` object is created. If you use the default constructor, you must set some additional properties before you can populate your new `JdbcRowSet` object with data.

In order to get its data, a `JdbcRowSet` object first needs to connect to a database. The following four properties hold information used in obtaining a connection to a database.

- `username`: the name a user supplies to a database as part of gaining access
- `password`: the user's database password
- `url`: the JDBC URL for the database to which the user wants to connect
- `datasourceName`: the name used to retrieve a `DataSource` object that has been registered with a JNDI naming service

Which of these properties you set depends on how you are going to make a connection. The preferred way is to use a `DataSource` object, but it may not be practical for you to register a `DataSource` object with a JNDI naming service, which is generally done by a system administrator. Therefore, the code examples all use the `DriverManager` mechanism to obtain a connection, for which you use the `url` property and not the `datasourceName` property.

Another property that you must set is the `command` property. This property is the query that determines what data the `JdbcRowSet` object will hold. For example, the following line of code sets the `command` property with a query that produces a `ResultSet` object containing all the data in the table `COFFEES`:

```

jdbcRs.setCommand("select * from COFFEES");

```

After you have set the `command` property and the properties necessary for making a connection, you are ready to populate the `jdbcRs` object with data by calling the `execute` method.

```

jdbcRs.execute();

```

The `execute` method does many things for you in the background:

- It makes a connection to the database using the values you assigned to the `url`, `username`, and `password` properties.
- It executes the query you set in the `command` property.
- It reads the data from the resulting `ResultSet` object into the `jdbcRs` object.

## <a name="using-jdbcrowset-object" id="using-jdbcrowset-object">Using JdbcRowSet Objects</a>

You update, insert, and delete a row in a `JdbcRowSet` object the same way you update, insert, and delete a row in an updatable `ResultSet` object. Similarly, you navigate a `JdbcRowSet` object the same way you navigate a scrollable `ResultSet` object.

The Coffee Break chain of coffee houses acquired another chain of coffee houses and now has a legacy database that does not support scrolling or updating of a result set. In other words, any `ResultSet` object produced by this legacy database does not have a scrollable cursor, and the data in it cannot be modified. However, by creating a `JdbcRowSet` object populated with the data from a `ResultSet` object, you can, in effect, make the `ResultSet` object scrollable and updatable.

As mentioned previously, a `JdbcRowSet` object is by default scrollable and updatable. Because its contents are identical to those in a `ResultSet` object, operating on the `JdbcRowSet` object is equivalent to operating on the `ResultSet` object itself. And because a `JdbcRowSet` object has an ongoing connection to the database, changes it makes to its own data are also made to the data in the database.

This section covers the following topics:

- [Navigating JdbcRowSet Objects](#navigating-jdbcrowset-object)
- [Updating Column Values](#updating-column-value)
- [Inserting Rows](#inserting-row)
- [Deleting Rows](#deleting-row)

### <a name="navigating-jdbcrowset-object" id="navigating-jdbcrowset-object">Navigating JdbcRowSet Objects</a>

A `ResultSet` object that is not scrollable can use only the `next` method to move its cursor forward, and it can move the cursor only forward from the first row to the last row. A default `JdbcRowSet` object, however, can use all of the cursor movement methods defined in the `ResultSet` interface.

A `JdbcRowSet` object can call the method `next`, and it can also call any of the other `ResultSet` cursor movement methods. For example, the following lines of code move the cursor to the fourth row in the `jdbcRs` object and then back to the third row:

```

jdbcRs.absolute(4);
jdbcRs.previous();

```

The method `previous` is analogous to the method `next` in that it can be used in a `while` loop to traverse all of the rows in order. The difference is that you must move the cursor to a position after the last row, and `previous` moves the cursor toward the beginning.

### <a name="updating-column-value" id="updating-column-value">Updating Column Values</a>

You update data in a `JdbcRowSet` object the same way you update data in a `ResultSet` object.

Assume that the Coffee Break owner wants to raise the price for a pound of Espresso coffee. If the owner knows that Espresso is in the third row of the `jdbcRs` object, the code for doing this might look like the following:

```

jdbcRs.absolute(3);
jdbcRs.updateFloat("PRICE", 10.99f);
jdbcRs.updateRow();

```

The code moves the cursor to the third row and changes the value for the column `PRICE` to 10.99, and then updates the database with the new price.

Calling the method `updateRow` updates the database because `jdbcRs` has maintained its connection to the database. For disconnected `RowSet` objects, the situation is different.

### <a name="inserting-row" id="inserting-row">Inserting Rows</a>

If the owner of the Coffee Break chain wants to add one or more coffees to what he offers, the owner will need to add one row to the `COFFEES` table for each new coffee, as is done in the following code fragment from `[JdbcRowSetSample](gettingstarted.html)`. Notice that because the `jdbcRs` object is always connected to the database, inserting a row into a `JdbcRowSet` object is the same as inserting a row into a `ResultSet` object: You move to the cursor to the insert row, use the appropriate updater method to set a value for each column, and call the method `insertRow`:

```

jdbcRs.moveToInsertRow();
jdbcRs.updateString("COF_NAME", "HouseBlend");
jdbcRs.updateInt("SUP_ID", 49);
jdbcRs.updateFloat("PRICE", 7.99f);
jdbcRs.updateInt("SALES", 0);
jdbcRs.updateInt("TOTAL", 0);
jdbcRs.insertRow();

jdbcRs.moveToInsertRow();
jdbcRs.updateString("COF_NAME", "HouseDecaf");
jdbcRs.updateInt("SUP_ID", 49);
jdbcRs.updateFloat("PRICE", 8.99f);
jdbcRs.updateInt("SALES", 0);
jdbcRs.updateInt("TOTAL", 0);
jdbcRs.insertRow();

```

When you call the method `insertRow`, the new row is inserted into the `jdbcRs` object and is also inserted into the database. The preceding code fragment goes through this process twice, so two new rows are inserted into the `jdbcRs` object and the database.

### <a name="deleting-row" id="deleting-row">Deleting Rows</a>

As is true with updating data and inserting a new row, deleting a row is just the same for a `JdbcRowSet` object as for a `ResultSet` object. The owner wants to discontinue selling French Roast decaffeinated coffee, which is the last row in the `jdbcRs` object. In the following lines of code, the first line moves the cursor to the last row, and the second line deletes the last row from the `jdbcRs` object and from the database:

```

jdbcRs.last();
jdbcRs.deleteRow();

```

## <a name="code-sample" id="code-sample">Code Sample</a>

The sample `[JdbcRowSetSample](gettingstarted.html)` does the following:

- Creates a new `JdbcRowSet` object initialized with the `ResultSet` object that was produced by the execution of a query that retrieves all the rows in the `COFFEES` table
- Moves the cursor to the third row of the `COFFEES` table and updates the `PRICE` column in that row
- Inserts two new rows, one for `HouseBlend` and one for `HouseDecaf`
- Moves the cursor to the last row and deletes it
