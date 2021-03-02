
# Using CachedRowSetObjects

A `CachedRowSet` object is special in that it can operate without being connected to its data source, that is, it is a *disconnected* `RowSet` object. It gets its name from the fact that it stores (caches) its data in memory so that it can operate on its own data rather than on the data stored in a database.

The `CachedRowSet` interface is the superinterface for all disconnected `RowSet` objects, so everything demonstrated here also applies to `WebRowSet`, `JoinRowSet`, and `FilteredRowSet` objects.

Note that although the data source for a `CachedRowSet` object (and the `RowSet` objects derived from it) is almost always a relational database, a `CachedRowSet` object is capable of getting data from any data source that stores its data in a tabular format. For example, a flat file or spreadsheet could be the source of data. This is true when the `RowSetReader` object for a disconnected `RowSet` object is implemented to read data from such a data source. The `CachedRowSet` interface has a `RowSetReader` object that reads data from a relational database, so in this tutorial, the data source is always a database.

The following topics are covered:

- [Setting Up CachedRowSet Objects](#setting-up-cachedrowset-object)
- [Populating CachedRowSet Objects](#populating-cachedrowset-object)
- [What Reader Does](#reader)
- [Updating CachedRowSet Objects](#updating-cachedrowset-object)
- [Updating Data Sources](#updating-data-source)
- [What Writer Does](#writer)
- [Notifying Listeners](#notifying-listeners)
- [Sending Large Amounts of Data](#sending-large-amounts-of-data)

## <a name="setting-up-cachedrowset-object" id="setting-up-cachedrowset-object">Setting Up CachedRowSet Objects</a>

Setting up a `CachedRowSet` object involves the following:

- [Creating CachedRowSet Objects](#creating-cachedrowset-object)
- [Setting CachedRowSet Properties](#setting-cachedrowset-properties)
- [Setting Key Columns](#setting-key-columns)

### <a name="creating-cachedrowset-object" id="creating-cachedrowset-object">Creating CachedRowSet Objects</a>

Create a new `CachedRowSet` object by using an instance of `RowSetFactory`, which is created from the class `RowSetProvider`.

The following example from `[CachedRowSetSample](gettingstarted.html)` creates a `CachedRowSet` object:

```

RowSetFactory factory = RowSetProvider.newFactory();
CachedRowSet crs = factory.createCachedRowSet();

```

The object `crs` has the same default values for its properties that a `JdbcRowSet` object has when it is first created. In addition, it has been assigned an instance of the default `SyncProvider` implementation, `RIOptimisticProvider`.

A `SyncProvider` object supplies a `RowSetReader` object (a *reader*) and a `RowSetWriter` object (a *writer*), which a disconnected `RowSet` object needs in order to read data from its data source or to write data back to its data source. What a reader and writer do is explained later in the sections [What Reader Does](#reader) and [What Writer Does](#writer). One thing to keep in mind is that readers and writers work entirely in the background, so the explanation of how they work is for your information only. Having some background on readers and writers should help you understand what some of the methods defined in the `CachedRowSet` interface do in the background.

### <a name="setting-cachedrowset-properties" id="setting-cachedrowset-properties">Setting CachedRowSet Properties</a>

Generally, the default values for properties are fine as they are, but you may change the value of a property by calling the appropriate setter method. There are some properties without default values that you must set yourself.

In order to get data, a disconnected `RowSet` object must be able to connect to a data source and have some means of selecting the data it is to hold. The following properties hold information necessary to obtain a connection to a database.

- `username`: The name a user supplies to a database as part of gaining access
- `password`: The user's database password
- `url`: The JDBC URL for the database to which the user wants to connect
- `datasourceName`: The name used to retrieve a DataSource object that has been registered with a JNDI naming service

Which of these properties you must set depends on how you are going to make a connection. The preferred way is to use a `DataSource` object, but it may not be practical for you to register a `DataSource` object with a JNDI naming service, which is generally done by a system administrator. Therefore, the code examples all use the `DriverManager` mechanism to obtain a connection, for which you use the `url` property and not the `datasourceName` property.

The following lines of code set the `username`, `password`, and `url` properties so that a connection can be obtained using the `DriverManager` class. (You will find the JDBC URL to set as the value for the `url` property in the documentation for your JDBC driver.)

```

public void setConnectionProperties(
    String username, String password) {
    crs.setUsername(username);
    crs.setPassword(password);
    crs.setUrl("jdbc:mySubprotocol:mySubname");
    // ...

```

Another property that you must set is the `command` property. Data is read into a `RowSet` object from a `ResultSet` object. The query that produces that `ResultSet` object is the value for the `command` property. For example, the following line of code sets the `command` property with a query that produces a `ResultSet` object containing all the data in the table `MERCH_INVENTORY`:

```

crs.setCommand("select * from MERCH_INVENTORY");

```

### <a name="setting-key-columns" id="setting-key-columns">Setting Key Columns</a>

If you are going make any updates to the `crs` object and want those updates saved in the database, you must set one more piece of information: the key columns. Key columns are essentially the same as a primary key because they indicate one or more columns that uniquely identify a row. The difference is that a primary key is set on a table in the database, whereas key columns are set on a particular `RowSet` object. The following lines of code set the key columns for `crs` to the first column:

```

int[] keys = {1};
crs.setKeyColumns(keys);

```

The first column in the table `MERCH_INVENTORY` is `ITEM_ID`. It can serve as the key column because every item identifier is different and therefore uniquely identifies one row and only one row in the table `MERCH_INVENTORY`. In addition, this column is specified as a primary key in the definition of the `MERCH_INVENTORY` table. The method `setKeyColumns` takes an array to allow for the fact that it may take two or more columns to identify a row uniquely.

As a point of interest, the method `setKeyColumns` does not set a value for a property. In this case, it sets the value for the field `keyCols`. Key columns are used internally, so after setting them, you do nothing more with them. You will see how and when key columns are used in the section [Using SyncResolver Objects](#syncresolver).

## <a name="populating-cachedrowset-object" id="populating-cachedrowset-object">Populating CachedRowSet Objects</a>

Populating a disconnected `RowSet` object involves more work than populating a connected `RowSet` object. Fortunately, the extra work is done in the background. After you have done the preliminary work to set up the `CachedRowSet` object `crs`, the following line of code populates `crs`:

```

crs.execute();

```

The data in `crs` is the data in the `ResultSet` object 
produced by executing the query in the command property.

What is different is that the `CachedRowSet` implementation for the `execute` method does a lot more than the `JdbcRowSet` implementation. Or more correctly, the `CachedRowSet` object's reader, to which the method execute delegates its tasks, does a lot more.

Every disconnected `RowSet` object has a `SyncProvider` object assigned to it, and this `SyncProvider` object is what provides the `RowSet` object's **reader** (a `RowSetReader` object). When the `crs` object was created, it was used as the default `CachedRowSetImpl` constructor, which, in addition to setting default values for properties, assigns an instance of the `RIOptimisticProvider` implementation as the default `SyncProvider` object.

## <a name="reader" id="reader">What Reader Does</a>

When an application calls the method `execute`, a disconnected `RowSet` object's reader works behind the scenes to populate the `RowSet` object with data. A newly created `CachedRowSet` object is not connected to a data source and therefore must obtain a connection to that data source in order to get data from it. The default `SyncProvider` object (`RIOptimisticProvider`) provides a reader that obtains a connection by using the values set for the user name, password, and either the JDBC URL or the data source name, whichever was set more recently. Then the reader executes the query set for the command. It reads the data in the `ResultSet` object produced by the query, populating the `CachedRowSet` object with that data. Finally, the reader closes the connection.

## <a name="updating-cachedrowset-object" id="updating-cachedrowset-object">Updating CachedRowSet Object</a>

In the Coffee Break scenario, the owner wants to streamline operations. The owner decides to have employees at the warehouse enter inventory directly into a PDA (personal digital assistant), thereby avoiding the error-prone process of having a second person do the data entry. A `CachedRowSet` object is ideal in this situation because it is lightweight, serializable, and can be updated without a connection to the data source.

The owner will have the application development team create a GUI tool for the PDA that warehouse employees will use for entering inventory data. Headquarters will create a `CachedRowSet` object populated with the table showing the current inventory and send it using the Internet to the PDAs. When a warehouse employee enters data using the GUI tool, the tool adds each entry to an array, which the `CachedRowSet` object will use to perform the updates in the background. Upon completion of the inventory, the PDAs send their new data back to headquarters, where the data is uploaded to the main server.

This section covers the following topics:

- [Updating Column Values](#updating-column-value)
- [Inserting and Deleting Rows](#inserting-and-deleting-rows)

### <a name="updating-column-value" id="updating-column-value">Updating Column Values</a>

Updating data in a `CachedRowSet` object is just the same as updating data in a `JdbcRowSet` object. For example, the following code fragment from `[CachedRowSetSample.java](gettingstarted.html)` increments the value in the column `QUAN` by 1 in the row whose `ITEM_ID` column has an item identifier of `12345`:

```

        while (crs.next()) {
          System.out.println("Found item " + crs.getInt("ITEM_ID") + ": " +
                             crs.getString("ITEM_NAME"));
          if (crs.getInt("ITEM_ID") == 1235) {
            int currentQuantity = crs.getInt("QUAN") + 1;
            System.out.println("Updating quantity to " + currentQuantity);
            crs.updateInt("QUAN", currentQuantity + 1);
            crs.updateRow();
            // Syncing the row back to the DB
            crs.acceptChanges(con);
          }
        } // End of inner while

```

### <a name="inserting-and-deleting-rows" id="inserting-and-deleting-rows">Inserting and Deleting Rows</a>

Just as with updating a column value, the code for inserting and deleting rows in a `CachedRowSet` object is the same as for a `JdbcRowSet` object.

The following excerpt from `[CachedRowSetSample.java](gettingstarted.html)` inserts a new row into the `CachedRowSet` object `crs`:

```

crs.moveToInsertRow();
crs.updateInt("ITEM_ID", newItemId);
crs.updateString("ITEM_NAME", "TableCloth");
crs.updateInt("SUP_ID", 927);
crs.updateInt("QUAN", 14);
Calendar timeStamp;
timeStamp = new GregorianCalendar();
timeStamp.set(2006, 4, 1);
crs.updateTimestamp(
    "DATE_VAL",
    new Timestamp(timeStamp.getTimeInMillis()));
crs.insertRow();
crs.moveToCurrentRow();

```

If headquarters has decided to stop stocking a particular item, it would probably remove the row for that coffee itself. However, in the scenario, a warehouse employee using a PDA also has the capability of removing it. The following code fragment finds the row where the value in the `ITEM_ID` column is `12345` and deletes it from the `CachedRowSet` `crs`:

```

while (crs.next()) {
    if (crs.getInt("ITEM_ID") == 12345) {
        crs.deleteRow();
        break;
    }
}

```

## <a name="updating-data-source" id="updating-data-source">Updating Data Sources</a>

There is a major difference between making changes to a `JdbcRowSet` object and making changes to a `CachedRowSet` object. Because a `JdbcRowSet` object is connected to its data source, the methods `updateRow`, `insertRow`, and `deleteRow` can update both the `JdbcRowSet` object and the data source. In the case of a disconnected `RowSet` object, however, these methods update the data stored in the `CachedRowSet` object's memory but cannot affect the data source. A disconnected `RowSet` object must call the method `acceptChanges` in order to save its changes to the data source. In the inventory scenario, back at headquarters, an application will call the method `acceptChanges` to update the database with the new values for the column `QUAN`.

```

crs.acceptChanges();

```

## <a name="writer" id="writer">What Writer Does</a>

Like the method `execute`, the method `acceptChanges` does its work invisibly. Whereas the method `execute` delegates its work to the `RowSet` object's reader, the method `acceptChanges` delegates its tasks to the `RowSet` object's writer. In the background, the writer opens a connection to the database, updates the database with the changes made to the `RowSet` object, and then closes the connection.

### Using Default Implementation

The difficulty is that a *conflict* can arise. A conflict is a situation in which another party has updated a value in the database that corresponds to a value that was updated in a `RowSet` object. Which value should persist in the database? What the writer does when there is a conflict depends on how it is implemented, and there are many possibilities. At one end of the spectrum, the writer does not even check for conflicts and just writes all changes to the database. This is the case with the `RIXMLProvider` implementation, which is used by a `WebRowSet` object. At the other end, the writer ensures that there are no conflicts by setting database locks that prevent others from making changes.

The writer for the `crs` object is the one provided by the default `SyncProvider` implementation, `RIOptimisticProvider`. The `RIOPtimisticProvider` implementation gets its name from the fact that it uses an optimistic concurrency model. This model assumes that there will be few, if any, conflicts and therefore sets no database locks. The writer checks to see if there are any conflicts, and if there is none, it writes the changes made to the `crs` object to the database, and those changes become persistent. If there are any conflicts, the default is not to write the new `RowSet` values to the database.

In the scenario, the default behavior works very well. Because no one at headquarters is likely to change the value in the `QUAN` column of `COF_INVENTORY`, there will be no conflicts. As a result, the values entered into the `crs` object at the warehouse will be written to the database and thus will be persistent, which is the desired outcome.

## <a name="syncresolver" id="syncresolver">Using SyncResolver Objects</a>

In other situations, however, it is possible for conflicts to exist. To accommodate these situations, the `RIOPtimisticProvider` implementation provides an option that lets you look at the values in conflict and decide which ones should be persistent. This option is the use of a `SyncResolver` object.

When the writer has finished looking for conflicts and has found one or more, it creates a `SyncResolver` object containing the database values that caused the conflicts. Next, the method `acceptChanges` throws a `SyncProviderException` object, which an application may catch and use to retrieve the `SyncResolver` object. The following lines of code retrieve the `SyncResolver` object `resolver`:

```

try {
    crs.acceptChanges();
} catch (SyncProviderException spe) {
    SyncResolver resolver = spe.getSyncResolver();
}

```

The object `resolver` is a `RowSet` object that replicates the `crs` object except that it contains only the values in the database that caused a conflict. All other column values are null.

With the `resolver` object, you can iterate through its rows to locate the values that are not null and are therefore values that caused a conflict. Then you can locate the value at the same position in the `crs` object and compare them. The following code fragment retrieves `resolver` and uses the `SyncResolver` method `nextConflict` to iterate through the rows that have conflicting values. The object `resolver` gets the status of each conflicting value, and if it is `UPDATE_ROW_CONFLICT`, meaning that the `crs` was attempting an update when the conflict occurred, the `resolver` object gets the row number of that value. Then the code moves the cursor for the `crs` object to the same row. Next, the code finds the column in that row of the `resolver` object that contains a conflicting value, which will be a value that is not null. After retrieving the value in that column from both the `resolver` and `crs` objects, you can compare the two and decide which one you want to become persistent. Finally, the code sets that value in both the `crs` object and the database using the method `setResolvedValue`, as shown in the following code from `[CachedRowSetSample](gettingstarted.html)`:

```

    try {
        // ...
        // Syncing the new row back to the database.
        System.out.println("About to add a new row...");
        crs.acceptChanges(con);
        System.out.println("Added a row...");
        this.viewTable(con);
        // ...
    } catch (SyncProviderException spe) {

      SyncResolver resolver = spe.getSyncResolver();

      Object crsValue; // value in the RowSet object
      Object resolverValue; // value in the SyncResolver object
      Object resolvedValue; // value to be persisted

      while (resolver.nextConflict()) {

        if (resolver.getStatus() == SyncResolver.INSERT_ROW_CONFLICT) {
          int row = resolver.getRow();
          crs.absolute(row);

          int colCount = crs.getMetaData().getColumnCount();
          for (int j = 1; j &lt;= colCount; j++) {
            if (resolver.getConflictValue(j) != null) {
              crsValue = crs.getObject(j);
              resolverValue = resolver.getConflictValue(j);

              // Compare crsValue and resolverValue to determine
              // which should be the resolved value (the value to persist)
              //
              // This example choses the value in the RowSet object,
              // crsValue, to persist.,

              resolvedValue = crsValue;

              resolver.setResolvedValue(j, resolvedValue);
            }
          }
        }
      }
    }

```

## <a name="notifying-listeners" id="notifying-listeners">Notifying Listeners</a>

Being a JavaBeans component means that a `RowSet` object can notify other components when certain things happen to it. For example, if data in a `RowSet` object changes, the `RowSet` object can notify interested parties of that change. The nice thing about this notification mechanism is that, as an application programmer, all you have to do is add or remove the components that will be notified.

This section covers the following topics:

- [Setting Up Listeners](#setting-up-listeners)
- [How Notification Works](#how-notification-works)

### <a name="setting-up-listeners" id="setting-up-listeners">Setting Up Listeners</a>

A *listener* for a `RowSet` object is a component that implements the following methods from the `RowSetListener` interface:

- `cursorMoved`: Defines what the listener will do, if anything, when the cursor in the `RowSet` object moves.
- `rowChanged`: Defines what the listener will do, if anything, when one or more column values in a row have changed, a row has been inserted, or a row has been deleted.
- `rowSetChanged`: Defines what the listener will do, if anything, when the `RowSet` object has been populated with new data.

An example of a component that might want to be a listener is a `BarGraph` object that graphs the data in a `RowSet` object. As the data changes, the `BarGraph` object can update itself to reflect the new data.

As an application programmer, the only thing you must do to take advantage of the notification mechanism is to add or remove listeners. The following line of code means that every time the cursor for the `crs` objects moves, values in `crs` are changed, or `crs` as a whole gets new data, the `BarGraph` object `bar` will be notified:

```

crs.addRowSetListener(bar);

```

You can also stop notifications by removing a listener, as is done in the following line of code:

```

crs.removeRowSetListener(bar);

```

Using the Coffee Break scenario, assume that headquarters checks with the database periodically to get the latest price list for the coffees it sells online. In this case, the listener is the `PriceList` object `priceList` at the Coffee Break web site, which must implement the `RowSetListener` methods `cursorMoved`, `rowChanged`, and `rowSetChanged`. The implementation of the `cursorMoved` method could be to do nothing because the position of the cursor does not affect the `priceList` object. The implementations for the `rowChanged` and `rowSetChanged` methods, on the other hand, must ascertain what changes have been made and update `priceList` accordingly.

### <a name="how-notification-works" id="how-notification-works">How Notification Works</a>

Methods that cause any of the `RowSet` events automatically notify all registered listeners. For example, any method that moves the cursor also calls the method `cursorMoved` on each of the listeners. Similarly, the method `execute` calls the method `rowSetChanged` on all listeners, and `acceptChanges` calls `rowChanged` on all listeners.

## <a name="sending-large-amounts-of-data" id="sending-large-amounts-of-data">Sending Large Amounts of Data</a>

The sample code [`CachedRowSetSample.testCachedRowSet`](gettingstarted.html) demonstrates how data can be sent in smaller pieces.
