
# Creating a URL

The easiest way to create a `URL` object is from a `String` that represents the human-readable form of the URL address. This is typically the form that another person will use for a URL. In your Java program, you can use a `String` containing this text to create a `URL` object:

```

URL myURL = new URL("http://example.com/");

```

The `URL` object created above represents an **absolute URL**. An absolute URL contains all of the information necessary to reach the resource in question. You can also create `URL` objects from a **relative URL** address.

## Creating a URL Relative to Another

A relative URL contains only enough information to reach the resource relative to (or in the context of) another URL.

Relative URL specifications are often used within HTML files. For example, suppose you write an HTML file called `JoesHomePage.html`. Within this page, are links to other pages, `PicturesOfMe.html` and `MyKids.html`, that are on the same machine and in the same directory as `JoesHomePage.html`. The links to `PicturesOfMe.html` and `MyKids.html` from `JoesHomePage.html` could be specified just as filenames, like this:

```

&lt;a href="PicturesOfMe.html"&gt;Pictures of Me&lt;/a&gt;
&lt;a href="MyKids.html"&gt;Pictures of My Kids&lt;/a&gt;

```

These URL addresses are **relative URLs**. That is, the URLs are specified relative to the file in which they are contained &#8212; `JoesHomePage.html`.

In your Java programs, you can create a `URL` object from a relative URL specification. For example, suppose you know two URLs at the site `example.com`:

```

http://example.com/pages/page1.html
http://example.com/pages/page2.html

```

You can create `URL` objects for these pages relative to their common base URL: `http://example.com/pages/` like this:

```

URL myURL = new URL("http://example.com/pages/");
URL page1URL = new URL(myURL, "page1.html");
URL page2URL = new URL(myURL, "page2.html");

```

This code snippet uses the `URL` constructor that lets you create a `URL` object from another `URL` object (the base) and a relative URL specification. The general form of this constructor is:

```

URL(URL **baseURL**, String **relativeURL**)

```

The first argument is a `URL` object that specifies the base of the new `URL`. The second argument is a `String` that specifies the rest of the resource name relative to the base. If `baseURL` is null, then this constructor treats `relativeURL` like an absolute URL specification. Conversely, if `relativeURL` is an absolute URL specification, then the constructor ignores `baseURL`.

This constructor is also useful for creating `URL` objects for named anchors (also called references) within a file. For example, suppose the `page1.html` file has a named anchor called `BOTTOM` at the bottom of the file. You can use the relative URL constructor to create a `URL` object for it like this:

```

URL page1BottomURL = new URL(page1URL,"#BOTTOM");

```

## Other URL Constructors

The `URL` class provides two additional constructors for creating a `URL` object. These constructors are useful when you are working with URLs, such as HTTP URLs, that have host name, filename, port number, and reference components in the resource name portion of the URL. These two constructors are useful when you do not have a String containing the complete URL specification, but you do know various components of the URL.

For example, suppose you design a network browsing panel similar to a file browsing panel that allows users to choose the protocol, host name, port number, and filename. You can construct a `URL` from the panel's components. The first constructor creates a `URL` object from a protocol, host name, and filename. The following code snippet creates a `URL` to the `page1.html` file at the `example.com` site:

```

new URL("http", "example.com", "/pages/page1.html");

```

This is equivalent to

```

new URL("http://example.com/pages/page1.html");

```

The first argument is the protocol, the second is the host name, and the last is the pathname of the file. Note that the filename contains a forward slash at the beginning. This indicates that the filename is specified from the root of the host.

The final `URL` constructor adds the port number to the list of arguments used in the previous constructor:

```

URL gamelan = new URL("http", "example.com", 80, "pages/page1.html"); 

```

This creates a `URL` object for the following URL:

```

http://example.com:80/pages/page1.html

```

If you construct a `URL` object using one of these constructors, you can get a `String` containing the complete URL address by using the `URL` object's `toString` method or the equivalent `toExternalForm` method.

## URL addresses with Special characters

Some URL addresses contain special characters, for example the space character. Like this:

```

http://example.com/hello world/

```

To make these characters legal they need to be encoded before passing them to the URL constructor.

```

URL url = new URL("http://example.com/hello%20world");

```

Encoding the special character(s) in this example is easy as there is only one character that needs encoding, but for URL addresses that have several of these characters or if you are unsure when writing your code what URL addresses you will need to access, you can use the multi-argument constructors of the 
[`java.net.URI`](https://docs.oracle.com/javase/8/docs/api/java/net/URI.html) class to automatically take care of the encoding for you.

```

URI uri = new URI("http", "example.com", "/hello world/", "");

```

And then convert the URI to a URL.

```

URL url = uri.toURL();

```

## MalformedURLException

Each of the four `URL` constructors throws a `MalformedURLException` if the arguments to the constructor refer to a `null` or unknown protocol. Typically, you want to catch and handle this exception by embedding your URL constructor statements in a `try`/`catch` pair, like this:

```

try {
    URL myURL = new URL(...);
} 
catch (MalformedURLException e) {
    // **exception handler code here**
    // ...
}

```

See 
[Exceptions](../../essential/exceptions/index.html) for information about handling exceptions.

`URL`s are "write-once" objects. Once you've created a `URL` object, you cannot change any of its attributes (protocol, host name, filename, or port number).
