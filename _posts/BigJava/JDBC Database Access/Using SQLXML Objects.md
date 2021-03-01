
# Using SQLXML Objects

The `Connection` interface provides support for the creation of `SQLXML` objects using the method `createSQLXML`. The object that is created does not contain any data. Data may be added to the object by calling the `setString`, `setBinaryStream`, `setCharacterStream` or `setResult` method on the `SQLXML` interface.

The following topics are covered:

- [Creating SQLXML Objects](#creating_sqlxml)
- [Retrieving SQLXML Values in ResultSet](#retrieving_sqlxml)
- [Accessing SQLXML Object Data](#accessing_sqlxml)
- [Storing SQLXML Objects](#storing_sqlxml)
- [Initializing SQLXML Objects](#initializing_sqlxml)
- [Releasing SQLXML Resources](#releasing_sqlxml)
- [Sample Code](#sample_code)

## <a name="creating_sqlxml" id="creating_sqlxml">Creating SQLXML Objects</a>

In the following excerpt, the method `Connection.createSQLXML` is used to create an empty `SQLXML` object. The `SQLXML.setString` method is used to write data to the `SQLXML` object that was created.

```

Connection con = DriverManager.getConnection(url, props);
SQLXML xmlVal = con.createSQLXML();
xmlVal.setString(val);

```

## <a name="retrieving_sqlxml" id="retrieving_sqlxml">Retrieving SQLXML Values in ResultSet</a>

The `SQLXML` data type is treated similarly to the more primitive built-in types. A `SQLXML` value can be retrieved by calling the `getSQLXML` method in the `ResultSet` or `CallableStatement` interface.

For example, the following excerpt retrieves a `SQLXML` value from the first column of the `ResultSet` *rs*:

```

SQLXML xmlVar = rs.getSQLXML(1);

```

`SQLXML` objects remain valid for at least the duration of the transaction in which they are created, unless their `free` method is invoked.

## <a name="accessing_sqlxml" id="accessing_sqlxml">Accessing SQLXML Object Data</a>

The `SQLXML` interface provides the `getString`, `getBinaryStream`, `getCharacterStream`, and `getSource` methods to access its internal content. The following excerpt retrieves the contents of an `SQLXML` object using the `getString` method:

```

SQLXML xmlVal= rs.getSQLXML(1);
String val = xmlVal.getString();

```

The `getBinaryStream` or `getCharacterStream` methods can be used to obtain an `InputStream` or a `Reader` object that can be passed directly to an XML parser. The following excerpt obtains an `InputStream` object from an `SQLXML` Object and then processes the stream using a DOM (Document Object Model) parser:

```

SQLXML sqlxml = rs.getSQLXML(column);
InputStream binaryStream = sqlxml.getBinaryStream();
DocumentBuilder parser = 
    DocumentBuilderFactory.newInstance().newDocumentBuilder();
Document result = parser.parse(binaryStream);

```

The `getSource` method returns a `javax.xml.transform.Source` object. Sources are used as input to XML parsers and XSLT transformers.

The following excerpt retrieves and parses the data from a `SQLXML` object using the `SAXSource` object returned by invoking the `getSource` method:

```

SQLXML xmlVal= rs.getSQLXML(1);
SAXSource saxSource = sqlxml.getSource(SAXSource.class);
XMLReader xmlReader = saxSource.getXMLReader();
xmlReader.setContentHandler(myHandler);
xmlReader.parse(saxSource.getInputSource());

```

## <a name="storing_sqlxml" id="storing_sqlxml">Storing SQLXML Objects</a>

A `SQLXML` object can be passed as an input parameter to a `PreparedStatement` object just like other data types. The method `setSQLXML` sets the designated `PreparedStatement` parameter with a `SQLXML` object.

In the following excerpt, `authorData` is an instance of the `java.sql.SQLXML` interface whose data was initialized previously.

```

PreparedStatement pstmt = conn.prepareStatement("INSERT INTO bio " +
                              "(xmlData, authId) VALUES (?, ?)");
pstmt.setSQLXML(1, authorData);
pstmt.setInt(2, authorId);

```

The `updateSQLXML` method can be used to update a column value in an updatable result set.

If the `java.xml.transform.Result`, `Writer`, or `OutputStream` object for the `SQLXML` object has not been closed prior to calling `setSQLXML` or `updateSQLXML`, a `SQLException` will be thrown.

## <a name="initializing_sqlxml" id="initializing_sqlxml">Initializing SQLXML Objects</a>

The `SQLXML` interface provides the methods `setString`, `setBinaryStream`, `setCharacterStream`, or `setResult` to initialize the content for a `SQLXML` object that has been created by calling the `Connection.createSQLXML` method.

The following excerpt uses the method `setResult` to return a `SAXResult` object to populate a newly created `SQLXML` object:

```

SQLXML sqlxml = con.createSQLXML();
SAXResult saxResult = sqlxml.setResult(SAXResult.class);
ContentHandler contentHandler = saxResult.getXMLReader().getContentHandler();
contentHandler.startDocument();
    
// set the XML elements and
// attributes into the result
contentHandler.endDocument();

```

The following excerpt uses the `setCharacterStream` method to obtain a `java.io.Writer` object in order to initialize a `SQLXML` object:

```

SQLXML sqlxml = con.createSQLXML();
Writer out= sqlxml.setCharacterStream();
BufferedReader in = new BufferedReader(new FileReader("xml/foo.xml"));
String line = null;
while((line = in.readLine() != null) {
    out.write(line);
}

```

Similarly, the `SQLXML` `setString` method can be used to initialize a `SQLXML` object.

If an attempt is made to call the `setString`, `setBinaryStream`, `setCharacterStream`, and `setResult` methods on a `SQLXML` object that has previously been initialized, a `SQLException` will be thrown. If more than one call to the methods `setBinaryStream`, `setCharacterStream`, and `setResult` occurs for the same `SQLXML` object, a `SQLException` is thrown and the previously returned `javax.xml.transform.Result`, `Writer`, or `OutputStream` object is not affected.

## <a name="releasing_sqlxml" id="releasing_sqlxml">Releasing SQLXML Resources</a>

`SQLXML` objects remain valid for at least the duration of the transaction in which they are created. This could potentially result in an application running out of resources during a long running transaction. Applications may release `SQLXML` resources by invoking their `free` method.

In the following excerpt, the `method SQLXML.free` is called to release the resources held for a previously created `SQLXML` object.

```

SQLXML xmlVar = con.createSQLXML();
xmlVar.setString(val);
xmlVar.free();

```

## <a name="sample_code" id="sample_code">Sample Code</a>

MySQL and Java DB and their respective JDBC drivers do not fully support the `SQLXML` JDBC data type as described on in this section. However, the sample `[RSSFeedsTable](gettingstarted.html)` demonstrates how to handle XML data with MySQL and Java DB.

The owner of The Coffee Break follows several RSS feeds from various web sites that cover restaurant and beverage industry news. An RSS (Really Simple Syndication or Rich Site Summary) feed is an XML document that contains a series of articles and associated metadata, such as the date of publication and author for each article. The owner would like to store these RSS feeds into a database table, including the RSS feed from The Coffee Break's blog.

The file `[rss-the-coffee-break-blog.xml](gettingstarted.html)` is an example RSS feed from The Coffee Break's blog.

### <a name="working-with-xml-data-in-mysql" id="working-with-xml-data-in-mysql">Working with XML Data in MySQL</a>

The sample `RSSFeedsTable` stores RSS feeds in the table `RSS_FEEDS`, which is created with the following command:

```

create table RSS_FEEDS
    (RSS_NAME varchar(32) NOT NULL,
    RSS_FEED_XML longtext NOT NULL,
    PRIMARY KEY (RSS_NAME));

```

MySQL does not support the XML data type. Instead, this sample stores XML data in a column of type `LONGTEXT`, which is a `CLOB` SQL data type. MySQL has four `CLOB` data types; the `LONGTEXT` data type holds the greatest amount of characters among the four.

The method `[RSSFeedsTable.addRSSFeed](gettingstarted.html)` adds an RSS feed to the `RSS_FEEDS` table. The first statements of this method converts the RSS feed (which is represented by an XML file in this sample) into an object of type `org.w3c.dom.Document`, which represents a DOM (Document Object Model) document. This class, along with classes and interfaces contained in the package `javax.xml`, contain methods that enable you to manipulate XML data content. For example, the following statement uses an XPath expression to retrieve the title of the RSS feed from the `Document` object:

```

Node titleElement =
    (Node)xPath.evaluate("/rss/channel/title[1]",
        doc, XPathConstants.NODE);

```

The XPath expression `/rss/channel/title[1]` retrieves the contents of the first `&lt;title&gt;` element. For the file `rss-the-coffee-break-blog.xml`, this is the string `The Coffee Break Blog`.

The following statements add the RSS feed to the table `RSS_FEEDS`:

```

// For databases that support the SQLXML
// data type, this creates a
// SQLXML object from
// org.w3c.dom.Document.

System.out.println("Adding XML file " + fileName);
String insertRowQuery =
    "insert into RSS_FEEDS " +
    "(RSS_NAME, RSS_FEED_XML) values " +
    "(?, ?)";
insertRow = con.prepareStatement(insertRowQuery);
insertRow.setString(1, titleString);

System.out.println("Creating SQLXML object with MySQL");
rssData = con.createSQLXML();
System.out.println("Creating DOMResult object");
DOMResult dom = (DOMResult)rssData.setResult(DOMResult.class);
dom.setNode(doc);

insertRow.setSQLXML(2, rssData);
System.out.println("Running executeUpdate()");
insertRow.executeUpdate();

```

The `[RSSFeedsTable.viewTable](gettingstarted.html)` method retrieves the contents of `RSS_FEEDS`. For each row, the method creates an object of type `org.w3c.dom.Document` named `doc` in which to store the XML content in the column `RSS_FEED_XML`. The method retrieves the XML content and stores it in an object of type `SQLXML` named `rssFeedXML`. The contents of `rssFeedXML` are parsed and stored in the `doc` object.

### <a name="working-with-xml-data-in-java-db" id="working-with-xml-data-in-java-db">Working with XML Data in Java DB</a>

**Note**: See the section "XML data types and operators" in [**Java DB Developer's Guide**](https://docs.oracle.com/javadb/index_jdk8.html) for more information about working with XML data in Java DB.

The sample `RSSFeedsTable` stores RSS feeds in the table `RSS_FEEDS`, which is created with the following command:

```

create table RSS_FEEDS
    (RSS_NAME varchar(32) NOT NULL,
    RSS_FEED_XML xml NOT NULL,
    PRIMARY KEY (RSS_NAME));

```

Java DB supports the XML data type, but it does not support the `SQLXML` JDBC data type. Consequently, you must convert any XML data to a character format, and then use the Java DB operator `XMLPARSE` to convert it to the XML data type.

The `[RSSFeedsTable.addRSSFeed](gettingstarted.html)` method adds an RSS feed to the `RSS_FEEDS` table. The first statements of this method convert the RSS feed (which is represented by an XML file in this sample) into an object of type `org.w3c.dom.Document`. This is described in the section [Working with XML Data in MySQL](#working-with-xml-data-in-mysql).

The `[RSSFeedsTable.addRSSFeed](gettingstarted.html)` method converts the RSS feed to a `String` object with the method `[JDBCTutorialUtilities.convertDocumentToString](gettingstarted.html)`.

Java DB has an operator named `XMLPARSE` that parses a character string representation into a Java DB XML value, which is demonstrated by the following excerpt:

```

String insertRowQuery =
    "insert into RSS_FEEDS " +
    "(RSS_NAME, RSS_FEED_XML) values " +
    "(?, <strong>xmlparse(document cast " +
    "(? as clob) preserve whitespace)</strong>)";

```

The `XMLPARSE` operator requires that you convert the character representation of the XML document into a string data type that Java DB recognizes. In this example, it converts it into a `CLOB` data type. 
See [Getting Started](gettingstarted.html) and the Java DB documentation for more information about Apache Xalan and Java DB requirements.

The method `[RSSFeedsTable.viewTable](gettingstarted.html)` retrieves the contents of `RSS_FEEDS`. Because Java DB does not support the JDBC data type `SQLXML` you must retrieve the XML content as a string. Java DB has an operator named `XMLSERIALIZE` that converts an XML type to a character type:

```

String query =
    "select RSS_NAME, " +
    "xmlserialize " +
    "(RSS_FEED_XML as clob) " +
    "from RSS_FEEDS";

```

As with the `XMLPARSE` operator, the `XMLSERIALIZE` operator requires that Apache Xalan be listed in your Java class path.
