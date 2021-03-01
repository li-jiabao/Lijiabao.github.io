
# Using Structured Objects

**Note**: MySQL and Java DB currently do not support user-defined types. Consequently, no JDBC tutorial example is available to demonstrate the features described in this section.

The following topics are covered:

- [Overview of Structured Types](#overview_structured)
- [Using DISTINCT Type in Structured Types](#using_distinct_in_structured)
- [Using References to Structured Types](#references_structured)
- [Sample Code for Creating SQL REF Object](#code_ref)
- [Using User-Defined Types as Column Values](#udt_column_values)
- [Inserting User-Defined Types into Tables](#insert_udt)

## <a name="overview_structured" id="overview_structured">Overview of Structured Types</a>

SQL structured types and `DISTINCT` types are the two data types that a user can define in SQL. They are often referred to as UDTs (user-defined types), and you create them with a SQL `CREATE` `TYPE` statement.

Getting back to the example of The Coffee Break, suppose that the owner has been successful beyond all expectations and has been expanding with new branches. The owner has decided to add a `STORES` table to the database containing information about each establishment. `STORES` will have four columns:

- `STORE_NO` for each store's identification number
- `LOCATION` for its address
- `COF_TYPES` for the coffees it sells
- `MGR` for the name of the store manager

The owner makes the column `LOCATION` be a SQL structured type, the column `COF_TYPES` a SQL `ARRAY`, and the column `MGR` a `REF(MANAGER)`, with `MANAGER` being a SQL structured type.

The first thing the owner must define the new structured types for the address and the manager. A SQL structured type is similar to structured types in the Java programming language in that it has members, called **attributes**, that may be any data type. The owner writes the following SQL statement to create the new data type `ADDRESS`:

```

CREATE TYPE ADDRESS
(
    NUM INTEGER,
    STREET VARCHAR(40),
    CITY VARCHAR(40),
    STATE CHAR(2),
    ZIP CHAR(5)
);

```

In this statement, the new type `ADDRESS` has five attributes, which are analogous to fields in a Java class. The attribute `NUM` is an `INTEGER`, the attribute `STREET` is a `VARCHAR(40)`, the attribute `CITY` is a `VARCHAR(40)`, the attribute `STATE` is a `CHAR(2)`, and the attribute `ZIP` is a `CHAR(5)`.

The following excerpt, in which `con` is a valid `Connection` object, sends the definition of `ADDRESS` to the database:

```

String createAddress =
    "CREATE TYPE ADDRESS " +
    "(NUM INTEGER, STREET VARCHAR(40), " +
    "CITY VARCHAR(40), STATE CHAR(2), ZIP CHAR(5))";
Statement stmt = con.createStatement();
stmt.executeUpdate(createAddress);

```

Now the `ADDRESS` structured type is registered with the database as a data type, and the owner can use it as the data type for a table column or an attribute of a structured type.

## <a name="using_distinct_in_structured" id="using_distinct_in_structured">Using DISTINCT Type in Structured Type</a>

One of the attributes the owner of The Coffee Break plans to include in the new structured type `MANAGER` is the manager's telephone number. Because the owner will always list the telephone number as a 10-digit number (to be sure it includes the area code) and will never manipulate it as a number, the owner decides to define a new type called `PHONE_NO` that consists of 10 characters. The SQL definition of this data type, which can be thought of as a structured type with only one attribute, looks like this:

```

CREATE TYPE PHONE_NO AS CHAR(10);

```

Or, as noted earlier, for some drivers the definition might look like this:

```

CREATE DISTINCT TYPE PHONE_NO AS CHAR(10);

```

A `DISTINCT` type is always based on another data type, which must be a predefined type. In other words, a `DISTINCT` type cannot be based on a user-defined type (UDT). To retrieve or set a value that is a `DISTINCT` type, use the appropriate method for the underlying type (the type on which it is based). For example, to retrieve an instance of `PHONE_NO`, which is based on a `CHAR` type, you would use the method `getString` because that is the method for retrieving a `CHAR`.

Assuming that a value of type `PHONE_NO` is in the fourth column of the current row of the `ResultSet` object `**rs**`, the following line of code retrieves it:

```

String phoneNumber = rs.getString(4);

```

Similarly, the following line of code sets an input parameter that has type `PHONE_NO` for a prepared statement being sent to the database:

```

pstmt.setString(1, phoneNumber);

```

Adding on to the previous code fragment, the definition of `PHONE_NO` will be sent to the database with the following line of code:

```

stmt.executeUpdate(
    "CREATE TYPE PHONE_NO AS CHAR(10)");

```

After registering the type `PHONE_NO` with the database, the owner can use it as a column type in a table or as the data type for an attribute in a structured type. The definition of `MANAGER` in the following SQL statement uses `PHONE_NO` as the data type for the attribute `PHONE`:

```

CREATE TYPE MANAGER
(
    MGR_ID INTEGER,
    LAST_NAME VARCHAR(40),
    FIRST_NAME VARCHAR(40),
    PHONE PHONE_NO
);

```

Reusing `**stmt**`, defined previously, the following code fragment sends the definition of the structured type `MANAGER` to the database:

```

  String createManager =
    "CREATE TYPE MANAGER " +
    "(MGR_ID INTEGER, LAST_NAME " +
    "VARCHAR(40), " +
    "FIRST_NAME VARCHAR(40), " +
    "PHONE PHONE_NO)";
  stmt.executeUpdate(createManager);

```

## <a name="references_structured" id="references_structured">Using References to Structured Types</a>

The owner of The Coffee Break has created three new data types used as column types or attribute types in the database: The structured types `LOCATION` and `MANAGER`, and the `DISTINCT` type `PHONE_NO`. The entrepreneur has used `PHONE_NO` as the type for the attribute `PHONE` in the new type `MANAGER`, and `ADDRESS` as the data type for the column `LOCATION` in the table `STORES`. The `MANAGER` type could be used as the type for the column `MGR`, but instead the entrepreneur prefers to use the type `REF(MANAGER)` because the entrepreneur often has one person manage two or three stores. Using `REF(MANAGER)` as a column type avoids repeating all the data for `MANAGER` when one person manages more than one store.

With the structured type `MANAGER` already created, the owner can now create a table containing instances of `MANAGER` that can be referenced. A reference to an instance of `MANAGER` will have the type `REF(MANAGER)`. A SQL `REF` is nothing more than a logical pointer to a structured type, so an instance of `REF(MANAGER)` serves as a logical pointer to an instance of `MANAGER`.

Because a SQL `REF` value needs to be permanently associated with the instance of the structured type that it references, it is stored in a special table together with its associated instance. A programmer does not create `REF` types directly but rather creates the table that will store instances of a particular structured type that can be referenced. Every structured type that is to be referenced will have its own table. When you insert an instance of the structured type into the table, the database automatically creates a `REF` instance. For example, to contain instances of `MANAGER` that can be referenced, the owner created the following special table using SQL:

```

  CREATE TABLE MANAGERS OF MANAGER
  (OID REF(MANAGER)
  VALUES ARE SYSTEM GENERATED);

```

This statement creates a table with the special column `OID`, which stores values of type `REF(MANAGER)`. Each time an instance of `MANAGER` is inserted into the table, the database will generate an instance of `REF(MANAGER)` and store it in the column `OID`. Implicitly, an additional column stores each attribute of `MANAGER` that has been inserted into the table, as well. For example, the following code fragment shows how the entrepreneur created three instances of the `MANAGER` structured type to represent three managers:

```

  INSERT INTO MANAGERS (
    MGR_ID, LAST_NAME,
    FIRST_NAME, PHONE) VALUES
  (
    000001,
    'MONTOYA',
    'ALFREDO',
    '8317225600'
  );

  INSERT INTO MANAGERS (
    MGR_ID, LAST_NAME,
    FIRST_NAME, PHONE) VALUES
  (
    000002,
    'HASKINS',
    'MARGARET',
    '4084355600'
  );

  INSERT INTO MANAGERS (
    MGR_ID, LAST_NAME,
    FIRST_NAME, PHONE) VALUES
  (
    000003,
    'CHEN',
    'HELEN',
    '4153785600'
   );

```

The table `MANAGERS` will now have three rows, one row for each manager inserted so far. The column `OID` will contain three unique object identifiers of type `REF(MANAGER)`, one for each instance of `MANAGER.` These object identifiers were generated automatically by the database and will be permanently stored in the table `MANAGERS`. Implicitly, an additional column stores each attribute of `MANAGER`. For example, in the table `MANAGERS`, one row contains a `REF(MANAGER)` that references Alfredo Montoya, another row contains a `REF(MANAGER)` that references Margaret Haskins, and a third row contains a `REF(MANAGER)` that references Helen Chen.

To access a `REF(MANAGER)` instance, you select it from its table. For example, the owner retrieved the reference to Alfredo Montoya, whose ID number is 000001, with the following code fragment:

```

  String selectMgr =
    "SELECT OID FROM MANAGERS " +
    "WHERE MGR_ID = 000001";
  ResultSet rs = stmt.executeQuery(selectMgr);
  rs.next();
  Ref manager = rs.getRef("OID");

```

Now the variable `**manager**` can be used as a column value that references Alfredo Montoya.

## <a name="code_ref" id="code_ref">Sample Code for Creating SQL REF Object</a>

The following code example creates the table `MANAGERS`, a table of instances of the structured type `MANAGER` that can be referenced, and inserts three instances of `MANAGER` into the table. The column `OID` in this table will store instances of `REF(MANAGER)`. After this code is executed, the `MANAGERS` table will have a row for each of the three `MANAGER` objects inserted, and the value in the `OID` column will be the `REF(MANAGER)` type that identifies the instance of `MANAGER` stored in that row.

```

package com.oracle.tutorial.jdbc;

import java.sql.*;

public class CreateRef {

    public static void main(String args[]) {

        JDBCTutorialUtilities myJDBCTutorialUtilities;
        Connection myConnection = null;

        if (args[0] == null) {
            System.err.println("Properties file not specified " +
                               "at command line");
            return;
        } else {
            try {
                myJDBCTutorialUtilities = new JDBCTutorialUtilities(args[0]);
            } catch (Exception e) {
                System.err.println("Problem reading properties " +
                                   "file " + args[0]);
                e.printStackTrace();
                return;
            }
        }

        Connection con = null;
        Statement stmt = null;

        try {
            String createManagers =
                "CREATE TABLE " +
                "MANAGERS OF MANAGER " +
                "(OID REF(MANAGER) " +
                "VALUES ARE SYSTEM " +
                "GENERATED)";

            String insertManager1 =
                "INSERT INTO MANAGERS " +
                "(MGR_ID, LAST_NAME, " +
                "FIRST_NAME, PHONE) " +
                "VALUES " +
                "(000001, 'MONTOYA', " +
                "'ALFREDO', " +
                "'8317225600')";

            String insertManager2 =
                "INSERT INTO MANAGERS " +
                "(MGR_ID, LAST_NAME, " +
                "FIRST_NAME, PHONE) " +
                "VALUES " +
                "(000002, 'HASKINS', " +
                "'MARGARET', " +
                "'4084355600')";

            String insertManager3 =
                "INSERT INTO MANAGERS " +
                "(MGR_ID, LAST_NAME, " +
                "FIRST_NAME, PHONE) " +
                "VALUES " +
                "(000003, 'CHEN', 'HELEN', " +
                "'4153785600')";
  
            con = myJDBCTutorialUtilities.getConnection();
            con.setAutoCommit(false);

            stmt = con.createStatement();
            stmt.executeUpdate(createManagers);

            stmt.addBatch(insertManager1);
            stmt.addBatch(insertManager2);
            stmt.addBatch(insertManager3);
            int [] updateCounts = stmt.executeBatch();

            con.commit();

            System.out.println("Update count for:  ");
            for (int i = 0; i &lt; updateCounts.length; i++) {
                System.out.print("    command " + (i + 1) + " = ");
                System.out.println(updateCounts[i]);
            }
        } catch(BatchUpdateException b) {
            System.err.println("-----BatchUpdateException-----");
            System.err.println("Message:  " + b.getMessage());
            System.err.println("SQLState:  " + b.getSQLState());
            System.err.println("Vendor:  " + b.getErrorCode());
            System.err.print("Update counts for " + "successful commands:  ");
            int [] rowsUpdated = b.getUpdateCounts();
            for (int i = 0; i &lt; rowsUpdated.length; i++) {
                System.err.print(rowsUpdated[i] + "   ");
            }
            System.err.println("");
        } catch(SQLException ex) {
            System.err.println("------SQLException------");
            System.err.println("Error message:  " + ex.getMessage());
            System.err.println("SQLState:  " + ex.getSQLState());
            System.err.println("Vendor:  " + ex.getErrorCode());
        } finally {
            if (stmt != null) { stmt.close(); }
              JDBCTutorialUtilities.closeConnection(con);
        }
    }
}

```

## <a name="udt_column_values" id="udt_column_values">Using User-Defined Types as Column Values</a>

Our entrepreneur now has the UDTs required to create the table `STORES`. The structured type `ADDRESS` is the type for the column `LOCATION`, and the type `REF(MANAGER)` is the type for the column `MGR`.

The UDT `COF_TYPES` is based on the SQL data type `ARRAY` and is the type for the column `COF_TYPES`. The following line of code creates the type `COF_ARRAY` as an `ARRAY` value with 10 elements. The base type of `COF_ARRAY` is `VARCHAR(40)`.

```

  CREATE TYPE COF_ARRAY AS ARRAY(10) OF VARCHAR(40);

```

With the new data types defined, the following SQL statement creates the table `STORES`:

```

  CREATE TABLE STORES
  (
    STORE_NO INTEGER,
    LOCATION ADDRESS,
    COF_TYPES COF_ARRAY,
    MGR REF(MANAGER)
  );

```

## <a name="insert_udt" id="insert_udt">Inserting User-Defined Types into Tables</a>

The following code fragment inserts one row into the `STORES` table, supplying values for the columns `STORE_NO`, `LOCATION`, `COF_TYPES`, and `MGR`, in that order:

```

  INSERT INTO STORES VALUES
  (
    100001,
    ADDRESS(888, 'Main_Street',
      'Rancho_Alegre',
      'CA', '94049'),
    COF_ARRAY('Colombian', 'French_Roast',
      'Espresso', 'Colombian_Decaf',
      'French_Roast_Decaf'),
    SELECT OID FROM MANAGERS
      WHERE MGR_ID = 000001
  );

```

The following goes through each column and the value inserted into it.

```

  STORE_NO: 100001

```

This column is type `INTEGER`, and the number `100001` is an `INTEGER` type, similar to entries made before in the tables `COFFEES` and `SUPPLIERS`.

```

  LOCATION: ADDRESS(888, 'Main_Street',
    'Rancho_Alegre', 'CA', '94049')

```

The type for this column is the structured type `ADDRESS`, and this value is the constructor for an instance of `ADDRESS`. When we sent the definition of `ADDRESS` was sent to the database, one of the things it did was to create a constructor for the new type. The comma-separated values in parentheses are the initialization values for the attributes of the `ADDRESS` type, and they must appear in the same order in which the attributes were listed in the definition of the `ADDRESS` type. `888` is the value for the attribute `NUM`, which is an `INTEGER` value. `"Main_Street"` is the value for `STREET`, and `"Rancho_Alegre"` is the value for `CITY`, with both attributes being of type `VARCHAR(40)`. The value for the attribute `STATE` is `"CA"`, which is of type `CHAR(2)`, and the value for the attribute `ZIP` is `"94049"`, which is of type `CHAR(5)`.

```

  COF_TYPES: COF_ARRAY(
    'Colombian',
    'French_Roast',
    'Espresso',
    'Colombian_Decaf',
    'French_Roast_Decaf'),

```

The column `COF_TYPES` is of type `COF_ARRAY` with a base type of `VARCHAR(40)`, and the comma-separated values between parentheses are the `String` objects that are the array elements. The owner defined the type `COF_ARRAY` as having a maximum of 10 elements. This array has 5 elements because the entrepreneur supplied only 5 `String` objects for it.

```

  MGR: SELECT OID FROM MANAGERS
    WHERE MGR_ID = 000001

```

The column `MGR` is type `REF(MANAGER)`, which means that a value in this column must be a reference to the structured type `MANAGER`. All of the instances of `MANAGER` are stored in the table `MANAGERS`. All of the instances of `REF(MANAGER)` are also stored in this table, in the column `OID`. The manager for the store described in this table row is Alfredo Montoya, and his information is stored in the instance of `MANAGER` that has `100001` for the attribute `MGR_ID`. To get the `REF(MANAGER)` instance associated with the `MANAGER` object for Alfredo Montoya, select the column `OID` that is in the row where `MGR_ID` is `100001` in the table `MANAGERS`. The value that will be stored in the `MGR` column of the `STORES` table (the `REF(MANAGER)` value) is the value the DBMS generated to uniquely identify this instance of the `MANAGER` structured type.

Send the preceding SQL statement to the database with the following code fragment:

```

  String insertMgr =
    "INSERT INTO STORES VALUES " +
    "(100001, " +
    "ADDRESS(888, 'Main_Street', " +
      "'Rancho_Alegre', 'CA', " +
      "'94049'), " +
    "COF_ARRAY('Colombian', " +
      "'French_Roast', 'Espresso', " +
      "'Colombian_Decaf', " +
      "'French_Roast_Decaf'}, " +
    "SELECT OID FROM MANAGERS " +
    "WHERE MGR_ID = 000001)";

  stmt.executeUpdate(insertMgr);

```

However, because you are going to send several `INSERT INTO` statements, it will be more efficient to send them all together as a batch update, as in the following code example:

```

package com.oracle.tutorial.jdbc;

import java.sql.*;

public class InsertStores {
    public static void main(String args[]) {

        JDBCTutorialUtilities myJDBCTutorialUtilities;
        Connection myConnection = null;

        if (args[0] == null) {
            System.err.println(
                "Properties file " +
                "not specified " +
                "at command line");
            return;
        } else {
            try {
                myJDBCTutorialUtilities = new
                    JDBCTutorialUtilities(args[0]);
            } catch (Exception e) {
                System.err.println(
                    "Problem reading " +
                    "properties file " +
                    args[0]);
                e.printStackTrace();
                return;
            }
        }

        Connection con = null;
        Statement stmt = null;

        try {
            con = myJDBCTutorialUtilities.getConnection();
            con.setAutoCommit(false);

            stmt = con.createStatement();

            String insertStore1 =
                "INSERT INTO STORES VALUES (" +
                "100001, " +
                "ADDRESS(888, 'Main_Street', " +
                    "'Rancho_Alegre', 'CA', " +
                    "'94049'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " +
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000001))";

            stmt.addBatch(insertStore1);

            String insertStore2 =
                "INSERT INTO STORES VALUES (" +
                "100002, " +
                "ADDRESS(1560, 'Alder', " +
                    "'Ochos_Pinos', " +
                    "'CA', '94049'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " +
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf', " +
                    "'Kona', 'Kona_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000001))";

            stmt.addBatch(insertStore2);

            String insertStore3 =
                "INSERT INTO STORES VALUES (" +
                "100003, " +
                "ADDRESS(4344, " +
                    "'First_Street', " +
                    "'Verona', " +
                    "'CA', '94545'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " +
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf', " +
                    "'Kona', 'Kona_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000002))";

            stmt.addBatch(insertStore3);

            String insertStore4 =
                "INSERT INTO STORES VALUES (" +
                "100004, " +
                "ADDRESS(321, 'Sandy_Way', " +
                    "'La_Playa', " +
                    "'CA', '94544'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " +
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf', " +
                    "'Kona', 'Kona_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000002))";

            stmt.addBatch(insertStore4);

            String insertStore5 =
                "INSERT INTO STORES VALUES (" +
                "100005, " +
                "ADDRESS(1000, 'Clover_Road', " +
                    "'Happyville', " +
                    "'CA', '90566'), " +
                "COF_ARRAY('Colombian', " +
                    "'French_Roast', " +
                    "'Espresso', " + 
                    "'Colombian_Decaf', " +
                    "'French_Roast_Decaf'), " +
                "(SELECT OID FROM MANAGERS " +
                "WHERE MGR_ID = 000003))";

            stmt.addBatch(insertStore5);

            int [] updateCounts = stmt.executeBatch();

            ResultSet rs = stmt.executeQuery(
                "SELECT * FROM STORES");
            System.out.println("Table STORES after insertion:");
            System.out.println("STORE_NO   " + "LOCATION   " +
                "COF_TYPE   " + "MGR");

            while (rs.next()) {
                int storeNo = rs.getInt("STORE_NO");
                Struct location = (Struct)rs.getObject("LOCATION");
                Object[] locAttrs = location.getAttributes();
                Array coffeeTypes = rs.getArray("COF_TYPE");
                String[] cofTypes = (String[])coffeeTypes.getArray();

                Ref managerRef = rs.getRef("MGR");
                PreparedStatement pstmt = con.prepareStatement(
                    "SELECT MANAGER " +
                    "FROM MANAGERS " +
                    "WHERE OID = ?");
  
                pstmt.setRef(1, managerRef);
                ResultSet rs2 = pstmt.executeQuery();
                rs2.next();
                Struct manager = (Struct)rs2.getObject("MANAGER");
                Object[] manAttrs = manager.getAttributes();
      
                System.out.print(storeNo + "   ");
                System.out.print(
                    locAttrs[0] + " " +
                    locAttrs[1] + " " +
                    locAttrs[2] + ", " +
                    locAttrs[3] + " " +
                    locAttrs[4] + " ");

                for (int i = 0; i &lt; cofTypes.length; i++)
                    System.out.print( cofTypes[i] + " ");
          
                System.out.println(
                    manAttrs[1] + ", " +
                    manAttrs[2]);
        
                rs2.close();
                pstmt.close();
            }

            rs.close();

        } catch(BatchUpdateException b) {
            System.err.println("-----BatchUpdateException-----");
            System.err.println("SQLState:  " + b.getSQLState());
            System.err.println("Message:  " + b.getMessage());
            System.err.println("Vendor:  " + b.getErrorCode());
            System.err.print("Update counts:  ");
            int [] updateCounts = b.getUpdateCounts();

            for (int i = 0; i &lt; updateCounts.length; i++) {
                System.err.print(updateCounts[i] + "   ");
            }
            System.err.println("");

        } catch(SQLException ex) {
            System.err.println("SQLException: " + ex.getMessage());
            System.err.println("SQLState:  " + ex.getSQLState());
            System.err.println("Message:  " + ex.getMessage());
            System.err.println("Vendor:  " + ex.getErrorCode());
        } finally {
            if (stmt != null) { stmt.close(); }
                JDBCTutorialUtilities.closeConnection(con);
            }
        }
    }
}

```
