
# Using Datalink Objects

A `DATALINK` value references a resource outside the underlying data source through a URL. A URL, uniform resource locator, is a pointer to a resource on the World Wide Web. A resource can be something as simple as a file or a directory, or it can be a reference to a more complicated object, such as a query to a database or to a search engine.

The following topics are covered:

- [Storing References to External Data](#storing_datalink)
- [Retrieving References to External Data](#retrieving_datalink)

## <a name="storing_datalink" id="storing_datalink">Storing References to External Data</a>

Use the method `PreparedStatement.setURL` to specify a `java.net.URL` object to a prepared statement. In cases where the type of URL being set is not supported by the Java platform, store the URL with the `setString` method.

For example, suppose the owner of The Coffee Break would like to store a list of important URLs in a database table. The following example, [`DatalinkSample.addURLRow`](gettingstarted.html) adds one row of data to the table `DATA_REPOSITORY`. The row consists of a string identifying the URL, `DOCUMENT_NAME` and the URL itself, `URL`:

```

  public void addURLRow(String description, String url) throws SQLException {
    String query = "INSERT INTO data_repository(document_name,url) VALUES (?,?)";
    try (PreparedStatement pstmt = this.con.prepareStatement(query)) {
      pstmt.setString(1, description);
      pstmt.setURL(2,new URL(url));
      pstmt.execute();    
    } catch (SQLException sqlex) {
      JDBCTutorialUtilities.printSQLException(sqlex);
    } catch (Exception ex) {
      System.out.println("Unexpected exception");
      ex.printStackTrace();
    }
  }

```

## <a name="retrieving_datalink" id="retrieving_datalink">Retrieving References to External Data</a>

Use the method `ResultSet.getURL` to retrieve a reference to external data as a `java.net.URL` object. In cases where the type of URL returned by the methods `getObject` or `getURL` is not supported by the Java platform, retrieve the URL as a `String` object by calling the method `getString`.

The following example, [`DatalinkSample.viewTable`](gettingstarted.html), displays the contents of all the URLs stored in the table `DATA_REPOSITORY`:

```

  public static void viewTable(Connection con, Proxy proxy)
    throws SQLException, IOException {
    String query = "SELECT document_name, url FROM data_repository";
    try (Statement stmt = con.createStatement()) {
      ResultSet rs = stmt.executeQuery(query);
      if ( rs.next() )  {
        String documentName = null;
        java.net.URL url = null;
        documentName = rs.getString(1);
        // Retrieve the value as a URL object.
        url = rs.getURL(2);    
        if (url != null) {
          // Retrieve the contents from the URL.
          URLConnection myURLConnection = url.openConnection(proxy);
          BufferedReader bReader =
            new BufferedReader(new InputStreamReader(myURLConnection.getInputStream()));
          System.out.println("Document name: " + documentName);
          String pageContent = null;
          while ((pageContent = bReader.readLine()) != null ) {
            // Print the URL contents
            System.out.println(pageContent);
          }
        } else { 
          System.out.println("URL is null");
        } 
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    } catch(IOException ioEx) {
      System.out.println("IOException caught: " + ioEx.toString());
    } catch (Exception ex) {
      System.out.println("Unexpected exception");
      ex.printStackTrace();
    }
  }

```

The sample [`DatalinkSample`](gettingstarted.html) stores the Oracle URL, [https://www.oracle.com](https://www.oracle.com) in the table `DATA_REPOSITORY`. Afterward, it displays the contents of all documents referred to by the URLs stored in `DATA_REPOSITORY`, which includes the Oracle home page, [https://www.oracle.com](https://www.oracle.com).

The sample retrieves the URL from the result set as a `java.net.URL` object with the following statement:

```

url = rs.getURL(2);

```

The sample accesses the data referred to by the `URL` object with the following statements:

```

          // Retrieve the contents from the URL.
          URLConnection myURLConnection = url.openConnection(proxy);
          BufferedReader bReader =
            new BufferedReader(new InputStreamReader(myURLConnection.getInputStream()));
          System.out.println("Document name: " + documentName);
          String pageContent = null;
          while ((pageContent = bReader.readLine()) != null ) {
            // Print the URL contents
            System.out.println(pageContent);
          }

```

The method `URLConnection.openConnection` can take no arguments, which means that the `URLConnection` represents a direct connection to the Internet. If you require a proxy server to connect to the Internet, the `openConnection` method accepts a `java.net.Proxy` object as an argument. The following statements demonstrate how to create an HTTP proxy with the server name `www-proxy.example.com` and port number `80`:

```

Proxy myProxy;
InetSocketAddress myProxyServer;
myProxyServer = new InetSocketAddress("www-proxy.example.com", 80);
myProxy = new Proxy(Proxy.Type.HTTP, myProxyServer);

```
