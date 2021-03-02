
# Connecting to a URL

After you've successfully created a `URL` object, you can call the `URL` object's `openConnection` method to get a `URLConnection` object, or one of its protocol specific subclasses, e.g. 
[`java.net.HttpURLConnection`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpURLConnection.html)

You can use this `URLConnection` object to setup parameters and general request properties that you may need before connecting. Connection to the remote object represented by the URL is only initiated when the `URLConnection.connect` method is called. When you do this you are initializing a communication link between your Java program and the URL over the network. For example, the following code opens a connection to the site `example.com`:

```

try {
    URL myURL = new URL("http://example.com/");
    URLConnection myURLConnection = myURL.openConnection();
    myURLConnection.connect();
} 
catch (MalformedURLException e) { 
    // new URL() failed
    // ...
} 
catch (IOException e) {   
    // openConnection() failed
    // ...
}

```

A new `URLConnection` object is created every time by calling the `openConnection` method of the protocol handler for this URL.

You are not always required to explicitly call the `connect` method to initiate the connection. Operations that depend on being connected, like `getInputStream`, `getOutputStream`, etc, will implicitly perform the connection, if necessary.

Now that you've successfully connected to your URL, you can use the `URLConnection` object to perform actions such as reading from or writing to the connection. The next section shows you how.
