
# Cookies

Web applications are typically a series of Hypertext Transfer Protocol (HTTP) requests and responses. As HTTP is a stateless protocol, information is not automatically saved between HTTP requests. Web applications use cookies to store state information on the client. Cookies can be used to store information about the user, the user's shopping cart, and so on.

## Types of Cookies

The two types of cookies follow:

- Session cookies &#8211; Session cookies are stored in memory and are accessible as long as the user is using the web application. Session cookies are lost when the user exits the web application. Such cookies are identified by a session ID and are most commonly used to store details of a shopping cart.
- Permanent cookies &#8211; Permanent cookies are used to store long-term information such as user preferences and user identification information. Permanent cookies are stored in persistent storage and are not lost when the user exits the application. Permanent cookies are lost when they expire.

## Cookie Support in Rich Internet Applications

Rich Internet applications (applets and Java Web Start applications) support session and permanent cookies. The underlying cookie store depends on the browser and the operating system on the client.

To learn more about cookies, see the following:


<li>
[Working With Cookies](../../networking/cookies/index.html) lesson in the Java Tutorial
</li>

<li>
[API Documentation for CookieManager](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html) and related classes</li>
