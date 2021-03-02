
# Class Literals as Runtime-Type Tokens

One of the changes in JDK 5.0 is that the class `java.lang.Class` is generic. It's an interesting example of using genericity for something other than a container class.

Now that `Class` has a type parameter `T`, you might well ask, what does `T` stand for? It stands for the type that the `Class` object is representing.

For example, the type of `String.class` is `Class&lt;String&gt;`, and the type of `Serializable.class` is `Class&lt;Serializable&gt;`. This can be used to improve the type safety of your reflection code.

In particular, since the `newInstance()` method in `Class` now returns a `T`, you can get more precise types when creating objects reflectively.

For example, suppose you need to write a utility method that performs a database query, given as a string of SQL, and returns a collection of objects in the database that match that query.

One way is to pass in a factory object explicitly, writing code like:

```

**interface** Factory&lt;T&gt; { T make();} 

**public** &lt;T&gt; Collection&lt;T&gt; select(Factory&lt;T&gt; factory, String statement) { 
    Collection&lt;T&gt; result = new ArrayList&lt;T&gt;(); 

    /* *Run sql query using jdbc* */  
    **for** (/* *Iterate over jdbc results.* */) { 
        T item = factory.make();
        /* <i>Use reflection and set all of item's 
         * fields from sql results.</i> 
         */ 
        result.add(item); 
    } 
    **return** result; 
}

```

You can call this either as

```

select(new Factory&lt;EmpInfo&gt;(){ 
    **public** EmpInfo make() {
        **return** new EmpInfo();
    }}, "selection string");

```

or you can declare a class `EmpInfoFactory` to support the `Factory` interface

```

**class** EmpInfoFactory **implements** Factory&lt;EmpInfo&gt; {
    ...
    **public** EmpInfo make() { 
        **return** new EmpInfo();
    }
}

```

and call it

```

select(getMyEmpInfoFactory(), "selection string");

```

The downside of this solution is that it requires either:

- the use of verbose anonymous factory classes at the call site, or
- declaring a factory class for every type used and passing a factory instance at the call site, which is somewhat unnatural.

It is natural to use the class literal as a factory object, which can then be used by reflection. Today (without generics) the code might be written:

```

Collection emps = sqlUtility.select(EmpInfo.class, "select * from emps");
...
**public static** Collection select(Class c, String sqlStatement) { 
    Collection result = new ArrayList();
    /* *Run sql query using jdbc.* */
    **for** (/* *Iterate over jdbc results.* */ ) { 
        Object item = c.newInstance(); 
        /* <i>Use reflection and set all of item's
         * fields from sql results.</i> 
         */  
        result.add(item); 
    } 
    **return** result; 
}

```

However, this would not give us a collection of the precise type we desire. Now that `Class` is generic, we can instead write the following:

```

Collection&lt;EmpInfo&gt; 
    emps = sqlUtility.select(EmpInfo.class, "select * from emps");
...
**public static** &lt;T&gt; Collection&lt;T&gt; select(Class&lt;T&gt; c, String sqlStatement) { 
    Collection&lt;T&gt; result = new ArrayList&lt;T&gt;();
    /* *Run sql query using jdbc.* */
    **for** (/* *Iterate over jdbc results.* */ ) { 
        T item = c.newInstance(); 
        /* <i>Use reflection and set all of item's
         * fields from sql results.</i> 
         */  
        result.add(item);
    } 
    **return** result; 
} 

```

The above code gives us the precise type of collection in a type safe way.

This technique of using class literals as run time type tokens is a very useful trick to know. It's an idiom that's used extensively in the new APIs for manipulating annotations, for example.
