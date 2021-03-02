
# Lesson: JDBC Basics

In this lesson you will learn the basics of the JDBC API.

<li>
[Getting Started](gettingstarted.html) sets up a basic database development environment and shows you how to compile and run the JDBC tutorial samples.
</li>
<li>
[Processing SQL Statements with JDBC](processingsqlstatements.html) outlines the steps required to process any SQL statement. The pages that follow describe these steps in more detail:
<ul>
<li>
[Establishing a Connection](connecting.html) connects you to your database.
</li>
<li>
[Connecting with DataSource Objects](sqldatasources.html) shows you how to connect to your database with `DataSource` objects, the preferred way of getting a connection to a data source.
</li>
<li>
[Handling SQLExceptions](sqlexception.html) shows you how to handle exceptions caused by database errors.
</li>
<li>
[Setting Up Tables](tables.html) describes all the database tables used in the JDBC tutorial samples and how to create and populate tables with JDBC API and SQL scripts.
</li>
<li>
[Retrieving and Modifying Values from Result Sets](retrieving.html) develop the process of configuring your database, sending queries, and retrieving data from your database.
</li>
<li>
[Using Prepared Statements](prepared.html) describes a more flexible way to create database queries.
</li>
<li>
[Using Transactions](transactions.html) shows you how to control when a database query is actually executed.
</li>

[Using RowSet Objects](rowset.html) introduces you to `RowSet` objects; these are objects that hold tabular data in a way that make it more flexible and easier to use than result sets. The pages that follow describe the different kinds of `RowSet` objects available:

<li>
[Using JdbcRowSet Objects](jdbcrowset.html)
</li>
<li>
[Using CachedRowSetObjets](cachedrowset.html)
</li>
<li>
[Using JoinRowSet Objects](joinrowset.html)
</li>
<li>
[Using FilteredRowSet Objects](filteredrowset.html)
</li>
<li>
[Using WebRowSet Objects](webrowset.html)
</li>

[Using Advanced Data Types](sqltypes.html) introduces you to other data types; the pages that follow describe these data types in further detail:

<li>
[Using Large Objects](blob.html)
</li>
<li>
[Using SQLXML Objects](sqlxml.html)
</li>
<li>
[Using Array Objects](array.html)
</li>
<li>
[Using DISTINCT Data Type](distinct.html)
</li>
<li>
[Using Structured Objects](sqlstructured.html)
</li>
<li>
[Using Customized Type Mappings](sqlcustommapping.html)
</li>
<li>
[Using Datalink Objects](sqldatalink.html)
</li>
<li>
[Using RowId Objects](sqlrowid.html)
</li>

[Using Stored Procedures](storedprocedures.html) shows you how to create and use a stored procedure, which is a group of SQL statements that can be called like a Java method with variable input and output parameters.

[Using JDBC with GUI API](jdbcswing.html) demonstrates how to integrate JDBC with the Swing API.
