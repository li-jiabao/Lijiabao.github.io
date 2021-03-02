
# Using RowId Objects

**Note**: MySQL and Java DB currently do not support the `RowId` JDBC interface. Consequently, no JDBC tutorial example is available to demonstrate the features described in this section.

A `RowId` object represents an address to a row in a database table. Note, however, that the `ROWID` type is not a standard SQL type. `ROWID` values can be useful because they are typically the fastest way to access a single row and are unique identifies for rows in a table. However, you should not use a `ROWID` value as the primary key of a table. For example, if you delete a particular row from a table, a database might reassign its `ROWID` value to a row inserted later.

The following topics are covered:

- [Retrieving RowId Objects](#retrieving_rowid_objects)
- [Using RowId Objects](#using_rowid_objects)
- [Lifetime of RowId Validity](#lifetime_rowid_validity)

## <a name="retrieving_rowid_objects" id="retrieving_rowid_objects">Retrieving RowId Objects</a>

Retrieve a `java.sql.RowId` object by calling the getter methods defined in the interfaces `ResultSet` and `CallableStatement`. The `RowId` object that is returned is an immutable object that you can use for subsequent referrals as a unique identifier to a row. The following is an example of calling the `ResultSet.getRowId` method:

```

java.sql.RowId rowId_1 = rs.getRowId(1);

```

## <a name="using_rowid_objects" id="using_rowid_objects">Using RowId Objects</a>

You can set a `RowId` object as a parameter in a parameterized `PreparedStatement` object:

```

Connection conn = ds.getConnection(username, password);
PreparedStatement ps = conn.prepareStatement(
    "INSERT INTO BOOKLIST" +
    "(ID, AUTHOR, TITLE, ISBN) " +
    "VALUES (?, ?, ?, ?)");
ps.setRowId(1, rowId_1);

```

You can also update a column with a specific `RowId` object in an updatable `ResultSet` object:

```

ResultSet rs = ...
rs.next();
rs.updateRowId(1, rowId_1);

```

A `RowId` object value is typically not portable between data sources and should be considered as specific to the data source when using the set or update method in `PreparedStatement` and `ResultSet` objects, respectively. It is therefore inadvisable to get a `RowId` object from a `ResultSet` object with a connection to one data source and then attempt to use the same `RowId` object in a unrelated `ResultSet` object with a connection to a different data source.

## <a name="lifetime_rowid_validity" id="lifetime_rowid_validity">Lifetime of RowId Validity</a>

A `RowId` object is valid as long as the identified row is not deleted and the lifetime of the `RowId` object is within the bounds of the lifetime specified by that the data source for the `RowId`.

To determine the lifetime of `RowId` objects of your database or data source, call the method `DatabaseMetaData.getRowIdLifetime`. It returns a value of a `RowIdLifetime` enumerated data type. The following method [`JDBCTutorialUtilities.rowIdLifeTime`](gettingstarted.html) returns the lifetime of `RowId` objects:

```

public static void rowIdLifetime(Connection conn)
    throws SQLException {

    DatabaseMetaData dbMetaData = conn.getMetaData();
    RowIdLifetime lifetime = dbMetaData.getRowIdLifetime();

    switch (lifetime) {
        case ROWID_UNSUPPORTED:
            System.out.println("ROWID type not supported");
            break;

        case ROWID_VALID_FOREVER:
            System.out.println("ROWID has unlimited lifetime");
            break;

        case ROWID_VALID_OTHER:
            System.out.println("ROWID has indeterminate lifetime");
            break;

        case ROWID_VALID_SESSION:
            System.out.println(
                "ROWID type has lifetime that " +
                "is valid for at least the " +
                "containing session");
            break;

        case ROWID_VALID_TRANSACTION:
            System.out.println(
                "ROWID type has lifetime that " +
                "is valid for at least the " +
                "containing transaction");
            break;
    }
}

```
