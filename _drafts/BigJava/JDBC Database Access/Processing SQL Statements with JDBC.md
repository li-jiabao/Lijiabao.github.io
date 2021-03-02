
# Processing SQL Statements with JDBC

In general, to process any SQL statement with JDBC, you follow these steps:

1. [Establishing a connection.](#establishing_connections)
1. [Create a statement.](#creating_statements)
1. [Execute the query.](#executing_queries)
1. [Process the `ResultSet` object.](#processing_resultset_objects)
1. [Close the connection.](#closing_connections)

This page uses the following method, `[CoffeesTables.viewTable](gettingstarted.html)`, from the tutorial sample to demonstrate these steps. This method outputs the contents of the table `COFFEES`. This method will be discussed in more detail later in this tutorial:

```

  public static void viewTable(Connection con) throws SQLException {
    String query = "select COF_NAME, SUP_ID, PRICE, SALES, TOTAL from COFFEES";
    try (Statement stmt = con.createStatement()) {
      ResultSet rs = stmt.executeQuery(query);
      while (rs.next()) {
        String coffeeName = rs.getString("COF_NAME");
        int supplierID = rs.getInt("SUP_ID");
        float price = rs.getFloat("PRICE");
        int sales = rs.getInt("SALES");
        int total = rs.getInt("TOTAL");
        System.out.println(coffeeName + ", " + supplierID + ", " + price +
                           ", " + sales + ", " + total);
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

## <a name="establishing_connections" id="establishing_connections">Establishing Connections</a>

First, establish a connection with the data source you want to use. A data source can be a DBMS, a legacy file system, or some other source of data with a corresponding JDBC driver. This connection is represented by a `Connection` object. See 
[Establishing a Connection](connecting.html) for more information.

## <a name="creating_statements" id="creating_statements">Creating Statements</a>

A `Statement` is an interface that represents a SQL statement. You execute `Statement` objects, and they generate `ResultSet` objects, which is a table of data representing a database result set. You need a `Connection` object to create a `Statement` object.

For example, `CoffeesTables.viewTable` creates a `Statement` object with the following code:

```

stmt = con.createStatement();

```

There are three different kinds of statements:

- `Statement`: Used to implement simple SQL statements with no parameters.
<li>`PreparedStatement`: (Extends `Statement`.) Used for precompiling SQL statements that might contain input parameters. See 
[Using Prepared Statements](prepared.html) for more information.</li>
<li>`CallableStatement:` (Extends `PreparedStatement`.) Used to execute stored procedures that may contain both input and output parameters. See 
[Stored Procedures](storedprocedures.html) for more information.</li>

## <a name="executing_queries" id="executing_queries">Executing Queries</a>

To execute a query, call an `execute` method from `Statement` such as the following:

- `execute`: Returns `true` if the first object that the query returns is a `ResultSet` object. Use this method if the query could return one or more `ResultSet` objects. Retrieve the `ResultSet` objects returned from the query by repeatedly calling `Statement.getResultSet`.
- `executeQuery`: Returns one `ResultSet` object.
- `executeUpdate`: Returns an integer representing the number of rows affected by the SQL statement. Use this method if you are using `INSERT`, `DELETE`, or `UPDATE` SQL statements.

For example, `CoffeesTables.viewTable` executed a `Statement` object with the following code:

```

ResultSet rs = stmt.executeQuery(query);

```

See 
[Retrieving and Modifying Values from Result Sets](retrieving.html) for more information.

## <a name="processing_resultset_objects" id="processing_resultset_objects">Processing ResultSet Objects</a>

You access the data in a `ResultSet` object through a cursor. Note that this cursor is not a database cursor. This cursor is a pointer that points to one row of data in the `ResultSet` object. Initially, the cursor is positioned before the first row. You call various methods defined in the `ResultSet` object to move the cursor.

For example, `CoffeesTables.viewTable` repeatedly calls the method `ResultSet.next` to move the cursor forward by one row. Every time it calls `next`, the method outputs the data in the row where the cursor is currently positioned:

```

      ResultSet rs = stmt.executeQuery(query);
      while (rs.next()) {
        String coffeeName = rs.getString("COF_NAME");
        int supplierID = rs.getInt("SUP_ID");
        float price = rs.getFloat("PRICE");
        int sales = rs.getInt("SALES");
        int total = rs.getInt("TOTAL");
        System.out.println(coffeeName + ", " + supplierID + ", " + price +
                           ", " + sales + ", " + total);
      }
      // ...

```

See 
[Retrieving and Modifying Values from Result Sets](retrieving.html) for more information. <!-- *************************************** -->

## <a name="closing_connections" id="closing_connections">Closing Connections</a>

When you are finished using a `Connection`, `Statement`, or `ResultSet` object, call its `close` method to immediately release the resources it's using.

Alternatively, use a `try`-with-resources statement to automatically close `Connection`, `Statement`, and `ResultSet` objects, regardless of whether an `SQLException` has been thrown. (JDBC throws an `SQLException` when it encounters an error during an interaction with a data source. See 
[Handling SQL Exceptions](sqlexception.html) for more information.)
 An automatic resource statement consists of a `try` statement and one or more declared resources. For example, the `CoffeesTables.viewTable` method automatically closes its `Statement` object, as follows:

```

  public static void viewTable(Connection con) throws SQLException {
    String query = "select COF_NAME, SUP_ID, PRICE, SALES, TOTAL from COFFEES";
    **try (Statement stmt = con.createStatement())** {
      ResultSet rs = stmt.executeQuery(query);
      while (rs.next()) {
        String coffeeName = rs.getString("COF_NAME");
        int supplierID = rs.getInt("SUP_ID");
        float price = rs.getFloat("PRICE");
        int sales = rs.getInt("SALES");
        int total = rs.getInt("TOTAL");
        System.out.println(coffeeName + ", " + supplierID + ", " + price +
                           ", " + sales + ", " + total);
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

The following statement is a `try`-with-resources statement, which declares one resource, `stmt`, that will be automatically closed when the `try` block terminates:

```

    **try (Statement stmt = con.createStatement())** {
      // ...
    }

```

See 
[The `try`-with-resources Statement](../../essential/exceptions/tryResourceClose.html) in the
[Essential Classes](../../essential/index.html) trail for more information.
