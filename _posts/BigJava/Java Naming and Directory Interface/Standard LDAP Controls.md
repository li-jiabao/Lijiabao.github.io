
# Paged Results Control

## BasicControl

The 
[<tt>javax.naming.ldap.BasicControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/BasicControl.html) which implements the <tt>javax.naming.ldap.Control</tt> serves as a base implementation for extending other controls.

## Paged Results Control

The paged results control is useful for LDAP clients which want to receive search results in a controlled manner limited by the page size. The page size can be configured by the client as per the availability of its resources, like bandwidth and the processing capability.

The server uses a cookie (similar to the HTTP session cookie mechanism) to maintain the state of the search requests in order to track the results being sent to the client. The paged results control is specified in 
[RFC 2696](http://www.ietf.org/rfc/rfc2696.txt)
. The classes below provide the functionality required to support paged results control.

<li>
[<tt>javax.naming.ldap.PagedResultsControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/PagedResultsControl.html)</li>
<li>
[<tt>javax.naming.ldap.PagedResultsResponseControl</tt>](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/PagedResultsResponseControl.html)</li>

### How to use Paged Results Control?

The example below illustrates the client-server interaction between a client doing a search requesting a page size limit of 5. The entire result set returned by the server contains 21 entries.

<li>Client sends a search request asking for paged results with a page size of 5.
<pre><code>
    // Activate paged results
     int pageSize = 5; // 5 entries per page
     byte[] cookie = null;
     int total;
     ctx.setRequestControls(new Control[]{ 
         new PagedResultsControl(pageSize, Control.CRITICAL) });
     // Perform the search
     NamingEnumeration results = ctx.search("", "(objectclass=*)", 
                                            new SearchControls());
</code></pre>
</li>
<li>The server responds with entries plus an indication of 21 total entries in the search result and an opaque cookie to be used by the client when retrieving subsequent pages.
<pre><code>
   // Iterate over a batch of search results sent by the server
         while (results != null &amp;&amp; results.hasMore()) {
             // Display an entry
             SearchResult entry = (SearchResult)results.next();
             System.out.println(entry.getName());

             // Handle the entry's response controls (if any)
             if (entry instanceof HasControls) {
                 // ((HasControls)entry).getControls();
             }
         }
   // Examine the paged results control response 
         Control[] controls = ctx.getResponseControls();
         if (controls != null) {
             for (int i = 0; i &lt; controls.length; i++) {
                 if (controls[i] instanceof PagedResultsResponseControl) {
                     PagedResultsResponseControl prrc =
                         (PagedResultsResponseControl)controls[i];
                     total = prrc.getResultSize();
                     cookie = prrc.getCookie();
                 } else {
                     // Handle other response controls (if any)
                 }
             }
         }   
</code></pre>
</li>
<li>Client sends an identical search request, returning the opaque cookie, asking for the next page.
<pre><code>
  // Re-activate paged results
         ctx.setRequestControls(new Control[]{
             new PagedResultsControl(pageSize, cookie, Control.CRITICAL) });
</code></pre>
</li>
1. Server responds with five entries plus an indication that there are more entries The client repeats the search performed in step 4 until a null cookie is returned by the server, which indicates no more entries to be sent by the server.

The complete JNDI example can be found 
[`here`](examples/PagedSearch.java).

**Note:** The Paged Search Control is supported by the Windows Active Directory Server. It's not supported by the Oracle Directory Server version 5.2
