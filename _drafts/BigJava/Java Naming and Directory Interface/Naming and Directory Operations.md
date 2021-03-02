
# Naming Exceptions

Many methods in the JNDI packages throw a 
[<tt>NamingException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html) when they need to indicate that the operation requested cannot be performed. Commonly, you will see a <tt>try/catch</tt> wrapper around the methods that can throw a <tt>NamingException</tt>:

```

try {
    Context ctx = new InitialContext();
    Object obj = ctx.lookup("somename");
} catch (NamingException e) {
    // Handle the error
    System.err.println(e);
}

```

## Exception Class Hierarchy

The JNDI has a rich exception hierarchy stemming from the <tt>NamingException</tt> class. The class names of the exceptions are self-explanatory and are listed 
[here](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-tree.html).

To handle a particular subclass of <tt>NamingException</tt> specially, you catch the subclass separately. For example, the following code specially treats the <tt>AuthenticationException</tt> and its subclasses.

```

try {
    Context ctx = new InitialContext();
    Object obj = ctx.lookup("somename");
} catch (AuthenticationException e) {
    // attempt to reacquire the authentication information
    ...
} catch (NamingException e) {
    // Handle the error
    System.err.println(e);
}

```

## Enumerations

Operations such as 
[<tt>Context.list()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#list-javax.naming.Name-) and 
[<tt>DirContext.search()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-) return a 
[<tt>NamingEnumeration</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html). In these cases, if an error occurs and no results are returned, then <tt>NamingException</tt> or one of its appropriate subclasses will be thrown at the time that the method is invoked. If an error occurs but there are some results to be returned, then a <tt>NamingEnumeration</tt> is returned so that you can get those results. When all of the results are exhausted, invoking 
[<tt>NamingEnumeration.hasMore()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html#hasMore--) will cause a <tt>NamingException</tt> (or one of its subclasses) to be thrown to indicate the error. At that point, the enumeration becomes invalid and no more methods should be invoked on it.

For example, if you perform a <tt>search()</tt> and specify a count limit (**n**) of how many answers to return, then the <tt>search()</tt> will return an enumeration consisting of at most **n** results. If the number of results exceeds **n**, then when <tt>NamingEnumeration.hasMore()</tt> is invoked for the **n+1** time, a <tt>SizeLimitExceededException</tt> will be thrown. See the 
[Result Count](countlimit.html) of this lesson for a sample code.

## Examples in This Tutorial

In the inline sample code that is embedded within the text of this tutorial, the <tt>try/catch</tt> clauses are usually omitted for the sake of readability. Typically, because only code fragments are shown here, only the lines that are directly useful in illustrating a concept are included. You will see appropriate placements of the <tt>try/catch</tt> clauses for <tt>NamingException</tt> if you look in the source files that accompany this tutorial.

The Exceptions in the javax.naming package can be found 
[here](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-summary.html).
