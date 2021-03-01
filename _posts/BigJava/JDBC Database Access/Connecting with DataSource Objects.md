
# Connecting with DataSource Objects

### Connecting with DataSource Objects

This section covers `DataSource` objects, which are the preferred means of getting a connection to a data source. In addition to their other advantages, which will be explained later, `DataSource` objects can provide connection pooling and distributed transactions. This functionality is essential for enterprise database computing. In particular, it is integral to Enterprise JavaBeans (EJB) technology.

This section shows you how to get a connection using the `DataSource` interface and how to use distributed transactions and connection pooling. Both of these involve very few code changes in your JDBC application.

The work performed to deploy the classes that make these operations possible, which a system administrator usually does with a tool (such as Apache Tomcat or Oracle WebLogic Server), varies with the type of `DataSource` object that is being deployed. As a result, most of this section is devoted to showing how a system administrator sets up the environment so that programmers can use a `DataSource` object to get connections.

The following topics are covered:

- [Using DataSource Objects to Get Connections](#datasource_connection)
- [Deploying Basic DataSource Objects](#deploy_datasource)
- [Deploying Other DataSource Implementations](#datasource_implementation)
- [Getting and Using Pooled Connections](#pooled_connection)
- [Deploying Distributed Transactions](#deployment_distributed_transactions)
- [Using Connections for Distributed Transactions](#using_connections_distributed_transactions)

## <a name="datasource_connection" id="datasource_connection">Using DataSource Objects to Get a Connection</a>

In [Establishing a Connection](connecting.html), you learned how to get a connection using the `DriverManager` class. This section shows you how to use a `DataSource` object to get a connection to your data source, which is the preferred way.

Objects instantiated by classes that implement the `DataSource` represent a particular DBMS or some other data source, such as a file. A `DataSource` object represents a particular DBMS or some other data source, such as a file. If a company uses more than one data source, it will deploy a separate `DataSource` object for each of them. The `DataSource` interface is implemented by a driver vendor. It can be implemented in three different ways:

- A basic `DataSource` implementation produces standard `Connection` objects that are not pooled or used in a distributed transaction.
- A `DataSource` implementation that supports connection pooling produces `Connection` objects that participate in connection pooling, that is, connections that can be recycled.
- A `DataSource` implementation that supports distributed transactions produces `Connection` objects that can be used in a distributed transaction, that is, a transaction that accesses two or more DBMS servers.

A JDBC driver should include at least a basic `DataSource` implementation. For example, the Java DB JDBC driver includes the implementation `org.apache.derby.jdbc.ClientDataSource` and for MySQL, `com.mysql.jdbc.jdbc2.optional.MysqlDataSource`. If your client runs on Java 8 compact profile 2, then the Java DB JDBC driver is `org.apache.derby.jdbc.BasicClientDataSource40`. The sample for this tutorial requires compact profile 3 or greater.

A `DataSource` class that supports distributed transactions typically also implements support for connection pooling. For example, a `DataSource` class provided by an EJB vendor almost always supports both connection pooling and distributed transactions.

Suppose that the owner of the thriving chain of The Coffee Break shops, from the previous examples, has decided to expand further by selling coffee over the Internet. With the large amount of online business expected, the owner will definitely need connection pooling. Opening and closing connections involves a great deal of overhead, and the owner anticipates that this online ordering system will necessitate a sizable number of queries and updates. With connection pooling, a pool of connections can be used over and over again, avoiding the expense of creating a new connection for every database access. In addition, the owner now has a second DBMS that contains data for the recently acquired coffee roasting company. This means that the owner will want to be able to write distributed transactions that use both the old DBMS server and the new one.

The chain owner has reconfigured the computer system to serve the new, larger customer base. The owner has purchased the most recent JDBC driver and an EJB application server that works with it to be able to use distributed transactions and get the increased performance that comes with connection pooling. Many JDBC drivers are available that are compatible with the recently purchased EJB server. The owner now has a three-tier architecture, with a new EJB application server and JDBC driver in the middle tier and the two DBMS servers as the third tier. Client computers making requests are the first tier.

## <a name="deploy_datasource" id="deploy_datasource">Deploying Basic DataSource Objects</a>

The system administrator needs to deploy `DataSource` objects so that The Coffee Break's programming team can start using them. Deploying a `DataSource` object consists of three tasks:

1. Creating an instance of the `DataSource` class
1. Setting its properties
1. Registering it with a naming service that uses the Java Naming and Directory Interface (JNDI) API

First, consider the most basic case, which is to use a basic implementation of the `DataSource` interface, that is, one that does not support connection pooling or distributed transactions. In this case there is only one `DataSource` object that needs to be deployed. A basic implementation of `DataSource` produces the same kind of connections that the `DriverManager` class produces.

### Creating Instance of DataSource Class and Setting its Properties

Suppose a company that wants only a basic implementation of `DataSource` has bought a driver from the JDBC vendor DB Access, Inc. This driver includes the class `com.dbaccess.BasicDataSource` that implements the `DataSource` interface. The following code excerpt creates an instance of the class `BasicDataSource` and sets its properties. After the instance of `BasicDataSource` is deployed, a programmer can call the method `DataSource.getConnection` to get a connection to the company's database, `CUSTOMER_ACCOUNTS`. First, the system administrator creates the `BasicDataSource` object `**ds**` using the default constructor. The system administrator then sets three properties. Note that the following code is typically be executed by a deployment tool:

```

com.dbaccess.BasicDataSource ds = new com.dbaccess.BasicDataSource();
ds.setServerName("grinder");
ds.setDatabaseName("CUSTOMER_ACCOUNTS");
ds.setDescription("Customer accounts database for billing");

```

The variable `**ds**` now represents the database `CUSTOMER_ACCOUNTS` installed on the server. Any connection produced by the `BasicDataSource` object `**ds**` will be a connection to the database `CUSTOMER_ACCOUNTS`.

### Registering DataSource Object with Naming Service That Uses JNDI API

With the properties set, the system administrator can register the `BasicDataSource` object with a JNDI (Java Naming and Directory Interface) naming service. The particular naming service that is used is usually determined by a system property, which is not shown here. The following code excerpt registers the `BasicDataSource` object and binds it with the logical name `jdbc/billingDB`:

```

Context ctx = new InitialContext();
ctx.bind("jdbc/billingDB", ds);

```

This code uses the JNDI API. The first line creates an `InitialContext` object, which serves as the starting point for a name, similar to root directory in a file system. The second line associates, or binds, the `BasicDataSource` object `**ds**` to the logical name `jdbc/billingDB`. In the next code excerpt, you give the naming service this logical name, and it returns the `BasicDataSource` object. The logical name can be any string. In this case, the company decided to use the name `billingDB` as the logical name for the `CUSTOMER_ACCOUNTS` database.

In the previous example, `jdbc` is a subcontext under the initial context, just as a directory under the root directory is a subdirectory. The name `jdbc/billingDB` is like a path name, where the last item in the path is analogous to a file name. In this case, `billingDB` is the logical name that is given to the `BasicDataSource` object `**ds**`. The subcontext `jdbc` is reserved for logical names to be bound to `DataSource` objects, so `jdbc` will always be the first part of a logical name for a data source.

### Using Deployed DataSource Object

After a basic `DataSource` implementation is deployed by a system administrator, it is ready for a programmer to use. This means that a programmer can give the logical data source name that was bound to an instance of a `DataSource` class, and the JNDI naming service will return an instance of that `DataSource` class. The method `getConnection` can then be called on that `DataSource` object to get a connection to the data source it represents. For example, a programmer might write the following two lines of code to get a `DataSource` object that produces a connection to the database `CUSTOMER_ACCOUNTS`.

```

Context ctx = new InitialContext();
DataSource ds = (DataSource)ctx.lookup("jdbc/billingDB");

```

The first line of code gets an initial context as the starting point for retrieving a `DataSource` object. When you supply the logical name `jdbc/billingDB` to the method `lookup`, the method returns the `DataSource` object that the system administrator bound to `jdbc/billingDB` at deployment time. Because the return value of the method `lookup` is a Java `Object`, we must cast it to the more specific `DataSource` type before assigning it to the variable `**ds**`.

The variable `**ds**` is an instance of the class `com.dbaccess.BasicDataSource` that implements the `DataSource` interface. Calling the method `**ds**.getConnection` produces a connection to the `CUSTOMER_ACCOUNTS` database.

```

Connection con = ds.getConnection("fernanda","brewed");

```

The `getConnection` method requires only the user name and password because the variable `**ds**` has the rest of the information necessary for establishing a connection with the `CUSTOMER_ACCOUNTS` database, such as the database name and location, in its properties.

### Advantages of DataSource Objects

Because of its properties, a `DataSource` object is a better alternative than the `DriverManager` class for getting a connection. Programmers no longer have to hard code the driver name or JDBC URL in their applications, which makes them more portable. Also, `DataSource` properties make maintaining code much simpler. If there is a change, the system administrator can update data source properties and not be concerned about changing every application that makes a connection to the data source. For example, if the data source were moved to a different server, all the system administrator would have to do is set the `serverName` property to the new server name.

Aside from portability and ease of maintenance, using a `DataSource` object to get connections can offer other advantages. When the `DataSource` interface is implemented to work with a `ConnectionPoolDataSource` implementation, all of the connections produced by instances of that `DataSource` class will automatically be pooled connections. Similarly, when the `DataSource` implementation is implemented to work with an `XADataSource` class, all of the connections it produces will automatically be connections that can be used in a distributed transaction. The next section shows how to deploy these types of `DataSource` implementations.

## <a name="datasource_implementation" id="datasource_implementation">Deploying Other DataSource Implementations</a>

A system administrator or another person working in that capacity can deploy a `DataSource` object so that the connections it produces are pooled connections. To do this, he or she first deploys a `ConnectionPoolDataSource` object and then deploys a `DataSource` object implemented to work with it. The properties of the `ConnectionPoolDataSource` object are set so that it represents the data source to which connections will be produced. After the `ConnectionPoolDataSource` object has been registered with a JNDI naming service, the `DataSource` object is deployed. Generally only two properties must be set for the `DataSource` object: `description` and `dataSourceName`. The value given to the `dataSourceName` property is the logical name identifying the `ConnectionPoolDataSource` object previously deployed, which is the object containing the properties needed to make the connection.

With the `ConnectionPoolDataSource` and `DataSource` objects deployed, you can call the method `DataSource.getConnection` on the `DataSource` object and get a pooled connection. This connection will be to the data source specified in the `ConnectionPoolDataSource` object's properties.

The following example describes how a system administrator for The Coffee Break would deploy a `DataSource` object implemented to provide pooled connections. The system administrator would typically use a deployment tool, so the code fragments shown in this section are the code that a deployment tool would execute.

To get better performance, The Coffee Break company has bought a JDBC driver from DB Access, Inc. that includes the class `com.dbaccess.ConnectionPoolDS`, which implements the `ConnectionPoolDataSource` interface. The system administrator creates create an instance of this class, sets its properties, and registers it with a JNDI naming service. The Coffee Break has bought its `DataSource` class, `com.applogic.PooledDataSource`, from its EJB server vendor, Application Logic, Inc. The class `com.applogic.PooledDataSource` implements connection pooling by using the underlying support provided by the `ConnectionPoolDataSource` class `com.dbaccess.ConnectionPoolDS`.

The `ConnectionPoolDataSource` object must be deployed first. The following code creates an instance of `com.dbaccess.ConnectionPoolDS` and sets its properties:

```

com.dbaccess.ConnectionPoolDS cpds = new com.dbaccess.ConnectionPoolDS();
cpds.setServerName("creamer");
cpds.setDatabaseName("COFFEEBREAK");
cpds.setPortNumber(9040);
cpds.setDescription("Connection pooling for " + "COFFEEBREAK DBMS");

```

After the `ConnectionPoolDataSource` object has been deployed, the system administrator deploys the `DataSource` object. The following code registers the `com.dbaccess.ConnectionPoolDS` object `**cpds**` with a JNDI naming service. Note that the logical name being associated with the `**cpds**` variable has the subcontext `pool` added under the subcontext `jdbc`, which is similar to adding a subdirectory to another subdirectory in a hierarchical file system. The logical name of any instance of the class `com.dbaccess.ConnectionPoolDS` will always begin with `jdbc/pool`. Oracle recommends putting all `ConnectionPoolDataSource` objects under the subcontext `jdbc/pool`:

```

Context ctx = new InitialContext();
ctx.bind("jdbc/pool/fastCoffeeDB", cpds);

```

Next, the `DataSource` class that is implemented to interact with the `**cpds**` variable and other instances of the `com.dbaccess.ConnectionPoolDS` class is deployed. The following code creates an instance of this class and sets its properties. Note that only two properties are set for this instance of `com.applogic.PooledDataSource`. The `description` property is set because it is always required. The other property that is set, `dataSourceName`, gives the logical JNDI name for `**cpds**`, which is an instance of the `com.dbaccess.ConnectionPoolDS` class. In other words, `**cpds**` represents the `ConnectionPoolDataSource` object that will implement connection pooling for the `DataSource` object.

The following code, which would probably be executed by a deployment tool, creates a `PooledDataSource` object, sets its properties, and binds it to the logical name `jdbc/fastCoffeeDB`:

```

com.applogic.PooledDataSource ds = new com.applogic.PooledDataSource();
ds.setDescription("produces pooled connections to COFFEEBREAK");
ds.setDataSourceName("jdbc/pool/fastCoffeeDB");
Context ctx = new InitialContext();
ctx.bind("jdbc/fastCoffeeDB", ds);

```

At this point, a `DataSource` object is deployed from which an application can get pooled connections to the database `COFFEEBREAK`.

## <a name="pooled_connection" id="pooled_connection">Getting and Using Pooled Connections</a>

A **connection pool** is a cache of database connection objects. The objects represent physical database connections that can be used by an application to connect to a database. At run time, the application requests a connection from the pool. If the pool contains a connection that can satisfy the request, it returns the connection to the application. If no connections are found, a new connection is created and returned to the application. The application uses the connection to perform some work on the database and then returns the object back to the pool. The connection is then available for the next connection request.

Connection pools promote the reuse of connection objects and reduce the number of times that connection objects are created. Connection pools significantly improve performance for database-intensive applications because creating connection objects is costly both in terms of time and resources.

Now that these `DataSource` and `ConnectionPoolDataSource` objects are deployed, a programmer can use the `DataSource` object to get a pooled connection. The code for getting a pooled connection is just like the code for getting a nonpooled connection, as shown in the following two lines:

```

ctx = new InitialContext();
ds = (DataSource)ctx.lookup("jdbc/fastCoffeeDB");

```

The variable `**ds**` represents a `DataSource` object that produces pooled connections to the database `COFFEEBREAK`. You need to retrieve this `DataSource` object only once because you can use it to produce as many pooled connections as needed. Calling the method `getConnection` on the `**ds**` variable automatically produces a pooled connection because the `DataSource` object that the `**ds**` variable represents was configured to produce pooled connections.

Connection pooling is generally transparent to the programmer. There are only two things you need to do when you are using pooled connections:

<li>
Use a `DataSource` object rather than the `DriverManager` class to get a connection. In the following line of code, `**ds**` is a `DataSource` object implemented and deployed so that it will create pooled connections and `username` and `password` are variables that represent the credentials of the user that has access to the database:
<pre><code>
Connection con = ds.getConnection(username, password);
</code></pre>
</li>
<li>
Use a `finally` statement to close a pooled connection. The following `finally` block would appear after the `try/catch` block that applies to the code in which the pooled connection was used:
<pre><code>
try {
    Connection con = ds.getConnection(username, password);
    // ... code to use the pooled
    // connection con
} catch (Exception ex {
    // ... code to handle exceptions
} finally {
    if (con != null) con.close();
}
</code></pre>
</li>

Otherwise, an application using a pooled connection is identical to an application using a regular connection. The only other thing an application programmer might notice when connection pooling is being done is that performance is better.

The following sample code gets a `DataSource` object that produces connections to the database `COFFEEBREAK` and uses it to update a price in the table `COFFEES`:

```

import java.sql.*;
import javax.sql.*;
import javax.ejb.*;
import javax.naming.*;

public class ConnectionPoolingBean implements SessionBean {

    // ...

    public void ejbCreate() throws CreateException {
        ctx = new InitialContext();
        ds = (DataSource)ctx.lookup("jdbc/fastCoffeeDB");
    }

    public void updatePrice(float price, String cofName,
                            String username, String password)
        throws SQLException{

        Connection con;
        PreparedStatement pstmt;
        try {
            con = ds.getConnection(username, password);
            con.setAutoCommit(false);
            pstmt = con.prepareStatement("UPDATE COFFEES " +
                        "SET PRICE = ? " +
                        "WHERE COF_NAME = ?");
            pstmt.setFloat(1, price);
            pstmt.setString(2, cofName);
            pstmt.executeUpdate();

            con.commit();
            pstmt.close();

        } finally {
            if (con != null) con.close();
        }
    }

    private DataSource ds = null;
    private Context ctx = null;
}


```

The connection in this code sample participates in connection pooling because the following are true:

- An instance of a class implementing `ConnectionPoolDataSource` has been deployed.
- An instance of a class implementing `DataSource` has been deployed, and the value set for its `dataSourceName` property is the logical name that was bound to the previously deployed `ConnectionPoolDataSource` object.

Note that although this code is very similar to code you have seen before, it is different in the following ways:

<li>
It imports the `javax.sql`, `javax.ejb`, and `javax.naming` packages in addition to `java.sql`.
The `DataSource` and `ConnectionPoolDataSource` interfaces are in the `javax.sql` package, and the JNDI constructor `InitialContext` and method `Context.lookup` are part of the `javax.naming` package. This particular example code is in the form of an EJB component that uses API from the `javax.ejb` package. The purpose of this example is to show that you use a pooled connection the same way you use a nonpooled connection, so you need not worry about understanding the EJB API.
</li>
<li>
It uses a `DataSource` object to get a connection instead of using the `DriverManager` facility.
</li>
<li>
It uses a `finally` block to ensure that the connection is closed.
</li>

Getting and using a pooled connection is similar to getting and using a regular connection. When someone acting as a system administrator has deployed a `ConnectionPoolDataSource` object and a `DataSource` object properly, an application uses that `DataSource` object to get a pooled connection. An application should, however, use a `finally` block to close the pooled connection. For simplicity, the preceding example used a `finally` block but no `catch` block. If an exception is thrown by a method in the `try` block, it will be thrown by default, and the `finally` clause will be executed in any case.

## <a name="deployment_distributed_transactions" id="deployment_distributed_transactions">Deploying Distributed Transactions</a>

`DataSource` objects can be deployed to get connections that can be used in distributed transactions. As with connection pooling, two different class instances must be deployed: an `XADataSource` object and a `DataSource` object that is implemented to work with it.

Suppose that the EJB server that The Coffee Break entrepreneur bought includes the `DataSource` class `com.applogic.TransactionalDS`, which works with an `XADataSource` class such as `com.dbaccess.XATransactionalDS`. The fact that it works with any `XADataSource` class makes the EJB server portable across JDBC drivers. When the `DataSource` and `XADataSource` objects are deployed, the connections produced will be able to participate in distributed transactions. In this case, the class `com.applogic.TransactionalDS` is implemented so that the connections produced are also pooled connections, which will usually be the case for `DataSource` classes provided as part of an EJB server implementation.

The `XADataSource` object must be deployed first. The following code creates an instance of `com.dbaccess.XATransactionalDS` and sets its properties:

```

com.dbaccess.XATransactionalDS xads = new com.dbaccess.XATransactionalDS();
xads.setServerName("creamer");
xads.setDatabaseName("COFFEEBREAK");
xads.setPortNumber(9040);
xads.setDescription("Distributed transactions for COFFEEBREAK DBMS");

```

The following code registers the `com.dbaccess.XATransactionalDS` object `**xads**` with a JNDI naming service. Note that the logical name being associated with `**xads**` has the subcontext `xa` added under `jdbc`. Oracle recommends that the logical name of any instance of the class `com.dbaccess.XATransactionalDS` always begin with `jdbc/xa`.

```

Context ctx = new InitialContext();
ctx.bind("jdbc/xa/distCoffeeDB", xads);

```

Next, the `DataSource` object that is implemented to interact with `**xads**` and other `XADataSource` objects is deployed. Note that the `DataSource` class, `com.applogic.TransactionalDS`, can work with an `XADataSource` class from any JDBC driver vendor. Deploying the `DataSource` object involves creating an instance of the `com.applogic.TransactionalDS` class and setting its properties. The `dataSourceName` property is set to `jdbc/xa/distCoffeeDB`, the logical name associated with `com.dbaccess.XATransactionalDS`. This is the `XADataSource` class that implements the distributed transaction capability for the `DataSource` class. The following code deploys an instance of the `DataSource` class:

```

com.applogic.TransactionalDS ds = new com.applogic.TransactionalDS();
ds.setDescription("Produces distributed transaction " +
                  "connections to COFFEEBREAK");
ds.setDataSourceName("jdbc/xa/distCoffeeDB");
Context ctx = new InitialContext();
ctx.bind("jdbc/distCoffeeDB", ds);

```

Now that instances of the classes `com.applogic.TransactionalDS` and `com.dbaccess.XATransactionalDS` have been deployed, an application can call the method `getConnection` on instances of the `TransactionalDS` class to get a connection to the `COFFEEBREAK` database that can be used in distributed transactions.

## <a name="using_connections_distributed_transactions" id="using_connections_distributed_transactions">Using Connections for Distributed Transactions</a>

To get a connection that can be used for distributed transactions, must use a `DataSource` object that has been properly implemented and deployed, as shown in the section [Deploying Distributed Transactions](#deployment_distributed_transactions). With such a `DataSource` object, call the method `getConnection` on it. After you have the connection, use it just as you would use any other connection. Because `jdbc/distCoffeesDB` has been associated with an `XADataSource` object in a JNDI naming service, the following code produces a `Connection` object that can be used in distributed transactions:

```

Context ctx = new InitialContext();
DataSource ds = (DataSource)ctx.lookup("jdbc/distCoffeesDB");
Connection con = ds.getConnection();

```

There are some minor but important restrictions on how this connection is used while it is part of a distributed transaction. A transaction manager controls when a distributed transaction begins and when it is committed or rolled back; therefore, application code should never call the methods `Connection.commit` or `Connection.rollback`. An application should likewise never call `Connection.setAutoCommit(true)`, which enables the auto-commit mode, because that would also interfere with the transaction manager's control of the transaction boundaries. This explains why a new connection that is created in the scope of a distributed transaction has its auto-commit mode disabled by default. Note that these restrictions apply only when a connection is participating in a distributed transaction; there are no restrictions while the connection is not part of a distributed transaction.

For the following example, suppose that an order of coffee has been shipped, which triggers updates to two tables that reside on different DBMS servers. The first table is a new `INVENTORY` table, and the second is the `COFFEES` table. Because these tables are on different DBMS servers, a transaction that involves both of them will be a distributed transaction. The code in the following example, which obtains a connection, updates the `COFFEES` table, and closes the connection, is the second part of a distributed transaction.

Note that the code does not explicitly commit or roll back the updates because the scope of the distributed transaction is being controlled by the middle tier server's underlying system infrastructure. Also, assuming that the connection used for the distributed transaction is a pooled connection, the application uses a `finally` block to close the connection. This guarantees that a valid connection will be closed even if an exception is thrown, thereby ensuring that the connection is returned to the connection pool to be recycled.

The following code sample illustrates an enterprise Bean, which is a class that implements the methods that can be called by a client computer. The purpose of this example is to demonstrate that application code for a distributed transaction is no different from other code except that it does not call the `Connection` methods `commit`, `rollback`, or `setAutoCommit(true)`. Therefore, you do not need to worry about understanding the EJB API that is used.

```

import java.sql.*;
import javax.sql.*;
import javax.ejb.*;
import javax.naming.*;

public class DistributedTransactionBean implements SessionBean {

    // ...

    public void ejbCreate() throws CreateException {

        ctx = new InitialContext();
        ds = (DataSource)ctx.lookup("jdbc/distCoffeesDB");
    }

    public void updateTotal(int incr, String cofName, String username,
                            String password)
        throws SQLException {

        Connection con;
        PreparedStatement pstmt;

        try {
            con = ds.getConnection(username, password);
            pstmt = con.prepareStatement("UPDATE COFFEES " +
                        "SET TOTAL = TOTAL + ? " +
                        "WHERE COF_NAME = ?");
            pstmt.setInt(1, incr);
            pstmt.setString(2, cofName);
            pstmt.executeUpdate();
            stmt.close();
        } finally {
            if (con != null) con.close();
        }
    }

    private DataSource ds = null;
    private Context ctx = null;
}

```
