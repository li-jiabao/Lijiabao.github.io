
# Custom CookieManager

Two aspects of the `CookieManager` class can be customized, the `CookiePolicy` and the `CookieStore`.

## CookiePolicy

For convenience, `CookiePolicy` defines the following pre-defined policies for accepting cookies:

- `CookiePolicy.ACCEPT_ORIGINAL_SERVER` only accepts cookies from the original server.
- `CookiePolicy.ACCEPT_ALL` accepts all cookies.
- `CookiePolicy.ACCEPT_NONE` accepts no cookies.
<li>You can also define your own cookie policy by implementing the `shouldAccept` method of `CookiePolicy`. You can then use this `CookiePolicy` by passing it to the multi-argument `CookieManager` constructor or by calling the `setCookiePolicy(cookiePolicy)` method to change an already existing cookie manager.
The following is an example of a cookie policy that rejects cookies from domains that are on a blacklist, before applying the `CookiePolicy.ACCEPT_ORIGINAL_SERVER` policy:
<pre><code>
import java.net.*;

public class BlacklistCookiePolicy implements CookiePolicy {
    String[] blacklist;

    public BlacklistCookiePolicy(String[] list) {
        blacklist = list;
    }

    public boolean shouldAccept(URI uri, HttpCookie cookie)  {
        String host;
        try {
            host =  InetAddress.getByName(uri.getHost()).getCanonicalHostName();
        } catch (UnknownHostException e) {
            host = uri.getHost();
        }

        for (int i = 0; i&lt;blacklist.length; i++) {
	    if (HttpCookie.domainMatches(blacklist[i], host)) {
                return false;
            }
        }

        return CookiePolicy.ACCEPT_ORIGINAL_SERVER.shouldAccept(uri, cookie);
    }
}
</code></pre>
When you create a `BlacklistCookiePolicy` instance, you pass it an array of strings representing the domains that you do not want to accept cookies from. Then, you set this `BlacklistCookiePolicy` instance as the cookie policy for your `CookieManager`. For example:
<pre><code>
String[] list = new String[]{ ".example.com" };
CookieManager cm = new CookieManager(null, new BlacklistCookiePolicy(list));
CookieHandler.setDefault(cm);
</code></pre>
The sample code will not accept cookies from hosts such as the following:
<pre><code>
host.example.com
domain.example.com
</code></pre>
However, this sample code will accept cookies from hosts such as the following:
<pre><code>
example.com
example.org
myhost.example.org
</code></pre>
<h2>CookieStore</h2>
A `CookieStore` is an interface that represents a storage area for cookies. `CookieManager` adds the cookies to the `CookieStore` for every HTTP response and retrieves cookies from the `CookieStore` for every HTTP request.
You can implement this interface to provide your own `CookieStore` and pass it to the `CookieManager` during creation. You cannot set the `CookieStore` after the `CookieManager` instance has been created. However, you can get a reference to the cookie store by calling `CookieManager.getCookieStore()`. Doing so can be useful as it enables you to leverage the default in-memory `CookieStore` implementation that is provided by Java SE and complement its functionality.
For example, you might want to create a persistent cookie store that would save cookies so that they can be used even if the Java Virtual Machine is restarted. Your implementation would work similar to the following:
<ol>
- Any cookies that were previously saved are read in.
- During runtime, cookies are stored and retrieved from memory.
- Cookies are written out to persistent storage before exiting.
</ol>
The following is an incomplete example of this cookie store. This example shows you how to leverage the Java SE default in-memory cookie store and how you might extend its functionality.
<pre><code>
import java.net.*;
import java.util.*;

public class PersistentCookieStore implements CookieStore, Runnable {
    CookieStore store;

    public PersistentCookieStore() {
        // get the default in memory cookie store
        store = new CookieManager().getCookieStore();

        // todo: read in cookies from persistant storage
        // and add them store

        // add a shutdown hook to write out the in memory cookies
        Runtime.getRuntime().addShutdownHook(new Thread(this)); 
    }

    public void run() {
        // todo: write cookies in store to persistent storage
    }

    public void	add(URI uri, HttpCookie cookie) {
        store.add(uri, cookie);
    }

    public List&lt;HttpCookie&gt; get(URI uri) {
        return store.get(uri);
    }

    public List&lt;HttpCookie&gt; getCookies() {
        return store.getCookies();
    }
    
    public List&lt;URI&gt; getURIs() {
        return store.getURIs();
    }

    public boolean remove(URI uri, HttpCookie cookie) {
        return store.remove(uri, cookie);
    }

    public boolean removeAll()  {
        return store.removeAll();
    }
}
</code></pre>
</li>
