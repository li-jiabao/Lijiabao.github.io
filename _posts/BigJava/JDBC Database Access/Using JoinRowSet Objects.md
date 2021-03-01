
# Using JoinRowSet Objects

A `JoinRowSet` implementation lets you create a SQL `JOIN` between `RowSet` objects when they are not connected to a data source. This is important because it saves the overhead of having to create one or more connections.

The following topics are covered:

- [Creating JoinRowSet Objects](#creating-joinrowset-object)
- [Adding RowSet Objects](#adding-rowset-objects)
- [Managing Match Columns](#managing-match-columns)

The `JoinRowSet` interface is a subinterface of the `CachedRowSet` interface and thereby inherits the capabilities of a `CachedRowSet` object. This means that a `JoinRowSet` object is a disconnected `RowSet` object and can operate without always being connected to a data source.

## <a name="creating-joinrowset-object" id="creating-joinrowset-object">Creating JoinRowSet Objects</a>

A `JoinRowSet` object serves as the holder of a SQL `JOIN`. The following example from [`JoinSample`](gettingstarted.html) shows to create a `JoinRowSet` object:

```

    RowSetFactory factory = RowSetProvider.newFactory();  
    try (CachedRowSet coffees = factory.createCachedRowSet();
         CachedRowSet suppliers = factory.createCachedRowSet();
         JoinRowSet jrs = factory.createJoinRowSet()) {
      coffees.setCommand("SELECT * FROM COFFEES");
      // Set connection parameters for the CachedRowSet coffees
      coffees.execute();
      
      suppliers.setCommand("SELECT * FROM SUPPLIERS");
      // Set connection parameters for the CachedRowSet suppliers      suppliers.execute();      

      jrs.addRowSet(coffees, "SUP_ID");
      jrs.addRowSet(suppliers, "SUP_ID");

      // ...

```

The variable `jrs` holds nothing until `RowSet` objects are added to it.

## <a name="adding-rowset-objects" id="adding-rowset-objects">Adding RowSet Objects</a>

Any `RowSet` object can be added to a `JoinRowSet` object as long as it can be part of a SQL `JOIN`. A `JdbcRowSet` object, which is always connected to its data source, can be added, but typically it forms part of a `JOIN` by operating with the data source directly instead of becoming part of a `JOIN` by being added to a `JoinRowSet` object. The point of providing a `JoinRowSet` implementation is to make it possible for disconnected `RowSet` objects to become part of a `JOIN` relationship.

The owner of The Coffee Break chain of coffee houses wants to get a list of the coffees he buys from Acme, Inc. In order to do this, the owner will have to get information from two tables, `COFFEES` and `SUPPLIERS`. In the database world before `RowSet` technology, programmers would send the following query to the database:

```

String query =
    "SELECT COFFEES.COF_NAME " +
    "FROM COFFEES, SUPPLIERS " +
    "WHERE SUPPLIERS.SUP_NAME = Acme.Inc. " +
    "and " +
    "SUPPLIERS.SUP_ID = COFFEES.SUP_ID";

```

In the world of `RowSet` technology, you can accomplish the same result without having to send a query to the data source. You can add `RowSet` objects containing the data in the two tables to a `JoinRowSet` object. Then, because all the pertinent data is in the `JoinRowSet` object, you can perform a query on it to get the desired data.

The following code fragment from [`JoinSample.testJoinRowSet`](gettingstarted.html) creates two `CachedRowSet` objects, `coffees` populated with the data from the table `COFFEES`, and `suppliers` populated with the data from the table `SUPPLIERS`. The `coffees` and `suppliers` objects have to make a connection to the database to execute their commands and get populated with data, but after that is done, they do not have to reconnect again in order to form a `JOIN`.

```

    try (CachedRowSet coffees = factory.createCachedRowSet();
         CachedRowSet suppliers = factory.createCachedRowSet();
         JoinRowSet jrs = factory.createJoinRowSet()) {
      coffees.setCommand("SELECT * FROM COFFEES");
      coffees.setUsername(settings.userName);
      coffees.setPassword(settings.password);
      coffees.setUrl(settings.urlString);
      coffees.execute();
      
      suppliers.setCommand("SELECT * FROM SUPPLIERS");
      suppliers.setUsername(settings.userName);
      suppliers.setPassword(settings.password);
      suppliers.setUrl(settings.urlString);
      suppliers.execute();  
	  // ...

```

## <a name="managing-match-columns" id="managing-match-columns">Managing Match Columns</a>

Looking at the `SUPPLIERS` table, you can see that Acme, Inc. has an identification number of 101. The coffees in the table `COFFEES` with the supplier identification number of 101 are Colombian and Colombian_Decaf. The joining of information from both tables is possible because the two tables have the column `SUP_ID in common`. In JDBC `RowSet` technology, `SUP_ID`, the column on which the `JOIN` is based, is called the *match column*.

Each `RowSet` object added to a `JoinRowSet` object must have a match column, the column on which the `JOIN` is based. There are two ways to set the match column for a `RowSet` object. The first way is to pass the match column to the `JoinRowSet` method `addRowSet`, as shown in the following line of code:

```

jrs.addRowSet(coffees, "SUP_ID");

```

This line of code adds the `coffees` `CachedRowSet` to the `jrs` object and sets the `SUP_ID` column of `coffees` as the match column.

At this point, `jrs` has only `coffees` in it. The next `RowSet` object added to `jrs` will have to be able to form a `JOIN` with `coffees`, which is true of `suppliers` because both tables have the column SUP_ID. The following line of code adds `suppliers` to `jrs` and sets the column `SUP_ID` as the match column.

```

jrs.addRowSet(suppliers, "SUP_ID");

```

Now `jrs` contains a `JOIN` between `coffees` and `suppliers` from which the owner can get the names of the coffees supplied by Acme, Inc. Because the code did not set the type of `JOIN`, `jrs` holds an inner JOIN, which is the default. In other words, a row in `jrs` combines a row in `coffees` and a row in `suppliers`. It holds the columns in `coffees` plus the columns in `suppliers` for rows in which the value in the `COFFEES.SUP_ID` column matches the value in `SUPPLIERS.SUP_ID`. The following code prints out the names of coffees supplied by Acme, Inc., where the `String` `supplierName` is equal to `Acme, Inc.` Note that this is possible because the column `SUP_NAME`, which is from `suppliers`, and `COF_NAME`, which is from `coffees`, are now both included in the `JoinRowSet` object `jrs`.

```

      System.out.println("Coffees bought from " + supplierName + ": ");
      while (jrs.next()) {
        if (jrs.getString("SUP_NAME").equals(supplierName)) { 
          String coffeeName = jrs.getString(1);
          System.out.println("     " + coffeeName);
        }
      }

```

This will produce output similar to the following:

```

Coffees bought from Acme, Inc.:
     Colombian
     Colombian_Decaf

```

The `JoinRowSet` interface provides constants for setting the type of `JOIN` that will be formed, but currently the only type that is implemented is `JoinRowSet.INNER_JOIN`.
