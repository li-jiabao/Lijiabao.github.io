
# Establishing a Connection

First, you need to establish a connection with the data source you want to use. A data source can be a DBMS, a legacy file system, or some other source of data with a corresponding JDBC driver. Typically, a JDBC application connects to a target data source using one of two classes:

<li>
`DriverManager`: This fully implemented class connects an application to a data source, which is specified by a database URL. When this class first attempts to establish a connection, it automatically loads any JDBC 4.0 drivers found within the class path. Note that your application must manually load any JDBC drivers prior to version 4.0.
</li>
<li>
`DataSource`: This interface is preferred over `DriverManager` because it allows details about the underlying data source to be transparent to your application. A `DataSource` object's properties are set so that it represents a particular data source. See [Connecting with DataSource Objects](sqldatasources.html) for more information. For more information about developing applications with the `DataSource` class, see the latest **[The Java EE Tutorial](https://javaee.github.io/tutorial/toc.html)**.
</li>

**Note**: The samples in this tutorial use the `DriverManager` class instead of the `DataSource` class because it is easier to use and the samples do not require the features of the `DataSource` class.

This page covers the following topics:

- [Using the DriverManager Class](#drivermanager)
- [Specifying Database Connection URLs](#db_connection_url)

## <a name="drivermanager" id="drivermanager">Using the DriverManager Class</a>

Connecting to your DBMS with the `DriverManager` class involves calling the method `DriverManager.getConnection`. The following method, `[JDBCTutorialUtilities.getConnection](gettingstarted.html)`, establishes a database connection:

```

public Connection getConnection() throws SQLException {

    Connection conn = null;
    Properties connectionProps = new Properties();
    connectionProps.put("user", this.userName);
    connectionProps.put("password", this.password);

    if (this.dbms.equals("mysql")) {
        conn = DriverManager.getConnection(
                   "jdbc:" + this.dbms + "://" +
                   this.serverName +
                   ":" + this.portNumber + "/",
                   connectionProps);
    } else if (this.dbms.equals("derby")) {
        conn = DriverManager.getConnection(
                   "jdbc:" + this.dbms + ":" +
                   this.dbName +
                   ";create=true",
                   connectionProps);
    }
    System.out.println("Connected to database");
    return conn;
}

```

The method `DriverManager.getConnection` establishes a database connection. This method requires a database URL, which varies depending on your DBMS. The following are some examples of database URLs:

<li>
MySQL: `jdbc:mysql://localhost:3306/`, where `localhost` is the name of the server hosting your database, and `3306` is the port number
</li>
<li>
Java DB: `jdbc:derby:**testdb**;create=true`, where `**testdb**` is the name of the database to connect to, and `create=true` instructs the DBMS to create the database.
**Note**: This URL establishes a database connection with the Java DB Embedded Driver. Java DB also includes a Network Client Driver, which uses a different URL.
</li>

This method specifies the user name and password required to access the DBMS with a `Properties` object.

**Note**:

<li>
Typically, in the database URL, you also specify the name of an existing database to which you want to connect. For example, the URL `jdbc:mysql://localhost:3306/mysql` represents the database URL for the MySQL database named `mysql`. The samples in this tutorial use a URL that does not specify a specific database because the samples create a new database.
</li>
<li>
In previous versions of JDBC, to obtain a connection, you first had to initialize your JDBC driver by calling the method `Class.forName`. This methods required an object of type `java.sql.Driver`. Each JDBC driver contains one or more classes that implements the interface `java.sql.Driver`. The drivers for Java DB are `org.apache.derby.jdbc.EmbeddedDriver` and `org.apache.derby.jdbc.ClientDriver`, and the one for MySQL Connector/J is `com.mysql.cj.jdbc.Driver`. See the documentation of your DBMS driver to obtain the name of the class that implements the interface `java.sql.Driver`.
Any JDBC 4.0 drivers that are found in your class path are automatically loaded. (However, you must manually load any drivers prior to JDBC 4.0 with the method `Class.forName`.)
</li>

The method returns a `Connection` object, which represents a connection with the DBMS or a specific database. Query the database through this object.

## <a name="db_connection_url" id="db_connection_url">Specifying Database Connection URLs</a>

A database connection URL is a string that your DBMS JDBC driver uses to connect to a database. It can contain information such as where to search for the database, the name of the database to connect to, and configuration properties. The exact syntax of a database connection URL is specified by your DBMS.

### Java DB Database Connection URLs

The following is the database connection URL syntax for Java DB:

```

jdbc:derby:[**subsubprotocol**:][**databaseName**][;**attribute**=**value**]*

```

- `**subsubprotocol**` specifies where Java DB should search for the database, either in a directory, in memory, in a class path, or in a JAR file. It is typically omitted.
- `**databaseName**` is the name of the database to connect to.
<li>`**attribute**=**value**` represents an optional, semicolon-separated list of attributes. These attributes enable you to instruct Java DB to perform various tasks, including the following:
<ul>
- Create the database specified in the connection URL.
- Encrypt the database specified in the connection URL.
- Specify directories to store logging and trace information.
- Specify a user name and password to connect to the database.

See **Java DB Developer's Guide** and **Java DB Reference Manual** from [Java DB Technical Documentation](https://docs.oracle.com/javadb/index_jdk8.html) for more information.

### MySQL Connector/J Database URL

The following is the database connection URL syntax for MySQL Connector/J:

```

jdbc:mysql://[**host**][,**failoverhost**...]
    [:**port**]/[**database**]
    [?**propertyName1**][=**propertyValue1**]
    [&amp;**propertyName2**][=**propertyValue2**]...

```

- `**host**:**port**` is the host name and port number of the computer hosting your database. If not specified, the default values of `**host**` and `**port**` are 127.0.0.1 and 3306, respectively.
- `**database**` is the name of the database to connect to. If not specified, a connection is made with no default database.
- `**failover**` is the name of a standby database (MySQL Connector/J supports failover).
- `**propertyName**=**propertyValue**` represents an optional, ampersand-separated list of properties. These attributes enable you to instruct MySQL Connector/J to perform various tasks.

See [**MySQL Reference Manual**](https://dev.mysql.com/doc/) for more information.
