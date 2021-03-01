
# Using Advanced Data Types

The advanced data types introduced in this section give a relational database more flexibility in what can be used as a value for a table column. For example, a column can be used to store `BLOB` (binary large object) values, which can store very large amounts of data as raw bytes. A column can also be of type `CLOB` (character large object), which is capable of storing very large amounts of data in character format.

The latest version of the ANSI/ISO SQL standard is commonly referred to as SQL:2003. This standard specifies the following data types:

<li>
SQL92 built-in types, which consist of the familiar SQL column types such as `CHAR`, `FLOAT`, and `DATE`
</li>
<li>
SQL99 built-in types, which consist of types added by SQL99:
<ul>
<li>
`BOOLEAN`: Boolean (true or false) value
</li>
<li>
`BLOB`: Binary large Bobject
</li>
<li>
`CLOB`: Character large object
</li>

New built-in types added by SQL:2003:

<li>
`XML`: XML object
</li>

User defined types:

<li>
Structured type: User-defined type; for example:
<pre><code>
CREATE TYPE PLANE_POINT
AS (X FLOAT, Y FLOAT) NOT FINAL
</code></pre>
</li>
<li>
`DISTINCT` type: User-defined type based on a built-in type; for example:
<pre><code>
CREATE TYPE MONEY
AS NUMERIC(10,2) FINAL
</code></pre>
</li>

Constructed types: New types based on a given base type:

<li>
`REF(**structured-type**)`: Pointer that persistently denotes an instance of a structured type that resides in the database
</li>
<li>
`**base-type** ARRAY[**n**]`: Array of **n** base-type elements
</li>

Locators: Entities that are logical pointers to data that resides on the database server. A **locator** exists in the client computer and is a transient, logical pointer to data on the server. A locator typically refers to data that is too large to materialize on the client, such as images or audio. (**Materialized views** are query results that have been stored or "materialized" in advance as schema objects.) There are operators defined at the SQL level to retrieve randomly accessed pieces of the data denoted by the locator:

<li>
`LOCATOR(**structured-type**)`: Locator to a structured instance in the server
</li>
<li>
`LOCATOR(**array**)`: Locator to an array in the server
</li>
<li>
`LOCATOR(**blob**)`: Locator to a binary large object in the server
</li>
<li>
`LOCATOR(**clob**)`: Locator to a character large object in the server
</li>

`Datalink`: Type for managing data external to the data source. `Datalink` values are part of SQL MED (Management of External Data), a part of the SQL ANSI/ISO standard specification.

## Mapping Advanced Data Types

The JDBC API provides default mappings for advanced data types specified by the SQL:2003 standard. The following list gives the data types and the interfaces or classes to which they are mapped:

- `BLOB`: `Blob` interface
- `CLOB`: `Clob` interface
- `NCLOB`: `NClob` interface
- `ARRAY`: `Array` interface
- `XML`: `SQLXML` interface
- Structured types: `Struct` interface
- `REF(structured type)`: `Ref` interface
- `ROWID`: `RowId` interface
- `DISTINCT`: Type to which the base type is mapped. For example, a `DISTINCT` value based on a SQL `NUMERIC` type maps to a `java.math.BigDecimal` type because `NUMERIC` maps to `BigDecimal` in the Java programming language.
- `DATALINK`: `java.net.URL` object

## Using Advanced Data Types

You retrieve, store, and update advanced data types the same way you handle other data types. You use either `ResultSet.get**DataType**` or `CallableStatement.get**DataType**` methods to retrieve them, `PreparedStatement.set**DataType**` methods to store them, and `ResultSet.update**DataType**` methods to update them. (The variable `**DataType**` is the name of a Java interface or class mapped to an advanced data type.) Probably 90 percent of the operations performed on advanced data types involve using the `get**DataType**`, `set**DataType**`, and `update**DataType**` methods. The following table shows which methods to use:
<th id="h1">**Advanced Data Type**</th><th id="h2">**`get**DataType**` Method**</th><th id="h3">**`set**DataType**` method**</th><th id="h4">**`update**DataType**` Method**</th>
<td headers="h1">`BLOB`</td><td headers="h2">`getBlob`</td><td headers="h3">`setBlob`</td><td headers="h4">`updateBlob`</td>
<td headers="h1">`CLOB`</td><td headers="h2">`getClob`</td><td headers="h3">`setClob`</td><td headers="h4">`updateClob`</td>
<td headers="h1">`NCLOB`</td><td headers="h2">`getNClob`</td><td headers="h3">`setNClob`</td><td headers="h4">`updateNClob`</td>
<td headers="h1">`ARRAY`</td><td headers="h2">`getArray`</td><td headers="h3">`setArray`</td><td headers="h4">`updateArray`</td>
<td headers="h1">`XML`</td><td headers="h2">`getSQLXML`</td><td headers="h3">`setSQLXML`</td><td headers="h4">`updateSQLXML`</td>
<td headers="h1">`Structured type`</td><td headers="h2">`getObject`</td><td headers="h3">`setObject`</td><td headers="h4">`updateObject`</td>
<td headers="h1">`REF(structured type)`</td><td headers="h2">`getRef`</td><td headers="h3">`setRef`</td><td headers="h4">`updateRef`</td>
<td headers="h1">`ROWID`</td><td headers="h2">`getRowId`</td><td headers="h3">`setRowId`</td><td headers="h4">`updateRowId`</td>
<td headers="h1">`DISTINCT`</td><td headers="h2">`getBigDecimal`</td><td headers="h3">`setBigDecimal`</td><td headers="h4">`updateBigDecimal`</td>
<td headers="h1">`DATALINK`</td><td headers="h2">`getURL`</td><td headers="h3">`setURL`</td><td headers="h4">`updateURL`</td>

**Note**: The `DISTINCT` data type behaves differently from other advanced SQL data types. Being a user-defined type that is based on an already existing built-in types, it has no interface as its mapping in the Java programming language. Consequently, you use the method that corresponds to the Java type on which the `DISTINCT` data type is based. See [Using DISTINCT Data Type](distinct.html) for more information.

For example, the following code fragment retrieves a SQL `ARRAY` value. For this example, suppose that the column `SCORES` in the table `STUDENTS` contains values of type `ARRAY`. The variable `**stmt**` is a `Statement` object.

```

ResultSet rs = stmt.executeQuery(
    "SELECT SCORES FROM STUDENTS " +
    "WHERE ID = 002238");
rs.next();
Array scores = rs.getArray("SCORES");

```

The variable `**scores**` is a logical pointer to the SQL `ARRAY` object stored in the table `STUDENTS` in the row for student `002238`.

If you want to store a value in the database, you use the appropriate `set` method. For example, the following code fragment, in which `**rs**` is a `ResultSet` object, stores a `Clob` object:

```

Clob notes = rs.getClob("NOTES");
PreparedStatement pstmt =
    con.prepareStatement(
        "UPDATE MARKETS SET COMMENTS = ? " +
        "WHERE SALES &lt; 1000000");
pstmt.setClob(1, notes);
pstmt.executeUpdate();

```

This code sets `**notes**` as the first parameter in the update statement being sent to the database. The `Clob` value designated by `**notes**` will be stored in the table `MARKETS` in column `COMMENTS` in every row where the value in the column `SALES` is less than one million.
