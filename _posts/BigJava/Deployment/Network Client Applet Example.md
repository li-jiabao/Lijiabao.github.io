
# Network Client Applet Example

The 
[`QuoteClientApplet`](examples/QuoteClientApplet.java) class allows you to fetch quotations from a server-side application that runs on the same host as this applet. This class also displays the quotation received from the server.

The 
[`QuoteServer.java`](examples/QuoteServer.java) and 
[`QuoteServerThread.java`](examples/QuoteServerThread.java) classes make up the server-side application that returns quotations. Here's a text file (
[`one-liners.txt`](examples/one-liners.txt)) that contains a number of quotations.

Perform the following steps to test `QuoteClientApplet`.

<li>Download and save the following files to your local machine.
<ul>
<li>
[`QuoteClientApplet`](examples/QuoteClientApplet.java)</li>
<li>
[`QuoteServer.java`](examples/QuoteServer.java)</li>
<li>
[`QuoteServerThread.java`](examples/QuoteServerThread.java)</li>
<li>
[`one-liners.txt`](examples/one-liners.txt)</li>
<li>
[`quoteApplet.html`](examples/quoteApplet.html)</li>
</ul>
</li>
<li>Include the following HTML code in a web page to deploy `QuoteClientApplet`.
<pre><code>
&lt;script src=
  "https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt; 
    var attributes =
      { code:'QuoteClientApplet.class',  width:500, height:100} ; 
    var parameters =
      { codebase_lookup:'true', permissions:'sandbox' };
    deployJava.runApplet(attributes, parameters, '1.6'); 
&lt;/script&gt;

</code></pre>
Alternatively, you can use the 
[`quoteApplet.html`](examples/quoteApplet.html) page that already contains this HTML code.</li>
1. Compile the `QuoteClientApplet.java` class. Copy the generated class files to the same directory where you saved your web page.
1. Compile the classes for the server-side application, `QuoteServer.java` and `QuoteServerThread.java`.
1. Copy the file `one-liners.txt` to the directory that has the class files for the server-side application (generated in the previous step).
<li>Start the server-side application.
<pre><code>
java QuoteServer
</code></pre>
You should see a message with the port number, as shown in the following example. Note the port number.
<pre><code>
QuoteServer listening on port:3862
</code></pre>
</li>
<li>Open the web page containing your applet in a browser by entering the URL of the web page. The host name in the URL should be the same as the name of the host on which the server-side application is running.
For example, if the server-side application is running on a machine named `JohnDoeMachine`, you should enter a similar URL. The exact port number and path will vary depending on your web server setup.
<pre><code>
http://JohnDoeMachine:8080/quoteApplet/quoteApplet.html
</code></pre>
The `QuoteClientApplet` will be displayed on the web page.</li>
1. Enter the port number of your server-side application in the applet's text field and click OK. A quotation is displayed.

Here is a screen capture of the applet in action.


