
# Using Array Objects

**Note**: MySQL and Java DB currently do not support the `ARRAY` SQL data type. Consequently, no JDBC tutorial example is available to demonstrate the `Array` JDBC data type.

The following topics are covered:

- [Creating Array Objects](#creating_array)
- [Retrieving and Accessing Array Values in ResultSet](#retrieving_array)
- [Storing and Updating Array Objects](#storing_array)
- [Releasing Array Resources](#releasing_array)

## <a name="creating_array" id="creating_array">Creating Array Objects</a>

Use the method `Connection.createArrayOf` to create `Array` objects.

For example, suppose your database contains a table named `REGIONS`, which has been created and populated with the following SQL statements; note that the syntax of these statements will vary depending on your database:

```

create table REGIONS
    (REGION_NAME varchar(32) NOT NULL,
    ZIPS varchar32 ARRAY[10] NOT NULL,
    PRIMARY KEY (REGION_NAME));

insert into REGIONS values(
    'Northwest',
    '{"93101", "97201", "99210"}');
insert into REGIONS values(
    'Southwest',
    '{"94105", "90049", "92027"}');

```

```

Connection con = DriverManager.getConnection(url, props);
String [] northEastRegion = { "10022", "02110", "07399" };
Array aArray = con.createArrayOf("VARCHAR", northEastRegionnewYork);

```

The Oracle Database JDBC driver implements the `java.sql.Array` interface with the `oracle.sql.ARRAY` class.

## <a name="retrieving_array" id="retrieving_array">Retrieving and Accessing Array Values in ResultSet</a>

As with the JDBC 4.0 large object interfaces (`Blob`, `Clob`, `NClob`), you can manipulate `Array` objects without having to bring all of their data from the database server to your client computer. An `Array` object materializes the SQL `ARRAY` it represents as either a result set or a Java array.

The following excerpt retrieves the SQL `ARRAY` value in the column `ZIPS` and assigns it to the `java.sql.Array` object `z` object. The excerpt retrieves the contents of `z` and stores it in `zips`, a Java array that contains objects of type `String`. The excerpt iterates through the `zips` array and checks that each postal (zip) code is valid. This code assumes that the class `ZipCode` has been defined previously with the method `isValid` returning `true` if the given zip code matches one of the zip codes in a master list of valid zip codes:

```

ResultSet rs = stmt.executeQuery(
    "SELECT region_name, zips FROM REGIONS");

while (rs.next()) {
    Array z = rs.getArray("ZIPS");
    String[] zips = (String[])z.getArray();
    for (int i = 0; i &lt; zips.length; i++) {
        if (!ZipCode.isValid(zips[i])) {
            // ...
            // Code to display warning
        }
    }
}

```

In the following statement, the `ResultSet` method `getArray` returns the value stored in the column `ZIPS` of the current row as the `java.sql.Array` object `z`:

```

Array z = rs.getArray("ZIPS");

```

The variable `**z**` contains a locator, which is a logical pointer to the SQL `ARRAY` on the server; it does not contain the elements of the `ARRAY` itself. Being a logical pointer, `**z**` can be used to manipulate the array on the server.

In the following line, `getArray` is the `Array.getArray` method, not the `ResultSet.getArray` method used in the previous line. Because the `Array.getArray` method returns an `Object` in the Java programming language and because each zip code is a `String` object, the result is cast to an array of `String` objects before being assigned to the variable `zips`.

```

String[] zips = (String[])z.getArray();

```

The `Array.getArray` method materializes the SQL `ARRAY` elements on the client as an array of `String` objects. Because, in effect, the variable `**zips**` contains the elements of the array, it is possible to iterate through `zips` in a `for` loop, looking for zip codes that are not valid.

## <a name="storing_array" id="storing_array">Storing and Updating Array Objects</a>

Use the methods `PreparedStatement.setArray` and `PreparedStatement.setObject` to pass an `Array` value as an input parameter to a `PreparedStatement` object.

The following example sets the `Array` object `northEastRegion` (created in a previous example) as the second parameter to the PreparedStatement `pstmt`:

```

PreparedStatement pstmt = con.prepareStatement(
    "insert into REGIONS (region_name, zips) " + "VALUES (?, ?)");
pstmt.setString(1, "NorthEast");
pstmt.setArray(2, northEastRegion);
pstmt.executeUpdate();

```

Similarly, use the methods `PreparedStatement.updateArray` and `PreparedStatement.updateObject` to update a column in a table with an `Array` value.

## <a name="releasing_array" id="releasing_array">Releasing Array Resources</a>

`Array` objects remain valid for at least the duration of the transaction in which they are created. This could potentially result in an application running out of resources during a long running transaction. Applications may release `Array` resources by invoking their `free` method.

In the following excerpt, the method `Array.free` is called to release the resources held for a previously created `Array` object.

```

Array aArray = con.createArrayOf("VARCHAR", northEastRegionnewYork);
// ...
aArray.free();

```
