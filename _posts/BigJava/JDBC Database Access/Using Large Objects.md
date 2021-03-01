
# Using Large Objects

An important feature of `Blob`, `Clob`, and `NClob` Java objects is that you can manipulate them without having to bring all of their data from the database server to your client computer. Some implementations represent an instance of these types with a locator (logical pointer) to the object in the database that the instance represents. Because a `BLOB`, `CLOB`, or `NCLOB` SQL object may be very large, the use of locators can make performance significantly faster. However, other implementations fully materialize large objects on the client computer.

If you want to bring the data of a `BLOB`, `CLOB`, or `NCLOB` SQL value to the client computer, use methods in the `Blob`, `Clob`, and `NClob` Java interfaces that are provided for this purpose. These large object type objects materialize the data of the objects they represent as a stream.

The following topics are covered:

- [Adding Large Object Type Objects to Databases](#add_lob)
- [Retrieving CLOB Values](#retrieve_clob)
- [Adding and Retrieving BLOB Objects](#add_retrieve_blob)
- [Releasing Resources Held by Large Objects](#release_large_objects)

## <a name="add_lob" id="add_lob">Adding Large Object Type Object to Database</a>

The following excerpt from `[ClobSample.addRowToCoffeeDescriptions](gettingstarted.html)` adds a `CLOB` SQL value to the table `COFFEE_DESCRIPTIONS`. The `Clob` Java object `myClob` contains the contents of the file specified by `fileName`.

```

  public void addRowToCoffeeDescriptions(String coffeeName,
                                         String fileName) throws SQLException {
    String sql = "INSERT INTO COFFEE_DESCRIPTIONS VALUES(?,?)";
    Clob myClob = this.con.createClob();
    try (PreparedStatement pstmt = this.con.prepareStatement(sql);
      Writer clobWriter = myClob.setCharacterStream(1);){
      String str = this.readFile(fileName, clobWriter);
      System.out.println("Wrote the following: " + clobWriter.toString());
      if (this.settings.dbms.equals("mysql")) {
        System.out.println("MySQL, setting String in Clob object with setString method");
        myClob.setString(1, str);
      }
      System.out.println("Length of Clob: " + myClob.length());
      pstmt.setString(1, coffeeName);
      pstmt.setClob(2, myClob);
      pstmt.executeUpdate();
    } catch (SQLException sqlex) {
      JDBCTutorialUtilities.printSQLException(sqlex);
    } catch (Exception ex) {
      System.out.println("Unexpected exception: " + ex.toString());
    }
  }

```

The following line creates a `Clob` Java object:

```

Clob myClob = this.con.createClob();

```

The following line retrieves a stream (in this case a `Writer` object named `clobWriter`) that is used to write a stream of characters to the `Clob` Java object `myClob`. The method `ClobSample.readFile` writes this stream of characters; the stream is from the file specified by the `String` `fileName`. The method argument `1` indicates that the `Writer` object will start writing the stream of characters at the beginning of the `Clob` value:

```

Writer clobWriter = myClob.setCharacterStream(1);

```

The `ClobSample.readFile` method reads the file line-by-line specified by the file `fileName` and writes it to the `Writer` object specified by `writerArg`:

```

  private String readFile(String fileName, Writer writerArg) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
      String nextLine = "";
      StringBuffer sb = new StringBuffer();
      while ((nextLine = br.readLine()) != null) {
        System.out.println("Writing: " + nextLine);
        writerArg.write(nextLine);
        sb.append(nextLine);
      }
      // Convert the content into to a string
      String clobData = sb.toString();
	  // Return the data.
      return clobData;
	}
  }

```

The following excerpt creates a `PreparedStatement` object `pstmt` that inserts the `Clob` Java object `myClob` into `COFFEE_DESCRIPTIONS`:

```

    String sql = "INSERT INTO COFFEE_DESCRIPTIONS VALUES(?,?)";
    Clob myClob = this.con.createClob();
    try (PreparedStatement pstmt = this.con.prepareStatement(sql);
      // ...
      ) {
      // ...
      pstmt.setString(1, coffeeName);
      pstmt.setClob(2, myClob);
      pstmt.executeUpdate();
      // ...

```

## <a name="retrieve_clob" id="retrieve_clob">Retrieving CLOB Values</a>

The method `[ClobSample.retrieveExcerpt](gettingstarted.html)` retrieves the `CLOB` SQL value stored in the `COF_DESC` column of `COFFEE_DESCRIPTIONS` from the row whose column value `COF_NAME` is equal to the `String` value specified by the `coffeeName` parameter:

```

  public String retrieveExcerpt(String coffeeName,
                                int numChar) throws SQLException {

    String description = null;
    Clob myClob = null;
    String sql = "select COF_DESC from COFFEE_DESCRIPTIONS where COF_NAME = ?";

    try (PreparedStatement pstmt = this.con.prepareStatement(sql)) {
      pstmt.setString(1, coffeeName);
      ResultSet rs = pstmt.executeQuery();
      if (rs.next()) {
        myClob = rs.getClob(1);
        System.out.println("Length of retrieved Clob: " + myClob.length());
      }
      description = myClob.getSubString(1, numChar);
    } catch (SQLException sqlex) {
      JDBCTutorialUtilities.printSQLException(sqlex);
    } catch (Exception ex) {
      System.out.println("Unexpected exception: " + ex.toString());
    }
    return description;
  }

```

The following line retrieves the `Clob` Java value from the `ResultSet` object `rs`:

```

myClob = rs.getClob(1);

```

The following line retrieves a substring from the `myClob` object. The substring begins at the first character of the value of `myClob` and has up to the number of consecutive characters specified in `numChar`, where `numChar` is an integer.

```

description = myClob.getSubString(1, numChar);

```

## <a name="add_retrieve_blob" id="add_retrieve_blob">Adding and Retrieving BLOB Objects</a>

Adding and retrieving `BLOB` SQL objects is similar to adding and retrieving `CLOB` SQL objects. Use the `Blob.setBinaryStream` method to retrieve an `OutputStream` object to write the `BLOB` SQL value that the `Blob` Java object (which called the method) represents.

## <a name="release_large_objects" id="release_large_objects">Releasing Resources Held by Large Objects</a>

`Blob`, `Clob`, and `NClob` Java objects remain valid for at least the duration of the transaction in which they are created. This could potentially result in an application running out of resources during a long running transaction. Applications may release `Blob`, `Clob`, and `NClob` resources by invoking their `free` method.

In the following excerpt, the method `Clob.free` is called to release the resources held for a previously created `Clob` object:

```

Clob aClob = con.createClob();
int numWritten = aClob.setString(1, val);
aClob.free();

```
