
# Using Stored Procedures

A stored procedure is a group of SQL statements that form a logical unit and perform a particular task, and they are used to encapsulate a set of operations or queries to execute on a database server. For example, operations on an employee database (hire, fire, promote, lookup) could be coded as stored procedures executed by application code. Stored procedures can be compiled and executed with different parameters and results, and they can have any combination of input, output, and input/output parameters.

Note that stored procedures are supported by most DBMSs, but there is a fair amount of variation in their syntax and capabilities. Consequently, the tutorial contains two classes, `[StoredProcedureJavaDBSample](gettingstarted.html)` and `[StoredProcedureMySQLSample](gettingstarted.html)` to demonstrate how to create stored procedures in Java DB and MySQL, respectively.

This page covers the following topics:

- [Overview of Stored Procedures Examples](#overview_stored_procedures_examples)
- [Parameter Modes](#parameter_modes)
<li>[Creating Stored Procedures in Java DB](#creating_stored_procedures_java_db)
<ul>
- [Creating Stored Procedures in Java DB with SQL Scripts or JDBC API](#creating_javadb_sql_jdbc)
- [Creating Stored Procedures in Java DB](#creating_stored_procedures_java_db)
- [Package Java Class in JAR File](#package_java_class_in_jar_file)

- [Creating Stored Procedure in MySQL with SQL Scripts or JDBC API](#create_jdbc_mysql)

## <a name="overview_stored_procedures_examples" id="overview_stored_procedures_examples">Overview of Stored Procedures Examples</a>

The examples `[StoredProcedureJavaDBSample.java](gettingstarted.html)` and `[StoredProcedureMySQLSample.java](gettingstarted.html)` create and call the following stored procedures:

<li>
`SHOW_SUPPLIERS`: Prints a result set that contains the names of coffee suppliers and the coffees they supply to The Coffee Break. This stored procedure does not require any parameters. When the example calls this stored procedure, the example produces output similar to the following:
<pre><code>
Acme, Inc.: Colombian_Decaf
Acme, Inc.: Colombian
Superior Coffee: French_Roast_Decaf
Superior Coffee: French_Roast
The High Ground: Espresso
</code></pre>
</li>
<li>
`GET_SUPPLIER_OF_COFFEE`: Prints the name of the supplier `supplierName` for the coffee `coffeeName`. It requires the following parameters:
<ul>
- `IN coffeeName varchar(32)`: The name of the coffee
- `OUT supplierName varchar(40)`: The name of the coffee supplier

When the example calls this stored procedure with `Colombian` as the value for `coffeeName`, the example produces output similar to the following:

```

Supplier of the coffee Colombian: Acme, Inc.

```

`RAISE_PRICE`: Raises the price of the coffee `coffeeName` to the price `newPrice`. If the price increase is greater than the percentage `maximumPercentage`, then the price is raised by that percentage. This procedure will not change the price if the price `newPrice` is lower than the original price of the coffee. It requires the following parameters:

- `IN coffeeName varchar(32)`: The name of the coffee
- `IN maximumPercentage float`: The maximum percentage to raise the coffee's price
- `INOUT newPrice numeric(10,2)`: The new price of the coffee. After the `RAISE_PRICE` stored procedure has been called, this parameter will contain the current price of the coffee `coffeeName`.

When the example calls this stored procedure with `Colombian` as the value for `coffeeName`, `0.10` as the value for `maximumPercentage`, and `19.99` as the value for `newPrice`, the example produces output similar to the following:

```

Contents of COFFEES table before calling RAISE_PRICE:
Colombian, 101, **7.99**, 0, 0
Colombian_Decaf, 101, 8.99, 0, 0
Espresso, 150, 9.99, 0, 0
French_Roast, 49, 8.99, 0, 0
French_Roast_Decaf, 49, 9.99, 0, 0

Calling the procedure RAISE_PRICE

Value of newPrice after calling RAISE_PRICE: **8.79**

Contents of COFFEES table after calling RAISE_PRICE:
Colombian, 101, **8.79**, 0, 0
Colombian_Decaf, 101, 8.99, 0, 0
Espresso, 150, 9.99, 0, 0
French_Roast, 49, 8.99, 0, 0
French_Roast_Decaf, 49, 9.99, 0, 0

```

## <a name="parameter_modes" id="parameter_modes">Parameter Modes</a>

The parameter attributes `IN` (the default), `OUT`, and `INOUT` are parameter modes. They define the action of formal parameters. The following table summarizes the information about parameter modes.

<th id="h1">Characteristic of Parameter Mode</th><th id="h2">IN</th><th id="h3">OUT</th><th id="h4">INOUT</th>
|------
<td headers="h1">Must it be specified in the stored procedure definition?</td><td headers="h2">No; if omitted, then the parameter mode of the formal parameter is `IN`.</td><td headers="h3">Must be specified.</td><td headers="h4">Must be specified.</td>

No; if omitted, then the parameter mode of the formal parameter is `IN`.

Must be specified.
<td headers="h1">Does the parameter pass a value to the stored procedure or return a value?</td><td headers="h2">Passes values to a stored procedure.</td><td headers="h3">Returns values to the caller.</td><td headers="h4">Both; passes initial values to a stored procedure; returns updated values to the caller.</td>

Passes values to a stored procedure.

Both; passes initial values to a stored procedure; returns updated values to the caller.
<td headers="h1">Does the formal parameter act as a constant or a variable in the stored procedure?</td><td headers="h2">Formal parameter acts like a constant.</td><td headers="h3">Formal parameter acts like an uninitialized variable.</td><td headers="h4">Formal parameter acts like an initialized variable.</td>

Formal parameter acts like a constant.

Formal parameter acts like an initialized variable.
<td headers="h1">Can the formal parameter be assigned a value in the stored procedure?</td><td headers="h2">Formal parameter cannot be assigned a value.</td><td headers="h3">Formal parameter cannot be used in an expression; must be assigned a value.</td><td headers="h4">Formal parameter must be assigned a value.</td>

Formal parameter cannot be assigned a value.

Formal parameter must be assigned a value.
<td headers="h1">What kinds of actual parameters (arguments) can be passed to the stored procedure?</td><td headers="h2">Actual parameter can be a constant, initialized variable, literal, or expression.</td><td headers="h3">Actual parameter must be a variable.</td><td headers="h4">Actual parameter must be a variable.</td>

Actual parameter can be a constant, initialized variable, literal, or expression.

Actual parameter must be a variable.

## <a name="creating_stored_procedures_java_db" id="creating_stored_procedures_java_db">Creating Stored Procedures in Java DB</a>

**Note**: See the section "CREATE PROCEDURE statement" in [**Java DB Reference Manual**](https://docs.oracle.com/javadb/index_jdk8.html) for more information about creating stored procedures in Java DB.

Creating and using a stored procedure in Java DB involves the following steps:

1. [Create a public static Java method in a Java class](#creating_public_static_java_method): This method performs the required task of the stored procedure.
1. [Create the stored procedure](#creating_javadb_sql_jdbc): This stored procedure calls the Java method you created.
1. [Package the Java class (that contains the public static Java method you created earlier) in a JAR file.](#package_java_class_in_jar_file)
1. Call the stored procedure with the `CALL` SQL statement. See the section [Calling Stored Procedures in Java DB and MySQL](#calling_javadb_mysql).

### <a name="creating_public_static_java_method" id="creating_public_static_java_method">Creating Public Static Java Method</a>

The following method, `[StoredProcedureJavaDBSample.showSuppliers](gettingstarted.html)`, contains the SQL statements that the stored procedure `SHOW_SUPPLIERS` calls:

```

public static void showSuppliers(ResultSet[] rs)
    throws SQLException {

    Connection con = DriverManager.getConnection("jdbc:default:connection");
    Statement stmt = null;

    String query =
        "select SUPPLIERS.SUP_NAME, " +
        "COFFEES.COF_NAME " +
        "from SUPPLIERS, COFFEES " +
        "where SUPPLIERS.SUP_ID = " +
        "COFFEES.SUP_ID " +
        "order by SUP_NAME";

    stmt = con.createStatement();
    rs[0] = stmt.executeQuery(query);
}

```

The `SHOW_SUPPLIERS` stored procedure takes no arguments. You can specify arguments in a stored procedure by defining them in the method signature of your public static Java method. Note that the method `showSuppliers` contains a parameter of type `ResultSet[]`. If your stored procedure returns any number of `ResultSet` objects, specify one parameter of type `ResultSet[]` in your Java method. In addition, ensure that this Java method is public and static.

Retrieve the `Connection` object from the URL `jdbc:default:connection`. This is a convention in Java DB to indicate that the stored procedure will use the currently existing `Connection` object.

Note that the `Statement` object is not closed in this method. Do not close any `Statement` objects in the Java method of your stored procedure; if you do so, the `ResultSet` object will not exist when you issue the `CALL` statement when you call your stored procedure.

In order for the stored procedure to return a generated result set, you must assign the result set to an array component of the `ResultSet[]` parameter. In this example, the generated result set is assigned to the array component `rs[0]`.

The following method is `[StoredProcedureJavaDBSample.showSuppliers](gettingstarted.html)`:

```

public static void getSupplierOfCoffee(String coffeeName, String[] supplierName)
    throws SQLException {

    Connection con = DriverManager.getConnection("jdbc:default:connection");
    PreparedStatement pstmt = null;
    ResultSet rs = null;

    String query =
        "select SUPPLIERS.SUP_NAME " +
        "from SUPPLIERS, COFFEES " +
        "where " +
        "SUPPLIERS.SUP_ID = COFFEES.SUP_ID " +
        "and ? = COFFEES.COF_NAME";

    pstmt = con.prepareStatement(query);
    pstmt.setString(1, coffeeName);
    rs = pstmt.executeQuery();

    if (rs.next()) {
        supplierName[0] = rs.getString(1);
    } else {
        supplierName[0] = null;
    }
}

```

The formal parameter `coffeeName` has the parameter mode `IN`. This formal parameter is used like any other parameter in a Java method. Because the formal parameter `supplierName` has the parameter mode `OUT`, it must use a one dimensional array data type. Because this method does not produce a result set, the method definition does not contain a parameter of type `ResultSet[]`. In order to retrieve a value from an `OUT` formal parameter, you must assign the value to be retrieved to an array component of the `OUT` formal parameter. In this example, the retrieved name of the coffee supplier is assigned to the array component `supplierName[0]`.

The following is the method signature of the `[StoredProcedureJavaDBSample.raisePrice](gettingstarted.html) method:`

```

public static void raisePrice(
   String coffeeName, double maximumPercentage,
   BigDecimal[] newPrice) throws SQLException

```

Because the formal parameter `newPrice` has the parameter mode `INOUT`, it must use a one dimensional array data type. Java DB maps the `FLOAT` and `NUMERIC` SQL data types to the `double` and `java.math.BigDecimal` Java data types, respectively.

### <a name="creating_javadb_sql_jdbc" id="creating_javadb_sql_jdbc">Creating Stored Procedures in Java DB with SQL Scripts or JDBC API</a>

Java DB uses the Java programming language for its stored procedures. Consequently, when you define a stored procedure, you specify which Java class to call and where Java DB can find it.

The following excerpt from `[StoredProcedureJavaDBSample.createProcedures](gettingstarted.html)` creates a stored procedure named `SHOW_SUPPLIERS`:

```

public void createProcedures(Connection con)
    throws SQLException {

    Statement stmtCreateShowSuppliers = null;

    // ...

    String queryShowSuppliers =
        "CREATE PROCEDURE SHOW_SUPPLIERS() " +
        "PARAMETER STYLE JAVA " +
        "LANGUAGE JAVA " +
        "DYNAMIC RESULT SETS 1 " +
        "EXTERNAL NAME " +
        "'com.oracle.tutorial.jdbc." +
        "StoredProcedureJavaDBSample." +
        "showSuppliers'";

    // ...

    try {
        System.out.println("Calling CREATE PROCEDURE");
        stmtCreateShowSuppliers = con.createStatement();

        // ...

    } catch (SQLException e) {
        JDBCTutorialUtilities.printSQLException(e);
    } finally {
        if (stmtCreateShowSuppliers != null) {
            stmtCreateShowSuppliers.close();
        }
        // ...
    }
}

```

The following list describes the procedure elements you can specify in the `CREATE PROCEDURE` statement:

<li>`PARAMETER STYLE`: Identifies the convention used to pass parameters to the stored procedure. The following options are valid:
<ul>
- `JAVA`: Specifies that the stored procedure uses a parameter-passing convention that conforms to the Java language and the SQL routines specification.
- `DERBY`: Specifies that the stored procedure supports a vararg as the final argument in the parameter list.

The following statement (which is found in `[StoredProcedureJavaDBSample.createProcedures](gettingstarted.html)`)
creates a stored procedure named `GET_SUPPLIERS_OF_COFFEE` (line breaks have been added for clarity):

```

CREATE PROCEDURE GET_SUPPLIER_OF_COFFEE(
    IN coffeeName varchar(32),
    OUT supplierName
    varchar(40))
    PARAMETER STYLE JAVA
    LANGUAGE JAVA
    DYNAMIC RESULT SETS 0
    EXTERNAL NAME 'com.oracle.tutorial.jdbc.
        StoredProcedureJavaDBSample.
        getSupplierOfCoffee'

```

This stored procedure has two formal parameters, `coffeeName` and `supplierName`. The parameter specifiers `IN` and `OUT` are called parameter modes. They define the action of formal parameters. See [Parameter Modes](#parameter_modes) for more information. This stored procedure does not retrieve a result set, so the procedure element `DYNAMIC RESULT SETS` is `0`.

The following statement creates a stored procedure named `RAISE_PRICE` (line breaks have been added for clarity):

```

CREATE PROCEDURE RAISE_PRICE(
    IN coffeeName varchar(32),
    IN maximumPercentage float,
    INOUT newPrice float)
    PARAMETER STYLE JAVA
    LANGUAGE JAVA
    DYNAMIC RESULT SETS 0
    EXTERNAL NAME 'com.oracle.tutorial.jdbc.
        StoredProcedureJavaDBSample.raisePrice'

```

You can use SQL scripts to create stored procedures in Java DB. See the script `[javadb/create-procedures.sql](gettingstarted.html)` and the Ant target `javadb-create-procedure` in the `[build.xml](gettingstarted.html)` Ant build script.

### <a name="package_java_class_in_jar_file" id="package_java_class_in_jar_file">Package Java Class in JAR File</a>

The Ant build script `[build.xml](gettingstarted.html)` contains targets to compile and package the tutorial in a JAR file. At a command prompt, change the current directory to `**&lt;JDBC tutorial directory&gt;**`. From this directory, run the following command to compile and package the tutorial in a JAR file:

`ant jar`

The name of the JAR file is `**&lt;JDBC tutorial directory&gt;**/lib/JDBCTutorial.jar`.

The Ant build script adds the file `JDBCTutorial.jar` to the class path. You can also specify the location of the JAR file in your `CLASSPATH` environment variable. This enables Java DB to find the Java method that the stored procedure calls.

#### Adding JAR File Directly to Database

Java DB looks first in your class path for any required classes, and then in the database. This section shows you how to add JAR files directly to the database.

Use the following system procedures to add the `JDBCTutorial.jar` JAR file to the database (line breaks have been added for clarity):

```

CALL sqlj.install_jar(
  '**&lt;JDBC tutorial directory&gt;**/
  lib/JDBCTutorial.jar',
  'APP.JDBCTutorial', 0)
CALL sqlj.replace_jar(
  '**&lt;JDBC tutorial directory&gt;**/
  lib/JDBCTutorial.jar',
  'APP.JDBCTutorial')";
CALL syscs_util.syscs_set_database_property(
  'derby.database.classpath',
  'APP.JDBCTutorial')";

```

**Note**: The method [`StoredProcedureJavaDBSample.registerJarFile`](gettingstarted.html) demonstrates how to call these system procedures. If you call this method, ensure that you have modified `[javadb-sample-properties.xml](gettingstarted.html)` so that the value of the property `jar_file` is set to the full path name of `JDBCTutorial.jar`.

The `install_jar` procedure in the `SQL` schema adds a JAR file to the database. The first argument of this procedure is the full path name of the JAR file on the computer from which this procedure is run. The second argument is an identifier that Java DB uses to refer to the JAR file. (The identifier `APP` is the Java DB default schema.) The `replace_jar` procedure replaces a JAR file already in the database.

The system procedure `SYSCS_UTIL.SYSCS_SET_DATABASE_PROPERTY` sets or deletes the value of a property of the database on the current connection. This method sets the property `derby.database.classpath` to the identifier specified in the `install_jar` file. Java DB first looks in your Java class path for a class, then it looks in `derby.database.classpath`.

## <a name="creating_stored_procedure_mysql" id="creating_stored_procedure_mysql">Creating Stored Procedure in MySQL</a>

Creating and using a stored procedure in Java DB involves the following steps:

1. [Create the stored procedure with an SQL script or JDBC API](#create_jdbc_mysql)
1. Call the stored procedure with the `CALL` SQL statement. See the section [Calling Stored Procedures in Java DB and MySQL](#calling_javadb_mysql)

### <a name="create_jdbc_mysql" id="create_jdbc_mysql">Creating Stored Procedure in MySQL with SQL Scripts or JDBC API</a>

MySQL uses a SQL-based syntax for its stored procedures. The following excerpt from the SQL script `[mysql/create-procedures.sql](gettingstarted.html)` creates a stored procedure named `SHOW_SUPPLIERS`:

```

SELECT 'Dropping procedure SHOW_SUPPLIERS' AS ' '|
drop procedure if exists SHOW_SUPPLIERS|

# ...

SELECT 'Creating procedure SHOW_SUPPLIERS' AS ' '|
create procedure SHOW_SUPPLIERS()
    begin
        select SUPPLIERS.SUP_NAME,
        COFFEES.COF_NAME
        from SUPPLIERS, COFFEES
        where SUPPLIERS.SUP_ID = COFFEES.SUP_ID
        order by SUP_NAME;
    end|

```

The `DROP PROCEDURE` statement deletes that procedure `SHOW_SUPPLIERS` if it exists. In MySQL, statements in a stored procedure are separated by semicolons. However, a different delimiter is required to end the `create procedure` statement. This example uses the pipe (`|`) character; you can use another character (or more than one character). This character that separates statements is defined in the `delimiter` attribute in the Ant target that calls this script. This excerpt is from the Ant build file `[build.xml](gettingstarted.html)` (line breaks have been inserted for clarity):

```

&lt;target name="mysql-create-procedure"&gt;

  &lt;sql driver="${DB.DRIVER}"
       url="${DB.URL}" userid="${DB.USER}"
       password="${DB.PASSWORD}"
       classpathref="CLASSPATH"
       print="true"
       **delimiter="|"**
       autocommit="false"
       onerror="abort"&gt;
       &lt;transaction
         src="./sql/${DB.VENDOR}/
           create-procedures.sql"&gt;
       &lt;/transaction&gt;
  &lt;/sql&gt;

&lt;/target&gt;

```

Alternatively, you can use the `DELIMITER` SQL statement to specify a different delimiter character.

The `CREATE PROCEDURE` statement consists of the name of the procedure, a comma-separated list of parameters in parentheses, and SQL statements within the `BEGIN` and `END` keywords.

You can use the JDBC API to create a stored procedure. The following method, `[StoredProcedureMySQLSample.createProcedureShowSuppliers](gettingstarted.html)`, performs the same tasks as the previous script:

```

  public void createProcedureShowSuppliers() throws SQLException {
    
    String queryDrop = "DROP PROCEDURE IF EXISTS SHOW_SUPPLIERS";

    String createProcedure =
        "create procedure SHOW_SUPPLIERS() " +
          "begin " +
            "select SUPPLIERS.SUP_NAME, COFFEES.COF_NAME " +
              "from SUPPLIERS, COFFEES " +
              "where SUPPLIERS.SUP_ID = COFFEES.SUP_ID " +
              "order by SUP_NAME; " +
          "end";

    try (Statement stmtDrop = con.createStatement()) {
      System.out.println("Calling DROP PROCEDURE");
      stmtDrop.execute(queryDrop);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    } 

    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createProcedure);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

Note that the delimiter has not been changed in this method.

The stored procedure `SHOW_SUPPLIERS` generates a result set, even though the return type of the method `createProcedureShowSuppliers` is `void` and the method does not contain any parameters. A result set is returned when the stored procedure `SHOW_SUPPLIERS` is called with the method `CallableStatement.executeQuery`:

```

CallableStatement cs = null;
cs = this.con.prepareCall("{call SHOW_SUPPLIERS}");
ResultSet rs = cs.executeQuery();

```

The following excerpt from the method `[StoredProcedureMySQLSample.createProcedureGetSupplierOfCoffee](gettingstarted.html)` contains the SQL query that creates a stored procedure named `GET_SUPPLIER_OF_COFFEE`:

```

  public void createProcedureGetSupplierOfCoffee() throws SQLException {

    String queryDrop = "DROP PROCEDURE IF EXISTS GET_SUPPLIER_OF_COFFEE";

    String createProcedure =
        "create procedure GET_SUPPLIER_OF_COFFEE(IN coffeeName varchar(32), OUT supplierName varchar(40)) " +
          "begin " +
            "select SUPPLIERS.SUP_NAME into supplierName " +
              "from SUPPLIERS, COFFEES " +
              "where SUPPLIERS.SUP_ID = COFFEES.SUP_ID " +
              "and coffeeName = COFFEES.COF_NAME; " +
            "select supplierName; " +
          "end";

    try (Statement stmtDrop = con.createStatement()) {
      System.out.println("Calling DROP PROCEDURE");
      stmtDrop.execute(queryDrop);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }

    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createProcedure);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

This stored procedure has two formal parameters, `coffeeName` and `supplierName`. The parameter specifiers `IN` and `OUT` are called parameter modes. They define the action of formal parameters. See [Parameter Modes](#parameter_modes) for more information. The formal parameters are defined in the SQL query, not in the method `createProcedureGetSupplierOfCoffee`. To assign a value to the `OUT` parameter `supplierName`, this stored procedure uses a `SELECT` statement.

The following excerpt from the method `[StoredProcedureMySQLSample.createProcedureRaisePrice](gettingstarted.html)` contains the SQL query that creates a stored procedure named `RAISE_PRICE`:

```

  public void createProcedureRaisePrice() throws SQLException {
    
    String queryDrop = "DROP PROCEDURE IF EXISTS RAISE_PRICE";

    String createProcedure =
        "create procedure RAISE_PRICE(IN coffeeName varchar(32), IN maximumPercentage float, INOUT newPrice numeric(10,2)) " +
          "begin " +
            "main: BEGIN " +
              "declare maximumNewPrice numeric(10,2); " +
              "declare oldPrice numeric(10,2); " +
              "select COFFEES.PRICE into oldPrice " +
                "from COFFEES " +
                "where COFFEES.COF_NAME = coffeeName; " +
              "set maximumNewPrice = oldPrice * (1 + maximumPercentage); " +
              "if (newPrice &gt; maximumNewPrice) " +
                "then set newPrice = maximumNewPrice; " +
              "end if; " +
              "if (newPrice &lt;= oldPrice) " +
                "then set newPrice = oldPrice;" +
                "leave main; " +
              "end if; " +
              "update COFFEES " +
                "set COFFEES.PRICE = newPrice " +
                "where COFFEES.COF_NAME = coffeeName; " +
              "select newPrice; " +
            "END main; " +
          "end";

    try (Statement stmtDrop = con.createStatement()) {
      System.out.println("Calling DROP PROCEDURE");
      stmtDrop.execute(queryDrop);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }

    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createProcedure);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

The stored procedure assigns a value to the `INOUT` parameter `newPrice` with the `SET` and `SELECT` statements. To exit the stored procedure, the stored procedure first encloses the statements in a `BEGIN ... END` block labeled `main`. To exit the procedure, the method uses the statement `leave main`.

## <a name="calling_javadb_mysql" id="calling_javadb_mysql">Calling Stored Procedures in Java DB and MySQL</a>

The following excerpt from the method `[runStoredProcedures](gettingstarted.html)`, calls the stored procedure `SHOW_SUPPLIERS` and prints the generated result set:

```

cs = this.con.prepareCall("{call SHOW_SUPPLIERS()}");
ResultSet rs = cs.executeQuery();

while (rs.next()) {
    String supplier = rs.getString("SUP_NAME");
    String coffee = rs.getString("COF_NAME");
    System.out.println(supplier + ": " + coffee);
}

```

**Note**: As with `Statement` objects, to call the stored procedure, you can call `execute`, `executeQuery`, or `executeUpdate` depending on how many `ResultSet` objects the procedure returns. However, if you are not sure how many `ResultSet` objects the procedure returns, call `execute`.

Calling the stored procedure `SHOW_SUPPLIERS` is demonstrated in the section [Creating Stored Procedure with JDBC API in MySQL](#create_jdbc_mysql).

The following excerpt from method `[runStoredProcedures](gettingstarted.html)`, calls the stored procedure `GET_SUPPLIER_OF_COFFEE`:

```

cs = this.con.prepareCall("{call GET_SUPPLIER_OF_COFFEE(?, ?)}");
cs.setString(1, coffeeNameArg);
cs.registerOutParameter(2, Types.VARCHAR);
cs.executeQuery();

String supplierName = cs.getString(2);

```

The interface `CallableStatement` extends `PreparedStatement`. It is used to call stored procedures. Specify values for `IN` parameters (such as `coffeeName` in this example) just like you would with a `PreparedStatement` object by calling the appropriate setter method. However, if a stored procedure contains an `OUT` parameter, you must register it with the `registerOutParameter` method.

The following excerpt from the method `[runStoredProcedures](gettingstarted.html)`, calls the stored procedure `RAISE_PRICE`:

```

cs = this.con.prepareCall("{call RAISE_PRICE(?,?,?)}");
cs.setString(1, coffeeNameArg);
cs.setFloat(2, maximumPercentageArg);
cs.registerOutParameter(3, Types.NUMERIC);
cs.setFloat(3, newPriceArg);

cs.execute();

```

Because the parameter `newPrice` (the third parameter in the procedure `RAISE_PRICE`) has the parameter mode `INOUT`, you must both specify its value by calling the appropriate setter method and register it with the `registerOutParameter` method.
