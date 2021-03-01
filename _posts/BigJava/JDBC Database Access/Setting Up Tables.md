
# Setting Up Tables

This page describes all the tables used in the JDBC tutorial and how to create them:

- [COFFEES Table](#coffees)
- [SUPPLIERS Table](#suppliers)
- [COF_INVENTORY Table](#cof_inventory)
- [MERCH_INVENTORY Table](#merch_inventory)
- [COFFEE_HOUSES Table](#coffee_houses)
- [DATA_REPOSITORY Table](#data_repository)
- [Creating Tables](#create)
- [Populating Tables](#populate)

## <a name="coffees" id="coffees">COFFEES Table</a>

The `COFFEES` table stores information about the coffees available for sale at The Coffee Break:
<th id="h1">`COF_NAME`</th><th id="h2">`SUP_ID`</th><th id="h3">`PRICE`</th><th id="h4">`SALES`</th><th id="h5">`TOTAL`</th>
<td headers="h1">Colombian</td><td headers="h2">101</td><td headers="h3">7.99</td><td headers="h4">0</td><td headers="h5">0</td>
<td headers="h1">French_Roast</td><td headers="h2">49</td><td headers="h3">8.99</td><td headers="h4">0</td><td headers="h5">0</td>
<td headers="h1">Espresso</td><td headers="h2">150</td><td headers="h3">9.99</td><td headers="h4">0</td><td headers="h5">0</td>
<td headers="h1">Colombian_Decaf</td><td headers="h2">101</td><td headers="h3">8.99</td><td headers="h4">0</td><td headers="h5">0</td>
<td headers="h1">French_Roast_Decaf</td><td headers="h2">49</td><td headers="h3">9.99</td><td headers="h4">0</td><td headers="h5">0</td>

The following describes each of the columns in the `COFFEES` table:

- `COF_NAME`: Stores the coffee name. Holds values with a SQL type of `VARCHAR` with a maximum length of 32 characters. Because the names are different for each type of coffee sold, the name uniquely identifies a particular coffee and serves as the primary key.
- `SUP_ID`: Stores a number identifying the coffee supplier. Holds values with a SQL type of `INTEGER`. It is defined as a foreign key that references the column `SUP_ID` in the `SUPPLIERS` table. Consequently, the DBMS will enforce that each value in this column matches one of the values in the corresponding column in the `SUPPLIERS` table.
- `PRICE`: Stores the cost of the coffee per pound. Holds values with a SQL type of `FLOAT` because it needs to hold values with decimal points. (Note that money values would typically be stored in a SQL type `DECIMAL` or `NUMERIC`, but because of differences among DBMSs and to avoid incompatibility with earlier versions of JDBC, the tutorial uses the more standard type `FLOAT`.)
- `SALES`: Stores the number of pounds of coffee sold during the current week. Holds values with a SQL type of `INTEGER`.
- `TOTAL`: Stores the number of pounds of coffee sold to date. Holds values with a SQL type of `INTEGER`.

## <a name="suppliers" id="suppliers">SUPPLIERS Table</a>

The `SUPPLIERS` stores information about each of the suppliers:
<th id="h101">`SUP_ID`</th><th id="h102">`SUP_NAME`</th><th id="h103">`STREET`</th><th id="h104">`CITY`</th><th id="h105">`STATE`</th><th id="h106">`ZIP`</th>
<td headers="h101">101</td><td headers="h102">Acme, Inc.</td><td headers="h103">99 Market Street</td><td headers="h104">Groundsville</td><td headers="h105">CA</td><td headers="h106">95199</td>
<td headers="h101">49</td><td headers="h102">Superior Coffee</td><td headers="h103">1 Party Place</td><td headers="h104">Mendocino</td><td headers="h105">CA</td><td headers="h106">95460</td>
<td headers="h101">150</td><td headers="h102">The High Ground</td><td headers="h103">100 Coffee Lane</td><td headers="h104">Meadows</td><td headers="h105">CA</td><td headers="h106">93966</td>

The following describes each of the columns in the `SUPPLIERS` table:

- `SUP_ID`: Stores a number identifying the coffee supplier. Holds values with a SQL type of `INTEGER`. It is the primary key in this table.
- `SUP_NAME`: Stores the name of the coffee supplier.
- `STREET`, `CITY`, `STATE`, and `ZIP`: These columns store the address of the coffee supplier.

## <a name="cof_inventory" id="cof_inventory">COF_INVENTORY Table</a>

The table `COF_INVENTORY` stores information about the amount of coffee stored in each warehouse:
<th id="h201">`WAREHOUSE_ID`</th><th id="h202">`COF_NAME`</th><th id="h203">`SUP_ID`</th><th id="h204">`QUAN`</th><th id="h205">`DATE_VAL`</th>
<td headers="h201">1234</td><td headers="h202">House_Blend</td><td headers="h203">49</td><td headers="h204">0</td><td headers="h205">2006_04_01</td>
<td headers="h201">1234</td><td headers="h202">House_Blend_Decaf</td><td headers="h203">49</td><td headers="h204">0</td><td headers="h205">2006_04_01</td>
<td headers="h201">1234</td><td headers="h202">Colombian</td><td headers="h203">101</td><td headers="h204">0</td><td headers="h205">2006_04_01</td>
<td headers="h201">1234</td><td headers="h202">French_Roast</td><td headers="h203">49</td><td headers="h204">0</td><td headers="h205">2006_04_01</td>
<td headers="h201">1234</td><td headers="h202">Espresso</td><td headers="h203">150</td><td headers="h204">0</td><td headers="h205">2006_04_01</td>
<td headers="h201">1234</td><td headers="h202">Colombian_Decaf</td><td headers="h203">101</td><td headers="h204">0</td><td headers="h205">2006_04_01</td>

The following describes each of the columns in the `COF_INVENTORY` table:

- `WAREHOUSE_ID`: Stores a number identifying a warehouse.
- `COF_NAME`: Stores the name of a particular type of coffee.
- `SUP_ID`: Stores a number identifying a supplier.
- `QUAN`: Stores a number indicating the amount of merchandise available.
- `DATE`: Stores a timestamp value indicating the last time the row was updated.

## <a name="merch_inventory" id="merch_inventory">MERCH_INVENTORY Table</a>

The table `MERCH_INVENTORY` stores information about the amount of non-coffee merchandise in stock:
<th id="h301">`ITEM_ID`</th><th id="h302">`ITEM_NAME`</th><th id="h303">`SUP_ID`</th><th id="h304">`QUAN`</th><th id="h305">`DATE`</th>
<td headers="h301">00001234</td><td headers="h302">Cup_Large</td><td headers="h303">00456</td><td headers="h304">28</td><td headers="h305">2006_04_01</td>
<td headers="h301">00001235</td><td headers="h302">Cup_Small</td><td headers="h303">00456</td><td headers="h304">36</td><td headers="h305">2006_04_01</td>
<td headers="h301">00001236</td><td headers="h302">Saucer</td><td headers="h303">00456</td><td headers="h304">64</td><td headers="h305">2006_04_01</td>
<td headers="h301">00001287</td><td headers="h302">Carafe</td><td headers="h303">00456</td><td headers="h304">12</td><td headers="h305">2006_04_01</td>
<td headers="h301">00006931</td><td headers="h302">Carafe</td><td headers="h303">00927</td><td headers="h304">3</td><td headers="h305">2006_04_01</td>
<td headers="h301">00006935</td><td headers="h302">PotHolder</td><td headers="h303">00927</td><td headers="h304">88</td><td headers="h305">2006_04_01</td>
<td headers="h301">00006977</td><td headers="h302">Napkin</td><td headers="h303">00927</td><td headers="h304">108</td><td headers="h305">2006_04_01</td>
<td headers="h301">00006979</td><td headers="h302">Towel</td><td headers="h303">00927</td><td headers="h304">24</td><td headers="h305">2006_04_01</td>
<td headers="h301">00004488</td><td headers="h302">CofMaker</td><td headers="h303">08732</td><td headers="h304">5</td><td headers="h305">2006_04_01</td>
<td headers="h301">00004490</td><td headers="h302">CofGrinder</td><td headers="h303">08732</td><td headers="h304">9</td><td headers="h305">2006_04_01</td>
<td headers="h301">00004495</td><td headers="h302">EspMaker</td><td headers="h303">08732</td><td headers="h304">4</td><td headers="h305">2006_04_01</td>
<td headers="h301">00006914</td><td headers="h302">Cookbook</td><td headers="h303">00927</td><td headers="h304">12</td><td headers="h305">2006_04_01</td>

The following describes each of the columns in the `MERCH_INVENTORY` table:

- `ITEM_ID`: Stores a number identifying an item.
- `ITEM_NAME`: Stores the name of an item.
- `SUP_ID`: Stores a number identifying a supplier.
- `QUAN`: Stores a number indicating the amount of that item available.
- `DATE`: Stores a timestamp value indicating the last time the row was updated.

## <a name="coffee_houses" id="coffee_houses">COFFEE_HOUSES Table</a>

The table `COFFEE_HOUSES` stores locations of coffee houses:
<th id="h401">`STORE_ID`</th><th id="h402">`CITY`</th><th id="h403">`COFFEE`</th><th id="h404">`MERCH`</th><th id="h405">`TOTAL`</th>
<td headers="h401">10023</td><td headers="h402">Mendocino</td><td headers="h403">3450</td><td headers="h404">2005</td><td headers="h405">5455</td>
<td headers="h401">33002</td><td headers="h402">Seattle</td><td headers="h403">4699</td><td headers="h404">3109</td><td headers="h405">7808</td>
<td headers="h401">10040</td><td headers="h402">SF</td><td headers="h403">5386</td><td headers="h404">2841</td><td headers="h405">8227</td>
<td headers="h401">32001</td><td headers="h402">Portland</td><td headers="h403">3147</td><td headers="h404">3579</td><td headers="h405">6726</td>
<td headers="h401">10042</td><td headers="h402">SF</td><td headers="h403">2863</td><td headers="h404">1874</td><td headers="h405">4710</td>
<td headers="h401">10024</td><td headers="h402">Sacramento</td><td headers="h403">1987</td><td headers="h404">2341</td><td headers="h405">4328</td>
<td headers="h401">10039</td><td headers="h402">Carmel</td><td headers="h403">2691</td><td headers="h404">1121</td><td headers="h405">3812</td>
<td headers="h401">10041</td><td headers="h402">LA</td><td headers="h403">1533</td><td headers="h404">1007</td><td headers="h405">2540</td>
<td headers="h401">33005</td><td headers="h402">Olympia</td><td headers="h403">2733</td><td headers="h404">1550</td><td headers="h405">4283</td>
<td headers="h401">33010</td><td headers="h402">Seattle</td><td headers="h403">3210</td><td headers="h404">2177</td><td headers="h405">5387</td>
<td headers="h401">10035</td><td headers="h402">SF</td><td headers="h403">1922</td><td headers="h404">1056</td><td headers="h405">2978</td>
<td headers="h401">10037</td><td headers="h402">LA</td><td headers="h403">2143</td><td headers="h404">1876</td><td headers="h405">4019</td>
<td headers="h401">10034</td><td headers="h402">San_Jose</td><td headers="h403">1234</td><td headers="h404">1032</td><td headers="h405">2266</td>
<td headers="h401">32004</td><td headers="h402">Eugene</td><td headers="h403">1356</td><td headers="h404">1112</td><td headers="h405">2468</td>

The following describes each of the columns in the `COFFEE_HOUSES` table:

- `STORE_ID`: Stores a number identifying a coffee house. It indicates, among other things, the state in which the coffee house is located. A value beginning with 10, for example, means that the state is California. `STORE_ID` values beginning with 32 indicate Oregon, and those beginning with 33 indicate the state of Washington.
- `CITY`: Stores the name of the city in which the coffee house is located.
- `COFFEE`: Stores a number indicating the amount of coffee sold.
- `MERCH`: Stores a number indicating the amount of merchandise sold.
- `TOTAL`: Stores a number indicating the total amount of coffee and merchandise sold.

## <a name="data_repository" id="data_repository">DATA_REPOSITORY Table</a>

The table DATA_REPOSITORY stores URLs that reference documents and other data of interest to The Coffee Break. The script `populate_tables.sql` does not add any data to this table. The following describes each of the columns in this table:

- `DOCUMENT_NAME`: Stores a string that identifies the URL.
- `URL`: Stores a URL.

## <a name="create" id="create">Creating Tables</a>

You can create tables with Apache Ant or JDBC API.

### Creating Tables with Apache Ant

To create the tables used with the tutorial sample code, run the following command in the directory `**&lt;JDBC tutorial directory&gt;**`:

```

ant setup

```

This command runs several Ant targets, including the following, `build-tables` (from the `build.xml` file):

```

&lt;target name="build-tables"
  description="Create database tables"&gt;
  &lt;sql
    driver="${DB.DRIVER}"
    url="${DB.URL}"
    userid="${DB.USER}"
    password="${DB.PASSWORD}"
    classpathref="CLASSPATH"
    delimiter="${DB.DELIMITER}"
    autocommit="false" onerror="abort"&gt;
    &lt;transaction src=
  "./sql/${DB.VENDOR}/create-tables.sql"/&gt;
  &lt;/sql&gt;
&lt;/target&gt;

```

The sample specifies values for the following `sql` Ant task parameters:
<th id="h501">Parameter</th><th id="h502">Description</th>
<td headers="h501">`driver`</td><td headers="h502">Fully qualified class name of your JDBC driver. This sample uses `org.apache.derby.jdbc.EmbeddedDriver` for Java DB and `com.mysql.cj.jdbc.Driver` for MySQL Connector/J.</td>
<td headers="h501">`url`</td><td headers="h502">Database connection URL that your DBMS JDBC driver uses to connect to a database.</td>
<td headers="h501">`userid`</td><td headers="h502">Name of a valid user in your DBMS.</td>
<td headers="h501">`password`</td><td headers="h502">Password of the user specified in `userid`</td>
<td headers="h501">`classpathref`</td><td headers="h502">Full path name of the JAR file that contains the class specified in `driver`</td>
<td headers="h501">`delimiter`</td><td headers="h502">String or character that separates SQL statements. This sample uses the semicolon (`;`).</td>
<td headers="h501">`autocommit`</td><td headers="h502">Boolean value; if set to `false`, all SQL statements are executed as one transaction.</td>
<td headers="h501">`onerror`</td><td headers="h502">Action to perform when a statement fails; possible values are `continue`, `stop`, and `abort`. The value `abort` specifies that if an error occurs, the transaction is aborted.</td>

The sample stores the values of these parameters in a separate file. The build file `build.xml` retrieves these values with the `import` task:

```

&lt;import file="${ANTPROPERTIES}"/&gt;

```

The `transaction` element specifies a file that contains SQL statements to execute. The file `create-tables.sql` contains SQL statements that create all the tables described on this page. For example, the following excerpt from this file creates the tables `SUPPLIERS` and `COFFEES`:

```

create table SUPPLIERS
    (SUP_ID integer NOT NULL,
    SUP_NAME varchar(40) NOT NULL,
    STREET varchar(40) NOT NULL,
    CITY varchar(20) NOT NULL,
    STATE char(2) NOT NULL,
    ZIP char(5),
    PRIMARY KEY (SUP_ID));

create table COFFEES
    (COF_NAME varchar(32) NOT NULL,
    SUP_ID int NOT NULL,
    PRICE numeric(10,2) NOT NULL,
    SALES integer NOT NULL,
    TOTAL integer NOT NULL,
    PRIMARY KEY (COF_NAME),
    FOREIGN KEY (SUP_ID)
        REFERENCES SUPPLIERS (SUP_ID));

```

**Note**: The file `build.xml` contains another target named `drop-tables` that deletes the tables used by the tutorial. The `setup` target runs `drop-tables` before running the `build-tables` target.

### Creating Tables with JDBC API

The following method, `[SuppliersTable.createTable](gettingstarted.html)`, creates the `SUPPLIERS` table:

```

  public void createTable() throws SQLException {
    String createString =
      "create table SUPPLIERS " + "(SUP_ID integer NOT NULL, " +
      "SUP_NAME varchar(40) NOT NULL, " + "STREET varchar(40) NOT NULL, " +
      "CITY varchar(20) NOT NULL, " + "STATE char(2) NOT NULL, " +
      "ZIP char(5), " + "PRIMARY KEY (SUP_ID))";
    
    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createString);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

The following method, `[CoffeesTable.createTable](gettingstarted.html)`, creates the `COFFEES` table:

```

  public void createTable() throws SQLException {
    String createString =
      "create table COFFEES " + "(COF_NAME varchar(32) NOT NULL, " +
      "SUP_ID int NOT NULL, " + "PRICE numeric(10,2) NOT NULL, " +
      "SALES integer NOT NULL, " + "TOTAL integer NOT NULL, " +
      "PRIMARY KEY (COF_NAME), " +
      "FOREIGN KEY (SUP_ID) REFERENCES SUPPLIERS (SUP_ID))";
    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate(createString);
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

In both methods, `con` is a `Connection` object and `dbName` is the name of the database in which you are creating the table.

To execute the SQL query, such as those specified by the `String` `createString`, use a `Statement` object. To create a `Statement` object, call the method `Connection.createStatement` from an existing `Connection` object. To execute a SQL query, call the method `Statement.executeUpdate`.

All `Statement` objects are closed when the connection that created them is closed. However, it is good coding practice to explicitly close `Statement` objects as soon as you are finished with them. This allows any external resources that the statement is using to be released immediately. Close a statement by calling the method `Statement.close`. Place this statement in a `finally` to ensure that it closes even if the normal program flow is interrupted because an exception (such as `SQLException`) is thrown.

**Note**: You must create the `SUPPLIERS` table before the `COFFEES` because `COFFEES` contains a foreign key, `SUP_ID` that references `SUPPLIERS`.

## <a name="populate" id="populate">Populating Tables</a>

Similarly, you can insert data into tables with Apache Ant or JDBC API.

### Populating Tables with Apache Ant

In addition to creating the tables used by this tutorial, the command `ant setup` also populates these tables. This command runs the Ant target `populate-tables`, which runs the SQL script `populate-tables.sql`.

The following is an excerpt from `populate-tables.sql` that populates the tables `SUPPLIERS` and `COFFEES`:

```

insert into SUPPLIERS values(
    49, 'Superior Coffee', '1 Party Place',
    'Mendocino', 'CA', '95460');
insert into SUPPLIERS values(
    101, 'Acme, Inc.', '99 Market Street',
    'Groundsville', 'CA', '95199');
insert into SUPPLIERS values(
    150, 'The High Ground',
    '100 Coffee Lane', 'Meadows', 'CA', '93966');
insert into COFFEES values(
    'Colombian', 00101, 7.99, 0, 0);
insert into COFFEES values(
    'French_Roast', 00049, 8.99, 0, 0);
insert into COFFEES values(
    'Espresso', 00150, 9.99, 0, 0);
insert into COFFEES values(
    'Colombian_Decaf', 00101, 8.99, 0, 0);
insert into COFFEES values(
    'French_Roast_Decaf', 00049, 9.99, 0, 0);

```

### Populating Tables with JDBC API

The following method, `SuppliersTable.populateTable`, inserts data into the table:

```

  public void populateTable() throws SQLException {
    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate("insert into SUPPLIERS " +
                         "values(49, 'Superior Coffee', '1 Party Place', " +
                         "'Mendocino', 'CA', '95460')");
      stmt.executeUpdate("insert into SUPPLIERS " +
                         "values(101, 'Acme, Inc.', '99 Market Street', " +
                         "'Groundsville', 'CA', '95199')");
      stmt.executeUpdate("insert into SUPPLIERS " +
                         "values(150, 'The High Ground', '100 Coffee Lane', " +
                         "'Meadows', 'CA', '93966')");
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```

The following method, `CoffeesTable.populateTable`, inserts data into the table:

```

  public void populateTable() throws SQLException {
    try (Statement stmt = con.createStatement()) {
      stmt.executeUpdate("insert into COFFEES " +
                         "values('Colombian', 00101, 7.99, 0, 0)");
      stmt.executeUpdate("insert into COFFEES " +
                         "values('French_Roast', 00049, 8.99, 0, 0)");
      stmt.executeUpdate("insert into COFFEES " +
                         "values('Espresso', 00150, 9.99, 0, 0)");
      stmt.executeUpdate("insert into COFFEES " +
                         "values('Colombian_Decaf', 00101, 8.99, 0, 0)");
      stmt.executeUpdate("insert into COFFEES " +
                         "values('French_Roast_Decaf', 00049, 9.99, 0, 0)");
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    }
  }

```
