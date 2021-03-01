
# Result Count

Sometimes, a query might produce too many answers and you want to limit the number of answers returned. You can do this by using the count limit search control. By default, a search does not have a count limit--it will return all answers that it finds. To set the count limit of a search, pass the number to 
[<tt>SearchControls.setCountLimit()</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setCountLimit-long-).


[`The following example`](examples/SearchCountLimit.java) sets the count limit to 1.

```

// Set the search controls to limit the count to 1
SearchControls ctls = new SearchControls();
ctls.setCountLimit(1);

```

If the program attempts to get more than the count limit number of results, then a 
[<tt>SizeLimitExceededException</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/SizeLimitExceededException.html) will be thrown. So if a program has set a count limit, then it should either differentiate this exception from other 
[<tt>NamingException</tt>s](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html) or keep track of the count limit and not request more than that number of results.

Specifying a count limit for a search is one way of controlling the resources (such as memory and network bandwidth) that your application consumes. Other ways to control the resources consumed are to narrow your 
[search filter](filter.html) (be more specific about what you seek), start your search in the appropriate context, and use the appropriate 
[scope](scope.html).
