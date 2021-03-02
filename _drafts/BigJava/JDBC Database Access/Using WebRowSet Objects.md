
# Using WebRowSet Objects

A `WebRowSet` object is very special because in addition to offering all of the capabilities of a `CachedRowSet` object, it can write itself as an XML document and can also read that XML document to convert itself back to a `WebRowSet` object. Because XML is the language through which disparate enterprises can communicate with each other, it has become the standard for Web Services communication. As a consequence, a `WebRowSet` object fills a real need by enabling Web Services to send and receive data from a database in the form of an XML document.

The following topics are covered:

- [Creating and Populating WebRowSet Objects](#creating-and-populating-webrowset-object)
- [Writing and Reading WebRowSet Objects to XML](#writing-and-reading-webrowset-object-to-xml)
- [What Is in XML Documents](#what-is-in-xml-document)
- [Making Changes to WebRowSet Objects](#making-changes-to-webrowset-object)

The Coffee Break company has expanded to selling coffee online. Users order coffee by the pound from the Coffee Break Web site. The price list is regularly updated by getting the latest information from the company's database. This section demonstrates how to send the price data as an XML document with a `WebRowSet` object and a single method call.

## <a name="creating-and-populating-webrowset-object" id="creating-and-populating-webrowset-object">Creating and Populating WebRowSet Objects</a>

You create a new `WebRowSet` object by using an instance of `RowSetFactory`, which is created from the `RowSetProvider` class, to create a `WebRowSet` object. The following example is from [`WebRowSetSample`](gettingstarted.html):

```

    RowSetFactory factory = RowSetProvider.newFactory();  
    try (WebRowSet priceList = factory.createWebRowSet();
         // ...
    ) {	  
      // ...

```

Although the `priceList` object has no data yet, it has the default properties of a `BaseRowSet` object. Its `SyncProvider` object is at first set to the `RIOptimisticProvider` implementation, which is the default for all disconnected `RowSet` objects. However, the `WebRowSet` implementation resets the `SyncProvider` object to be the `RIXMLProvider` implementation.

You can use an instance of `RowSetFactory`, which is created from the `RowSetProvider` class, to create a `WebRowSet` object. See [Using the RowSetFactory Interface](jdbcrowset.html#rowsetfactory) in [Using JdbcRowSet Objects](jdbcrowset.html) for more information.

The Coffee Break headquarters regularly sends price list updates to its web site. This information on `WebRowSet` objects will show one way you can send the latest price list in an XML document.

The price list consists of the data in the columns `COF_NAME` and `PRICE` from the table `COFFEES`. The following code fragment sets the properties needed and populates the `priceList` object with the price list data:

```

      int[] keyCols = {1};
      priceList.setUsername(settings.userName);
      priceList.setPassword(settings.password);
      priceList.setUrl(settings.urlString);
      priceList.setCommand("select COF_NAME, PRICE from COFFEES");
      priceList.setKeyColumns(keyCols);

      // Populate the WebRowSet
      priceList.execute();

```

At this point, in addition to the default properties, the `priceList` object contains the data in the `COF_NAME` and `PRICE` columns from the `COFFEES` table and also the metadata about these two columns.

## <a name="writing-and-reading-webrowset-object-to-xml" id="writing-and-reading-webrowset-object-to-xml">Writing and Reading WebRowSet Object to XML</a>

To write a `WebRowSet` object as an XML document, call the method `writeXml`. To read that XML document's contents into a `WebRowSet` object, call the method `readXml`. Both of these methods do their work in the background, meaning that everything, except the results, is invisible to you.

### Using the writeXml Method

The method `writeXml` writes the `WebRowSet` object that invoked it as an XML document that represents its current state. It writes this XML document to the stream that you pass to it. The stream can be an `OutputStream` object, such as a `FileOutputStream` object, or a `Writer` object, such as a `FileWriter` object. If you pass the method `writeXml` an `OutputStream` object, you will write in bytes, which can handle all types of data; if you pass it a `Writer` object, you will write in characters. The following code demonstrates writing the `WebRowSet` object `priceList` as an XML document to the `FileOutputStream` object `oStream`:

```

java.io.FileOutputStream oStream =
    new java.io.FileOutputStream("priceList.xml");
priceList.writeXml(oStream);

```

The following code writes the XML document representing `priceList` to the `FileWriter` object `writer` instead of to an `OutputStream` object. The `FileWriter` class is a convenience class for writing characters to a file.

```

java.io.FileWriter writer =
    new java.io.FileWriter("priceList.xml");
priceList.writeXml(writer);

```

The other two versions of the method `writeXml` let you populate a `WebRowSet` object with the contents of a `ResultSet` object before writing it to a stream. In the following line of code, the method `writeXml` reads the contents of the `ResultSet` object `rs` into the `priceList` object and then writes `priceList` to the `FileOutputStream` object `oStream` as an XML document.

```

priceList.writeXml(rs, oStream);

```

In the next line of code, the `writeXml` methodpopulates `priceList` with the contents of `rs`, but it writes the XML document to a `FileWriter` object instead of to an `OutputStream` object:

```

priceList.writeXml(rs, writer);

```

### Using the readXml Method

The method `readXml` parses an XML document in order to construct the `WebRowSet` object the XML document describes. Similar to the method `writeXml`, you can pass `readXml` an `InputStream` object or a `Reader` object from which to read the XML document.

```

java.io.FileInputStream iStream =
    new java.io.FileInputStream("priceList.xml");
priceList.readXml(iStream);

java.io.FileReader reader = new
    java.io.FileReader("priceList.xml");
priceList.readXml(reader);

```

Note that you can read the XML description into a new `WebRowSet` object or into the same `WebRowSet` object that called the `writeXml` method. In the scenario, where the price list information is being sent from headquarters to the Web site, you would use a new `WebRowSet` object, as shown in the following lines of code:

```

WebRowSet recipient = new WebRowSetImpl();
java.io.FileReader reader =
    new java.io.FileReader("priceList.xml");
recipient.readXml(reader);

```

## <a name="what-is-in-xml-document" id="what-is-in-xml-document">What Is in XML Documents</a>

`RowSet` objects are more than just the data they contain. They have properties and metadata about their columns as well. Therefore, an XML document representing a `WebRowSet` object includes this other information in addition to its data. Further, the data in an XML document includes both current values and original values. (Recall that original values are the values that existed immediately before the most recent changes to data were made. These values are necessary for checking if the corresponding value in the database has been changed, thus creating a conflict over which value should be persistent: the new value you put in the `RowSet` object or the new value someone else put in the database.)

The WebRowSet XML Schema, itself an XML document, defines what an XML document representing a `WebRowSet` object will contain and also the format in which it must be presented. Both the sender and the recipient use this schema because it tells the sender how to write the XML document (which represents the `WebRowSet` object) and the recipient how to parse the XML document. Because the actual writing and reading is done internally by the implementations of the methods `writeXml` and `readXml`, you, as a user, do not need to understand what is in the WebRowSet XML Schema document.

XML documents contain elements and subelements in a hierarchical structure. The following are the three main elements in an XML document describing a `WebRowSet` object:

- [Properties](#properties-webrowset)
- [Metadata](#metadata-webrowset)
- [Data](#data-webrowset)

Element tags signal the beginning and end of an element. For example, the `&lt;properties&gt;` tag signals the beginning of the properties element, and the `&lt;/properties&gt;` tag signals its end. The `&lt;map/&gt;` tag is a shorthand way of saying that the map subelement (one of the subelements in the properties element) has not been assigned a value. The following sample XML documents uses spacing and indentation to make it easier to read, but those are not used in an actual XML document, where spacing does not mean anything.

The next three sections show you what the three main elements contain for the `WebRowSet` `priceList` object, created in the sample [`WebRowSetSample`](gettingstarted.html).

### <a name="properties-webrowset" id="properties-webrowset">Properties</a>

Calling the method `writeXml` on the `priceList` object would produce an XML document describing `priceList`. The properties section of this XML document would look like the following:

```

&lt;properties&gt;
  &lt;command&gt;
    select COF_NAME, PRICE from COFFEES
  &lt;/command&gt;
  &lt;concurrency&gt;1008&lt;/concurrency&gt;
  &lt;datasource&gt;&lt;null/&gt;&lt;/datasource&gt;
  &lt;escape-processing&gt;true&lt;/escape-processing&gt;
  &lt;fetch-direction&gt;1000&lt;/fetch-direction&gt;
  &lt;fetch-size&gt;0&lt;/fetch-size&gt;
  &lt;isolation-level&gt;2&lt;/isolation-level&gt;
  &lt;key-columns&gt;
    &lt;column&gt;1&lt;/column&gt;
  &lt;/key-columns&gt;
  &lt;map&gt;
  &lt;/map&gt;
  &lt;max-field-size&gt;0&lt;/max-field-size&gt;
  &lt;max-rows&gt;0&lt;/max-rows&gt;
  &lt;query-timeout&gt;0&lt;/query-timeout&gt;
  &lt;read-only&gt;true&lt;/read-only&gt;
  &lt;rowset-type&gt;
    ResultSet.TYPE_SCROLL_INSENSITIVE
  &lt;/rowset-type&gt;
  &lt;show-deleted&gt;false&lt;/show-deleted&gt;
  &lt;table-name&gt;COFFEES&lt;/table-name&gt;
  &lt;url&gt;jdbc:mysql://localhost:3306/testdb&lt;/url&gt;
  &lt;sync-provider&gt;
    &lt;sync-provider-name&gt;
      com.sun.rowset.providers.RIOptimisticProvider
    &lt;/sync-provider-name&gt;
    &lt;sync-provider-vendor&gt;
      Sun Microsystems Inc.
    &lt;/sync-provider-vendor&gt;
    &lt;sync-provider-version&gt;
      1.0
    &lt;/sync-provider-version&gt;
    &lt;sync-provider-grade&gt;
      2
    &lt;/sync-provider-grade&gt;
    &lt;data-source-lock&gt;1&lt;/data-source-lock&gt;
  &lt;/sync-provider&gt;
&lt;/properties&gt;

```

Notice that some properties have no value. For example, the `datasource` property is indicated with the `&lt;datasource/&gt;` tag, which is a shorthand way of saying `&lt;datasource&gt;&lt;/datasource&gt;`. No value is given because the `url` property is set. Any connections that are established will be done using this JDBC URL, so no `DataSource` object needs to be set. Also, the `username` and `password` properties are not listed because they must remain secret.

### <a name="metadata-webrowset" id="metadata-webrowset">Metadata</a>

The metadata section of the XML document describing a `WebRowSet` object contains information about the columns in that `WebRowSet` object. The following shows what this section looks like for the `WebRowSet` object `priceList`. Because the `priceList` object has two columns, the XML document describing it has two `&lt;column-definition&gt;` elements. Each `&lt;column-definition&gt;` element has subelements giving information about the column being described.

```

&lt;metadata&gt;
  &lt;column-count&gt;2&lt;/column-count&gt;
  &lt;column-definition&gt;
    &lt;column-index&gt;1&lt;/column-index&gt;
    &lt;auto-increment&gt;false&lt;/auto-increment&gt;
    &lt;case-sensitive&gt;false&lt;/case-sensitive&gt;
    &lt;currency&gt;false&lt;/currency&gt;
    &lt;nullable&gt;0&lt;/nullable&gt;
    &lt;signed&gt;false&lt;/signed&gt;
    &lt;searchable&gt;true&lt;/searchable&gt;
    &lt;column-display-size&gt;
      32
    &lt;/column-display-size&gt;
    &lt;column-label&gt;COF_NAME&lt;/column-label&gt;
    &lt;column-name&gt;COF_NAME&lt;/column-name&gt;
    &lt;schema-name&gt;&lt;/schema-name&gt;
    &lt;column-precision&gt;32&lt;/column-precision&gt;
    &lt;column-scale&gt;0&lt;/column-scale&gt;
    &lt;table-name&gt;coffees&lt;/table-name&gt;
    &lt;catalog-name&gt;testdb&lt;/catalog-name&gt;
    &lt;column-type&gt;12&lt;/column-type&gt;
    &lt;column-type-name&gt;
      VARCHAR
    &lt;/column-type-name&gt;
  &lt;/column-definition&gt;
  &lt;column-definition&gt;
    &lt;column-index&gt;2&lt;/column-index&gt;
    &lt;auto-increment&gt;false&lt;/auto-increment&gt;
    &lt;case-sensitive&gt;true&lt;/case-sensitive&gt;
    &lt;currency&gt;false&lt;/currency&gt;
    &lt;nullable&gt;0&lt;/nullable&gt;
    &lt;signed&gt;true&lt;/signed&gt;
    &lt;searchable&gt;true&lt;/searchable&gt;
    &lt;column-display-size&gt;
      12
    &lt;/column-display-size&gt;
    &lt;column-label&gt;PRICE&lt;/column-label&gt;
    &lt;column-name&gt;PRICE&lt;/column-name&gt;
    &lt;schema-name&gt;&lt;/schema-name&gt;
    &lt;column-precision&gt;10&lt;/column-precision&gt;
    &lt;column-scale&gt;2&lt;/column-scale&gt;
    &lt;table-name&gt;coffees&lt;/table-name&gt;
    &lt;catalog-name&gt;testdb&lt;/catalog-name&gt;
    &lt;column-type&gt;3&lt;/column-type&gt;
    &lt;column-type-name&gt;
      DECIMAL
    &lt;/column-type-name&gt;
  &lt;/column-definition&gt;
&lt;/metadata&gt;

```

From this metadata section, you can see that there are two columns in each row. The first column is `COF_NAME`, which holds values of type `VARCHAR`. The second column is `PRICE`, which holds values of type `REAL`, and so on. Note that the column types are the data types used in the data source, not types in the Java programming language. To get or update values in the `COF_NAME` column, you use the methods `getString` or `updateString`, and the driver makes the conversion to the `VARCHAR` type, as it usually does.

### <a name="data-webrowset" id="data-webrowset">Data</a>

The data section gives the values for each column in each row of a `WebRowSet` object. If you have populated the `priceList` object and not made any changes to it, the data element of the XML document will look like the following. In the next section you will see how the XML document changes when you modify the data in the `priceList` object.

For each row there is a `&lt;currentRow&gt;` element, and because `priceList` has two columns, each `&lt;currentRow&gt;` element contains two `&lt;columnValue&gt;` elements.

```

&lt;data&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;Colombian&lt;/columnValue&gt;
    &lt;columnValue&gt;7.99&lt;/columnValue&gt;
  &lt;/currentRow&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;
      Colombian_Decaf
    &lt;/columnValue&gt;
    &lt;columnValue&gt;8.99&lt;/columnValue&gt;
  &lt;/currentRow&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;Espresso&lt;/columnValue&gt;
    &lt;columnValue&gt;9.99&lt;/columnValue&gt;
  &lt;/currentRow&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;French_Roast&lt;/columnValue&gt;
    &lt;columnValue&gt;8.99&lt;/columnValue&gt;
  &lt;/currentRow&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;French_Roast_Decaf&lt;/columnValue&gt;
    &lt;columnValue&gt;9.99&lt;/columnValue&gt;
  &lt;/currentRow&gt;
&lt;/data&gt;

```

## <a name="making-changes-to-webrowset-object" id="making-changes-to-webrowset-object">Making Changes to WebRowSet Objects</a>

You make changes to a `WebRowSet` object the same way you do to a `CachedRowSet` object. Unlike a `CachedRowSet` object, however, a `WebRowSet` object keeps track of updates, insertions, and deletions so that the `writeXml` method can write both the current values and the original values. The three sections that follow demonstrate making changes to the data and show what the XML document describing the `WebRowSet` object looks like after each change. You do not have to do anything at all regarding the XML document; any change to it is made automatically, just as with writing and reading the XML document.

### Inserting Rows

If the owner of the Coffee Break chain wants to add a new coffee to the price list, the code might look like this:

```

priceList.absolute(3);
priceList.moveToInsertRow();
priceList.updateString(COF_NAME, "Kona");
priceList.updateFloat(PRICE, 8.99f);
priceList.insertRow();
priceList.moveToCurrentRow();

```

In the reference implementation, an insertion is made immediately following the current row. In the preceding code fragment, the current row is the third row, so the new row would be added after the third row and become the new fourth row. To reflect this insertion, the XML document would have the following `&lt;insertRow&gt;` element added to it after the third `&lt;currentRow&gt;` element in the `&lt;data&gt;` element.

The `&lt;insertRow&gt;` element will look similar to the following.

```

&lt;insertRow&gt;
  &lt;columnValue&gt;Kona&lt;/columnValue&gt;
  &lt;columnValue&gt;8.99&lt;/columnValue&gt;
&lt;/insertRow&gt;

```

## Deleting Rows

The owner decides that Espresso is not selling enough and should be removed from the coffees sold at The Coffee Break shops. The owner therefore wants to delete Espresso from the price list. Espresso is in the third row of the `priceList` object, so the following lines of code delete it:

```

priceList.absolute(3); priceList.deleteRow();

```

The following `&lt;deleteRow&gt;` element will appear after the second row in the data section of the XML document, indicating that the third row has been deleted.

```

&lt;deleteRow&gt;
  &lt;columnValue&gt;Espresso&lt;/columnValue&gt;
  &lt;columnValue&gt;9.99&lt;/columnValue&gt;
&lt;/deleteRow&gt;


```

## Modifying Rows

The owner further decides that the price of Colombian coffee is too expensive and wants to lower it to $6.99 a pound. The following code sets the new price for Colombian coffee, which is in the first row, to $6.99 a pound:

```

priceList.first();
priceList.updateFloat(PRICE, 6.99);

```

The XML document will reflect this change in an `&lt;updateRow&gt;` element that gives the new value. The value for the first column did not change, so there is an `&lt;updateValue&gt;` element only for the second column:

```

&lt;currentRow&gt;
  &lt;columnValue&gt;Colombian&lt;/columnValue&gt;
  &lt;columnValue&gt;7.99&lt;/columnValue&gt;
  &lt;updateRow&gt;6.99&lt;/updateRow&gt;
&lt;/currentRow&gt;


```

At this point, with the insertion of a row, the deletion of a row, and the modification of a row, the XML document for the `priceList` object would look like the following:

```

&lt;data&gt;
  &lt;insertRow&gt;
    &lt;columnValue&gt;Kona&lt;/columnValue&gt;
    &lt;columnValue&gt;8.99&lt;/columnValue&gt;
  &lt;/insertRow&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;Colombian&lt;/columnValue&gt;
    &lt;columnValue&gt;7.99&lt;/columnValue&gt;
    &lt;updateRow&gt;6.99&lt;/updateRow&gt;
  &lt;/currentRow&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;
      Colombian_Decaf
    &lt;/columnValue&gt;
    &lt;columnValue&gt;8.99&lt;/columnValue&gt;
  &lt;/currentRow&gt;
  &lt;deleteRow&gt;
    &lt;columnValue&gt;Espresso&lt;/columnValue&gt;
    &lt;columnValue&gt;9.99&lt;/columnValue&gt;
  &lt;/deleteRow&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;French_Roast&lt;/columnValue&gt;
    &lt;columnValue&gt;8.99&lt;/columnValue&gt;
  &lt;/currentRow&gt;
  &lt;currentRow&gt;
    &lt;columnValue&gt;
      French_Roast_Decaf
    &lt;/columnValue&gt;
    &lt;columnValue&gt;9.99&lt;/columnValue&gt;
  &lt;/currentRow&gt;
&lt;/data&gt;

```

## WebRowSet Code Example

The sample `[WebRowSetSample.java](gettingstarted.html)` demonstrates all the features described on this page.
