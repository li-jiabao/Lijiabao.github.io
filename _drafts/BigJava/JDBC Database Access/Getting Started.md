
# Getting Started

The sample code that comes with this tutorial creates a database that is used by a proprietor of a small coffee house called The Coffee Break, where coffee beans are sold by the pound and brewed coffee is sold by the cup.

The following steps configure a JDBC development environment with which you can compile and run the tutorial samples:

  1. [Install the latest version of the Java SE SDK on your computer](#step1)
  1. [Install your database management system (DBMS) if needed](#step2)
  1. [Install a JDBC driver from the vendor of your database](#step3)
  1. [Install Apache Ant](#step4)
  1. [Install Apache Xalan](#step5)
  1. [Download the sample code](#step6)
  1. [Modify the `build.xml` file](#step7)
  1. [Modify the tutorial properties file](#step8)
  1. [Compile and package the samples](#step9)
  1. [Create databases, tables, and populate tables](#step10)
  1. [Run the samples](#step11)

## <a name="step1" id="step1">Install the latest version of the Java SE SDK on your computer</a>

Install the latest version of the Java SE SDK on your computer.

Ensure that the full directory path of the Java SE SDK `bin` directory is in your `PATH` environment variable so that you can run the Java compiler and the Java application launcher from any directory.

## <a name="step2" id="step2">Install your database management system (DBMS) if needed</a>

This tutorial has been tested for the following DBMS:

<li>[Java DB](https://www.oracle.com/java/technologies/javadb.html)
**Note**: Java DB is no longer included in recent versions of the JDK. Java DB was a rebranding of Apache Derby. If you would like to use Java DB, then download the latest version from [The Apache DB Project](https://db.apache.org/derby/).
</li>
- [MySQL](https://www.mysql.com/)

Note that if you are using another DBMS, you might have to alter the code of the tutorial samples.

## <a name="step3" id="step3">Install a JDBC driver from the vendor of your database</a>

If you are using Java DB, it already comes with a JDBC driver. If you are using MySQL, install the latest version of the JDBC driver for MySQL, [Connector/J](https://dev.mysql.com/downloads/connector/j/).

Contact the vendor of your database to obtain a JDBC driver for your DBMS.

There are many possible implementations of JDBC drivers. These implementations are categorized as follows:

<li>
**Type 1**: Drivers that implement the JDBC API as a mapping to another data access API, such as ODBC (Open Database Connectivity). Drivers of this type are generally dependent on a native library, which limits their portability. The JDBC-ODBC Bridge is an example of a Type 1 driver.
**Note**: The JDBC-ODBC Bridge should be considered a transitional solution. It is not supported by Oracle. Consider using this only if your DBMS does not offer a Java-only JDBC driver.
</li>
<li>
**Type 2**: Drivers that are written partly in the Java programming language and partly in native code. These drivers use a native client library specific to the data source to which they connect. Again, because of the native code, their portability is limited. Oracle's OCI (Oracle Call Interface) client-side driver is an example of a Type 2 driver.
</li>
<li>
**Type 3**: Drivers that use a pure Java client and communicate with a middleware server using a database-independent protocol. The middleware server then communicates the client's requests to the data source.
</li>
<li>
**Type 4**: Drivers that are pure Java and implement the network protocol for a specific data source. The client connects directly to the data source.
</li>

Check which driver types comes with your DBMS. Java DB comes with two Type 4 drivers, an Embedded driver and a Network Client Driver. MySQL Connector/J is a Type 4 driver.

Installing a JDBC driver generally consists of copying the driver to your computer, then adding the location of it to your class path. In addition, many JDBC drivers other than Type 4 drivers require you to install a client-side API. No other special configuration is usually needed.

## <a name="step4" id="step4">Install Apache Ant</a>

These steps use Apache Ant, a Java-based tool, to build, compile, and run the JDBC tutorial samples. Go to the following link to download Apache Ant:

`[https://ant.apache.org/](https://ant.apache.org/)`

Ensure that the Apache Ant executable file is in your `PATH` environment variable so that you can run it from any directory.

## <a name="step5" id="step5">Install Apache Xalan</a>

The sample `RSSFeedsTable.java`, which is described in [Using SQLXML Objects](sqlxml.html), requires Apache Xalan if your DBMS is Java DB. The sample uses Apache Xalan-Java. Go to the following link to download it:

`[https://xml.apache.org/xalan-j/](https://xml.apache.org/xalan-j/)`

## <a name="step6" id="step6">Download the sample code</a>

The sample code, `[JDBCTutorial.zip](examples/zipfiles/JDBCTutorial.zip)`, consists of the following files:

<li>`properties`
<ul>
- `javadb-build-properties.xml`
- `javadb-sample-properties.xml`
- `mysql-build-properties.xml`
- `mysql-sample-properties.xml`

<li>`javadb`
<ul>
- `create-procedures.sql`
- `create-tables.sql`
- `drop-tables.sql`
- `populate-tables.sql`

- `create-procedures.sql`
- `create-tables.sql`
- `drop-tables.sql`
- `populate-tables.sql`

- `CachedRowSetSample.java`
- `CityFilter.java`
- `ClobSample.java`
- `CoffeesFrame.java`
- `CoffeesTable.java`
- `CoffeesTableModel.java`
- `DatalinkSample.java`
- `ExampleRowSetListener.java`
- `FilteredRowSetSample.java`
- `JdbcRowSetSample.java`
- `JDBCTutorialUtilities.java`
- `JoinSample.java`
- `ProductInformationTable.java`
- `RSSFeedsTable.java`
- `StateFilter.java`
- `StoredProcedureJavaDBSample.java`
- `StoredProcedureMySQLSample.java`
- `SuppliersTable.java`
- `WebRowSetSample.java`

- `colombian-description.txt`

- `rss-coffee-industry-news.xml`
- `rss-the-coffee-break-blog.xml`

Create a directory to contain all the files of the sample. These steps refer to this directory as `**&lt;JDBC tutorial directory&gt;**`. Unzip the contents of `[JDBCTutorial.zip](examples/zipfiles/JDBCTutorial.zip)` into `**&lt;JDBC tutorial directory&gt;**`.

## <a name="step7" id="step7">Modify the build.xml file</a>

The `build.xml` file is the build file that Apache Ant uses to compile and execute the JDBC samples. The files `properties/javadb-build-properties.xml` and `properties/mysql-build-properties.xml` contain additional Apache Ant properties required for Java DB and MySQL, respectively. The files `properties/javadb-sample-properties.xml` and `properties/mysql-sample-properties.xml` contain properties required by the sample.

Modify these XML files as follows:

### Modify build.xml

In the `build.xml` file, modify the property `ANTPROPERTIES` to refer to either `properties/javadb-build-properties.xml` or `properties/mysql-build-properties.xml`, depending on your DBMS. For example, if you are using Java DB, your `build.xml` file would contain this:

```

&lt;property
  name="ANTPROPERTIES"
  value="properties/javadb-build-properties.xml"/&gt;

  &lt;import file="${ANTPROPERTIES}"/&gt;

```

Similarly, if you are using MySQL, your `build.xml` file would contain this:

```

&lt;property
  name="ANTPROPERTIES"
  value="properties/mysql-build-properties.xml"/&gt;

  &lt;import file="${ANTPROPERTIES}"/&gt;

```

### Modify database-specific properties file

In the `properties/javadb-build-properties.xml` or `properties/mysql-build-properties.xml` file (depending on your DBMS), modify the following properties, as described in the following table:
<th id="h1">Property</th><th id="h2">Description</th>
<td headers="h1">`JAVAC`</td><td headers="h2">The full path name of your Java compiler, `javac`</td>
<td headers="h1">`JAVA`</td><td headers="h2">The full path name of your Java runtime executable, `java`</td>
<td headers="h1">`PROPERTIESFILE`</td><td headers="h2">The name of the properties file, either `properties/javadb-sample-properties.xml` or `properties/mysql-sample-properties.xml`</td>
<td headers="h1">`MYSQLDRIVER`</td><td headers="h2">The full path name of your MySQL driver. For Connector/J, this is typically `**&lt;Connector/J installation directory&gt;**/mysql-connector-java-**version-number**.jar`.</td>
<td headers="h1">`JAVADBDRIVER`</td><td headers="h2">The full path name of your Java DB driver. This is typically `**&lt;Java DB installation directory&gt;**/lib/derby.jar`.</td>
<td headers="h1">`XALANDIRECTORY`</td><td headers="h2">The full path name of the directory that contains Apache Xalan.</td>
<td headers="h1">`CLASSPATH`</td><td headers="h2">The class path that the JDBC tutorial uses. **You do not need to change this value.**</td>
<td headers="h1">`XALAN`</td><td headers="h2">The full path name of the file `xalan.jar`.</td>
<td headers="h1">`DB.VENDOR`</td><td headers="h2">A value of either `derby` or `mysql` depending on whether you are using Java DB or MySQL, respectively. The tutorial uses this value to construct the URL required to connect to the DBMS and identify DBMS-specific code and SQL statements.</td>
<td headers="h1">`DB.DRIVER`</td><td headers="h2">The fully qualified class name of the JDBC driver. For Java DB, this is `org.apache.derby.jdbc.EmbeddedDriver`. For MySQL, this is `com.mysql.cj.jdbc.Driver`.</td>
<td headers="h1">`DB.HOST`</td><td headers="h2">The host name of the computer hosting your DBMS.</td>
<td headers="h1">`DB.PORT`</td><td headers="h2">The port number of the computer hosting your DBMS.</td>
<td headers="h1">`DB.SID`</td><td headers="h2">The name of the database the tutorial creates and uses.</td>
<td headers="h1">`DB.URL.NEWDATABASE`</td><td headers="h2">The connection URL used to connect to your DBMS when creating a new database. **You do not need to change this value.**</td>
<td headers="h1">`DB.URL`</td><td headers="h2">The connection URL used to connect to your DBMS. **You do not need to change this value.**</td>
<td headers="h1">`DB.USER`</td><td headers="h2">The name of the user that has access to create databases in the DBMS.</td>
<td headers="h1">`DB.PASSWORD`</td><td headers="h2">The password of the user specified in `DB.USER`.</td>
<td headers="h1">`DB.DELIMITER`</td><td headers="h2">The character used to separate SQL statements. **Do not change this value.** It should be the semicolon character (`;`).</td>

## <a name="step8" id="step8">Modify the tutorial properties file</a>

The tutorial samples use the values in either the `properties/javadb-sample-properties.xml` file or `properties/mysql-sample-properties.xml` file (depending on your DBMS) to connect to the DBMS and initialize databases and tables, as described in the following table:
<th id="h101">Property</th><th id="h102">Description</th>
<td headers="h101">`dbms`</td><td headers="h102">A value of either `derby` or `mysql` depending on whether you are using Java DB or MySQL, respectively. The tutorial uses this value to construct the URL required to connect to the DBMS and identify DBMS-specific code and SQL statements.</td>
<td headers="h101">`jar_file`</td><td headers="h102">The full path name of the JAR file that contains all the class files of this tutorial.</td>
<td headers="h101">`driver`</td><td headers="h102">The fully qualified class name of the JDBC driver. For Java DB, this is `org.apache.derby.jdbc.EmbeddedDriver`. For MySQL, this is `com.mysql.cj.jdbc.Driver`.</td>
<td headers="h101">`database_name`</td><td headers="h102">The name of the database the tutorial creates and uses.</td>
<td headers="h101">`user_name`</td><td headers="h102">The name of the user that has access to create databases in the DBMS.</td>
<td headers="h101">`password`</td><td headers="h102">The password of the user specified in `user_name`.</td>
<td headers="h101">`server_name`</td><td headers="h102">The host name of the computer hosting your DBMS.</td>
<td headers="h101">`port_number`</td><td headers="h102">The port number of the computer hosting your DBMS.</td>

**Note**: For simplicity in demonstrating the JDBC API, the JDBC tutorial sample code does not perform the password management techniques that a deployed system normally uses. In a production environment, you can follow the Oracle Database password management guidelines and disable any sample accounts. See the section [Securing Passwords in Application Design](https://docs.oracle.com/en/database/oracle/oracle-database/20/dbseg/managing-security-for-application-developers.html#GUID-DE90CA6F-F37B-4337-B331-4FA0F7C7599A) in **Oracle Database Security Guide** for password management guidelines and other security recommendations.

## <a name="step9" id="step9">Compile and package the samples</a>

At a command prompt, change the current directory to `**&lt;JDBC tutorial directory&gt;**`. From this directory, run the following command to compile the samples and package them in a jar file:

```

ant jar

```

## <a name="step10" id="step10">Create databases, tables, and populate tables</a>

If you are using MySQL, then run the following command to create a database:

```

ant create-mysql-database

```

**Note**: No corresponding Ant target exists in the `build.xml` file that creates a database for Java DB. The database URL for Java DB, which is used to establish a database connection, includes the option to create the database (if it does not already exist). See
[Establishing a Connection](connecting.html) for more information.

If you are using either Java DB or MySQL, then from the same directory, run the following command to delete existing sample database tables, recreate the tables, and populate them. For Java DB, this command also creates the database if it does not already exist:

```

ant setup

```

**Note**: You should run the command `ant setup` every time before you run one of the Java classes in the sample. Many of these samples expect specific data in the contents of the sample's database tables.

## <a name="step11" id="step11">Run the samples</a>

Each target in the `build.xml` file corresponds to a Java class or SQL script in the JDBC samples. The following table lists the targets in the `build.xml` file, which class or script each target executes, and other classes or files each target requires:
<th id="h201">Ant Target</th><th id="h202">Class or SQL Script</th><th id="h203">Other Required Classes or Files</th>
<td headers="h201">`javadb-create-procedure`</td><td headers="h202">`javadb/create-procedures.sql`; see the `build.xml` file to view other SQL statements that are run</td><td headers="h203">No other required files</td>
<td headers="h201">`mysql-create-procedure`</td><td headers="h202">`mysql/create-procedures.sql`.</td><td headers="h203">No other required files</td>
<td headers="h201">`run`</td><td headers="h202">`JDBCTutorialUtilities`</td><td headers="h203">No other required classes</td>
<td headers="h201">`runct`</td><td headers="h202">`CoffeesTable`</td><td headers="h203">`JDBCTutorialUtilities`</td>
<td headers="h201">`runst`</td><td headers="h202">`SuppliersTable`</td><td headers="h203">`JDBCTutorialUtilities`</td>
<td headers="h201">`runjrs`</td><td headers="h202">`JdbcRowSetSample`</td><td headers="h203">`JDBCTutorialUtilities`</td>
<td headers="h201">`runcrs`</td><td headers="h202">`CachedRowSetSample`, `ExampleRowSetListener`</td><td headers="h203">`JDBCTutorialUtilities`</td>
<td headers="h201">`runjoin`</td><td headers="h202">`JoinSample`</td><td headers="h203">`JDBCTutorialUtilities`</td>
<td headers="h201">`runfrs`</td><td headers="h202">`FilteredRowSetSample`</td><td headers="h203">`JDBCTutorialUtilities`, `CityFilter`, `StateFilter`</td>
<td headers="h201">`runwrs`</td><td headers="h202">`WebRowSetSample`</td><td headers="h203">`JDBCTutorialUtilities`</td>
<td headers="h201">`runclob`</td><td headers="h202">`ClobSample`</td><td headers="h203">`JDBCTutorialUtilities`, `txt/colombian-description.txt`</td>
<td headers="h201">`runrss`</td><td headers="h202">`RSSFeedsTable`</td><td headers="h203">`JDBCTutorialUtilities`, the XML files contained in the `xml` directory</td>
<td headers="h201">`rundl`</td><td headers="h202">`DatalinkSample`</td><td headers="h203">`JDBCTutorialUtilities`</td>
<td headers="h201">`runspjavadb`</td><td headers="h202">`StoredProcedureJavaDBSample`</td><td headers="h203">`JDBCTutorialUtilities`, `SuppliersTable`, `CoffeesTable`</td>
<td headers="h201">`runspmysql`</td><td headers="h202">`StoredProcedureMySQLSample`</td><td headers="h203">`JDBCTutorialUtilities`, `SuppliersTable`, `CoffeesTable`</td>
<td headers="h201">`runframe`</td><td headers="h202">`CoffeesFrame`</td><td headers="h203">`JDBCTutorialUtilities`, `CoffeesTableModel`</td>

For example, to run the class `CoffeesTable`, change the current directory to `**&lt;JDBC tutorial directory&gt;**`, and from this directory, run the following command:

```

ant runct

```
