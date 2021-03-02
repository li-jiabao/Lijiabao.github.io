
# Using Customized Type Mappings

### Using Custom Mapping

**Note**: MySQL currently does not support user-defined types. MySQL and Java DB currently do not support structured types, or the `DISTINCT` SQL data type. No JDBC tutorial example is available to demonstrate the features described in this section.

With business booming, the owner of The Coffee Break is regularly adding new stores and making changes to the database. The owner has decided to use a custom mapping for the structured type `ADDRESS`. This enables the owner to make changes to the Java class that maps the `ADDRESS` type. The Java class will have a field for each attribute of `ADDRESS`. The name of the class and the names of its fields can be any valid Java identifier.

The following topics are covered:

- [Implementing SQLData](#implementing_sqldata)
- [Using a Connection's Type Map](#using_connection_type_map)
- [Using Your Own Type Map](#using_your_own_type_map)

## <a name="implementing_sqldata" id="implementing_sqldata">Implementing SQLData</a>

The first thing required for a custom mapping is to create a class that implements the interface `SQLData`.

The SQL definition of the structured type `ADDRESS` looks like this:

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

A class that implements the `SQLData` interface for the custom mapping of the `ADDRESS` type might look like this:

```

public class Address implements SQLData {
    public int num;
    public String street;
    public String city;
    public String state;
    public String zip;
    private String sql_type;
    
    public String getSQLTypeName() {
        return sql_type;
    }

    public void readSQL(SQLInput stream, String type)
        throws SQLException {
        sql_type = type;
        num = stream.readInt();
        street = stream.readString();
        city = stream.readString();
        state = stream.readString();
        zip = stream.readString();
    }

    public void writeSQL(SQLOutput stream)
        throws SQLException {
        stream.writeInt(num);
        stream.writeString(street);
        stream.writeString(city);
        stream.writeString(state);
        stream.writeString(zip);
    }
}

```

## <a name="using_connection_type_map" id="using_connection_type_map">Using a Connection's Type Map</a>

After writing a class that implements the interface `SQLData`, the only other thing you have to do to set up a custom mapping is to make an entry in a type map. For the example, this means entering the fully qualified SQL name for the `ADDRESS` type and the `Class` object for the class `Address`. A type map, an instance of the `java.util.Map` interface, is associated with every new connection when it is created, so you use that one. Assuming that `con` is the active connection, the following code fragment adds an entry for the UDT `ADDRESS` to the type map associated with `con`.

```

java.util.Map map = con.getTypeMap();
map.put("SchemaName.ADDRESS", Class.forName("Address"));
con.setTypeMap(map);

```

Whenever you call the `getObject` method to retrieve an instance of the `ADDRESS` type, the driver will check the type map associated with the connection and see that it has an entry for `ADDRESS`. The driver will note the `Class` object for the `Address` class, create an instance of it, and do many other things in the background to map `ADDRESS` to `Address`. You do not have to do anything more than generate the class for the mapping and then make an entry in a type map to let the driver know that there is a custom mapping. The driver will do all the rest.

The situation is similar for storing a structured type that has a custom mapping. When you call the method `setObject`, the driver will check to see if the value to be set is an instance of a class that implements the interface `SQLData`. If it is (meaning that there is a custom mapping), the driver will use the custom mapping to convert the value to its SQL counterpart before returning it to the database. Again, the driver does the custom mapping behind the scenes; all you need to do is supply the method `setObject` with a parameter that has a custom mapping. You will see an example of this later in this section.

Look at the difference between working with the standard mapping, a `Struct` object, and the custom mapping, a class in the Java programming language. The following code fragment shows the standard mapping to a `Struct` object, which is the mapping the driver uses when there is no entry in the connection's type map.

```

ResultSet rs = stmt.executeQuery(
    "SELECT LOCATION " +
    "WHERE STORE_NO = 100003");
rs.next();
Struct address = (Struct)rs.getObject("LOCATION");

```

The variable `address` contains the following attribute values: `4344`, `"First_Street"`, `"Verona"`, `"CA"`, `"94545"`.

The following code fragment shows what happens when there is an entry for the structured type `ADDRESS` in the connection's type map. Remember that the column `LOCATION` stores values of type `ADDRESS`.

```

ResultSet rs = stmt.executeQuery(
    "SELECT LOCATION " +
    "WHERE STORE_NO = 100003");
rs.next();
Address store_3 = (Address)rs.getObject("LOCATION");

```

The variable `store_3` is now an instance of the class `Address`, with each attribute value being the current value of one of the fields of `Address`. Note that you must remember to convert the object retrieved by the `getObject` method to an `Address` object before assigning it to `store_3`. Note also that `store_3` must be an `Address` object.

Compare working with the `Struct` object to working with the instance of the `Address` class. Suppose the store moved to a better location in the neighboring town and therefore you must update the database. With the custom mapping, reset the fields of `store_3`, as in the following code fragment:

```

ResultSet rs = stmt.executeQuery(
    "SELECT LOCATION " +
    "WHERE STORE_NO = 100003");
rs.next();
Address store_3 = (Address)rs.getObject("LOCATION");
store_3.num = 1800;
store_3.street = "Artsy_Alley";
store_3.city = "Arden";
store_3.state = "CA";
store_3.zip = "94546";
PreparedStatement pstmt = con.prepareStatement(
    "UPDATE STORES " +
    "SET LOCATION = ? " +
    "WHERE STORE_NO = 100003");
pstmt.setObject(1, store_3);
pstmt.executeUpdate();

```

Values in the column `LOCATION` are instances of the `ADDRESS` type. The driver checks the connection's type map and sees that there is an entry linking `ADDRESS` with the class `Address` and consequently uses the custom mapping indicated in `Address`. When the code calls the method `setObject` with the variable `**store_3**` as the second parameter, the driver checks and sees that `**store_3**` represents an instance of the class `Address`, which implements the interface `SQLData` for the structured type `ADDRESS`, and again automatically uses the custom mapping.

Without a custom mapping for `ADDRESS`, the update would look more like this:

```

PreparedStatement pstmt = con.prepareStatement(
    "UPDATE STORES " +
    "SET LOCATION.NUM = 1800, " +
    "LOCATION.STREET = 'Artsy_Alley', " + 
    "LOCATION.CITY = 'Arden', " +
    "LOCATION.STATE = 'CA', " +
    "LOCATION.ZIP = '94546' " +
    "WHERE STORE_NO = 100003");
pstmt.executeUpdate;

```

## <a name="using_your_own_type_map" id="using_your_own_type_map">Using Your Own Type Map</a>

Up to this point, you have used only the type map associated with a connection for custom mapping. Ordinarily, that is the only type map most programmers will use. However, it is also possible to create a type map and pass it to certain methods so that the driver will use that type map instead of the one associated with the connection. This allows two different mappings for the same user-defined type (UDT). In fact, it is possible to have multiple custom mappings for the same UDT, just as long as each mapping is set up with a class implementing the `SQLData` interface and an entry in a type map. If you do not pass a type map to a method that can accept one, the driver will by default use the type map associated with the connection.

There are very few situations that call for using a type map other than the one associated with a connection. It could be necessary to supply a method with a type map if, for instance, several programmers working on a JDBC application brought their components together and were using the same connection. If two or more programmers had created their own custom mappings for the same SQL UDT, each would need to supply his or her own type map, thus overriding the connection's type map.
