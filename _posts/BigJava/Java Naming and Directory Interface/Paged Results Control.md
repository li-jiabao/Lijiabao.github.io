
# Sort Control

The sort control is used when a client wants the server to send the sorted search results. The server-side sorting is useful in a situation where the client needs to sort the results according to some criteria but is not equipped to perform the sort process on its own. The sort control is specified in 
[RFC 2891](http://www.ietf.org/rfc/rfc2891.txt). The classes below provide the functionality required to support sort control.

<li>
[<tt>javax.naming.ldap.SortControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortControl.html)</li>
<li>
[<tt>javax.naming.ldap.SortKey</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortKey.html)</li>
<li>
[<tt>javax.naming.ldap.SortResponseControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortResponseControl.html)</li>

The SortKey is an odered list of keys based upon which the server sorts the result.

## How to use Sort Control?

The example below illustrates the client-server interaction between a client performing a search requesting a server-side sorting based on the attribute "cn".

<li>Client sends a search request asking for
<pre><code>
   
    // Activate sorting
     String sortKey = "cn";
     ctx.setRequestControls(new Control[] { 
         new SortControl(sortKey, Control.CRITICAL) });

     // Perform a search
     NamingEnumeration results = 
         ctx.search("", "(objectclass=*)", new SearchControls());

</code></pre>
</li>
<li>The server responds with entries that are sorted based on the "cn" attribute and its corresponding matching rule.
<pre><code>
   
    // Iterate over sorted search results
     while (results != null &amp;&amp; results.hasMore()) {
         // Display an entry
         SearchResult entry = (SearchResult)results.next();
         System.out.println(entry.getName());

         // Handle the entry's response controls (if any)
         if (entry instanceof HasControls) {
             // ((HasControls)entry).getControls();
         }
     }
     // Examine the sort control response 
     Control[] controls = ctx.getResponseControls();
     if (controls != null) {
         for (int i = 0; i &lt; controls.length; i++) {
             if (controls[i] instanceof SortResponseControl) {
                 SortResponseControl src = (SortResponseControl)controls[i];
                 if (! src.isSorted()) {
                     throw src.getException();
                 }
             } else {
                 // Handle other response controls (if any)
             }
         }
     }  
</code></pre>    
</li>

The complete JNDI example can be found 
[`here`](examples/SortedResults.java).

**Note:** The sort control is supported by both Oracle Directory Server and the Windows Active Directory server.
