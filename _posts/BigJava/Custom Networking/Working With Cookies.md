
# HTTP State Management With Cookies

The HTTP state management mechanism specifies a way to create a stateful session with HTTP requests and responses.

Generally, HTTP request/response pairs are independent of each other. However, the state management mechanism enables clients and servers that can exchange state information to put these pairs in a larger context, which is called a *session*. The state information used to create and maintain the session is called a *cookie*.

A cookie is a piece of data that can be stored in a browser's cache. If you visit a web site and then revisit it, the cookie data can be used to identify you as a return visitor. Cookies enable state information, such as an online shopping cart, to be remembered. A cookie can be short term, holding data for a single web session, that is, until you close the browser, or a cookie can be longer term, holding data for a week or a year.

For more information about HTTP state management, see 

[RFC 2965: HTTP State Management Mechanism](http://www.ietf.org/rfc/rfc2965.txt).
