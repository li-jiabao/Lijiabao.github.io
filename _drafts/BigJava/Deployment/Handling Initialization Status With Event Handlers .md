
# Handling Initialization Status With Event Handlers 

An applet cannot handle requests from JavaScript code in the web page until the applet has been initialized. A call to an applet method or access to an applet variable from JavaScript code will be blocked until the applet's `init()` method is complete or the applet first invokes JavaScript code from the web page in which it is deployed. As the JavaScript implementation is single-threaded in many browsers, the web page may appear to be frozen during applet startup.

Beginning in the JDK 7 release, you can check the `status` variable of the applet while it is loading to determine if the applet is ready to handle requests from JavaScript code. You can also register event handlers that will automatically be invoked during various stages of applet initialization. To leverage this functionality, the applet should be deployed with the `java_status_events` parameter set to `"true"`.

In the Status and Event Handler example, JavaScript code registers an `onLoad` handler with the applet. The `onLoad` handler is automatically invoked by the Java Plug-in software when the applet has been initialized. The `onLoad` handler invokes other methods of the applet to draw the graph on the web page. The `init` method of the 
[`<code>DrawingApplet`</code>](examples/applet_StatusAndCallback/src/DrawingApplet.java) class sleeps for two seconds to simulate a long applet initialization period.

The following steps describe how to register event handlers and check an applet's status. See 
[Applet Status and Event Handlers](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/applet_dev_guide.html#JSDPG719) for a complete list of applet status values and applet events for which event handlers can be registered.

<li>Create a JavaScript function to register event handlers. The following code snippet shows the `registerAppletStateHandler` function that registers an `onLoad` event handler if the applet has not already loaded.
<pre><code>
&lt;script&gt;
&lt;!-- ... --&gt;
    var READY = 2;
    function **registerAppletStateHandler()** {
        // register onLoad handler if applet has
        // not loaded yet
        if (drawApplet.status &lt; READY)  {                 
            drawApplet.onLoad = onLoadHandler;
        } else if (drawApplet.status &gt;= READY) {
            // applet has already loaded or there
            // was an error
            document.getElementById("mydiv").innerHTML = 
              "Applet event handler not registered because applet status is: "
               + drawApplet.status;    
        }
    }
    
    function **onLoadHandler()** {
        // event handler for ready state
        document.getElementById("mydiv").innerHTML =
            "Applet has loaded";
        draw();
    }
&lt;!-- ... --&gt;
&lt;/script&gt;        
</code></pre>
</li>
<li>Invoke the previously created `registerAppletStateHandler` function in the `body` tag's onload method. This ensures that the HTML tag for the applet has been created in the Document Object Model (DOM) tree of the web page before the applet's event handlers are registered.
<pre><code>
&lt;body onload="registerAppletStateHandler()"&gt;
</code></pre>
</li>
<li>Deploy the applet with the `java_status_events` parameter set to `"true"`.
<pre><code>
&lt;script src=
  "https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;
    // set java_status_events parameter to true 
    var attributes = { id:'drawApplet',
        code:'DrawingApplet.class',
        archive: 'applet_StatusAndCallback.jar',
        width:600, height:400} ;
    var parameters = {**java_status_events: 'true'**, permissions:'sandbox' } ;
    deployJava.runApplet(attributes, parameters, '1.7');
&lt;/script&gt;
</code></pre>
</li>

Open 
[`<code>AppletPage.html`</code>](examples/dist/applet_StatusAndCallback/AppletPage.html) in a browser to view the behavior of applet event handlers. In the 
[`<code>AppletPageUpdatedDuringLoading.html`</code>](examples/dist/applet_StatusAndCallback/AppletPageUpdatedDuringLoading.html) page, the `status` variable of the applet is checked to determine if the applet has been loaded. Based on the status, the web page is continuously updated while the applet is being loaded.


[Download source code](examplesIndex.html#StatusEventHandler) for the **Status and Event Handler** example to experiment further.
