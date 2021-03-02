
# Working With a Server-Side Application 

Java applets, like other Java programs, can use the API defined in the `java.net` package to communicate across the network. A Java applet can communicate with server applications that run on the same host as the applet. This communication does not require any special setup on the server.

When the applet is deployed to a web server, use the `Applet` `getCodeBase` method and the `java.net.URL` `getHost` method to determine which host the applet came from, as follows:

```

String host = getCodeBase().getHost();

```

If the applet is deployed locally, the `getCodeBase` method returns null. Use of a web server is recommended.

After you have the correct host name, you can use all the networking code that is documented in the 
[Custom Networking](../../networking/index.html) trail.

For an example of implementing an applet that is a network client, see 
[Network Client Applet Example](clientExample.html).
