
# Default CookieManager


[`java.net.CookieManager`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html) provides a concrete implementation of a `CookieHandler` and for most users is sufficient for handling HTTP state management. `CookieManager` separates the storage of cookies from the policy surrounding, accepting, and rejecting them. A `CookieManager` is initialized with a 
[`java.net.CookieStore`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieStore.html) and a 
[`java.net.CookiePolicy`](https://docs.oracle.com/javase/8/docs/api/java/net/CookiePolicy.html). `CookieStore` manages the storage of the cookies. `CookiePolicy` makes policy decisions on cookie acceptance and rejection.

The following code shows how to create and set a system-wide CookieManager:

```

java.net.CookieManager cm = new java.net.CookieManager();
java.net.CookieHandler.setDefault(cm);

```

The first line calls the default `CookieManager` constructor to create the instance. The second line calls the static `setDefault` method of `CookieHandler` to set the system-wide handler.

The default `CookieManager` constructor creates a new `CookieManager` instance with a default cookie store and accept policy. `CookieStore` is the place where any accepted HTTP cookie is stored. If not specified when created, a `CookieManager` instance will use an internal in-memory implementation. This implementation is not persistent and only lives for the lifetime of the Java Virtual Machine. Users requiring a persistent store must implement their own store.

The default cookie policy used by `CookieManager` is `CookiePolicy.ACCEPT_ORIGINAL_SERVER`, which only accepts cookies from the original server. So, the `Set-Cookie` response from the server must have a &#8220;domain&#8221; attribute set, and it must match the domain of the host in the URL. For more information, see 
[`java.net.HttpCookie.domainMatches`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpCookie.html#domainMatches-java.lang.String-java.lang.String-). Users requiring a different policy must implement the `CookiePolicy` interface and pass it to the `CookieManager` constructor or set it to an already constructed `CookieManager` instance by using the `setCookiePolicy(cookiePolicy)` method.

When retrieving cookies from the cookie store, `CookieManager` also enforces the path-match rule from section 3.3.4 of 

[RFC 2965](http://www.ietf.org/rfc/rfc2965.txt)
. So, a cookie must also have its &#8220;path&#8221; attribute set so that the path-match rule can be applied before the cookie is retrieved from the cookie store.

In summary, `CookieManager` provides the framework for handling cookies and provides a good default implementation for `CookieStore`. `CookieManager` is highly customizable by enabling you to set your own `CookieStore`, `CookiePolicy`, or both.
