
# Using DISTINCT Data Type

**Note**: MySQL and Java DB currently do not support the `DISTINCT` SQL data type. Consequently, no JDBC tutorial example is available to demonstrate the features described in this section.

The `DISTINCT` data type behaves differently from the other advanced SQL data types. Being a user-defined type that is based on one of the already existing built-in types, it has no interface as its mapping in the Java programming language. Instead, the standard mapping for a `DISTINCT` data type is the Java type to which its underlying SQL data type maps.

To illustrate, create a `DISTINCT` data type and then see how to retrieve, set, or update it. Suppose you always use a two-letter abbreviation for a state and want to create a `DISTINCT` data type to be used for these abbreviations. You could define your new `DISTINCT` data type with the following SQL statement:

```

CREATE TYPE STATE AS CHAR(2);

```

Some databases use an alternate syntax for creating a `DISTINCT` data type, which is shown in the following line of code:

```

CREATE DISTINCT TYPE STATE AS CHAR(2);

```

If one syntax does not work, you can try the other. Alternatively, you can check the documentation for your driver to see the exact syntax it expects.

These statements create a new data type, `STATE`, which can be used as a column value or as the value for an attribute of a SQL structured type. Because a value of type `STATE` is in reality a value that is two `CHAR` types, you use the same method to retrieve it that you would use to retrieve a `CHAR` value, that is, `getString`. For example, assuming that the fourth column of `ResultSet **rs**` stores values of type `STATE`, the following line of code retrieves its value:

```

String state = rs.getString(4);

```

Similarly, you would use the method `setString` to store a `STATE` value in the database and the method `updateString` to modify its value.
