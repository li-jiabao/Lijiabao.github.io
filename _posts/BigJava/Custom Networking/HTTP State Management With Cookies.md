
# CookieHandler Callback Mechanism

HTTP state management is implemented in Java SE through the 
[`java.net.CookieHandler`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieHandler.html) class. A `CookieHandler` object provides a callback mechanism to provide an HTTP state management policy implementation in the HTTP protocol handler. That is, URLs that use HTTP as the protocol, `new URL("http://example.com")` for example, will use the HTTP protocol handler. This protocol handler calls back to the `CookieHandler` object, if set, to handle the state management.

The `CookieHandler` class is an abstract class that has two pairs of related methods. The first pair, `getDefault()` and `setDefault(cookieHandler)`, are static methods that enable you to discover the current handler that is installed and to install your own handler.

No default handler is installed, and installing a handler is done on a system-wide basis. For applications running within a secure environment, that is, they have a security manager installed, you must have special permission to get and set the handler. For more information, see 
[`java.net.CookieHandler.getDefault`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieHandler.html#getDefault--).

The second pair of related methods, `put(uri, responseHeaders)` and `get(uri, requestHeaders)`, enable you to set and get all the applicable cookies to and from a cookie cache for the specified URI in the response/request headers, respectively. These methods are abstract, and a concrete implementation of a `CookieHandler` must provide the implementation.

Java Web Start and Java Plug-in have a default `CookieHandler` installed. However, if you are running a stand-alone application and want to enable HTTP state management, you must set a system-wide handler. The next two pages in this lesson show you how to do so.
