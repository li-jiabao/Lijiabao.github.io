---
title: Communicating With Other Applets
author: lijiabao
date: 2020-12-06 12:42:39.897696200 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Communicating With Other Applets

A Java applet can communicate with other Java applets by using JavaScript functions in the parent web page. JavaScript functions enable communication between applets by receiving messages from one applet and invoking methods of other applets. See the following topics for more information about the interaction between Java code and JavaScript code:

<li>
[Invoking JavaScript Code From an Applet](../applet/invokingJavaScriptFromApplet.html)</li>
<li>
[Invoking Applet Methods From JavaScript Code](../applet/invokingAppletMethodsFromJavaScript.html)</li>

You should avoid using the following mechanisms to find other applets and share data between applets:

- Avoid using static variables to share data between applets.
<li>Do not use the `getApplet` and `getApplets` methods of the 
[`AppletContext`](https://docs.oracle.com/javase/8/docs/api/java/applet/AppletContext.html) class to find other applets. These methods only find applets that are running in the same instance of the Java Runtime Environment software.</li>

Applets must originate from the same directory on the server in order to communicate with each other.

The Sender and Receiver applets are shown next. When a user clicks the button to increment the counter, the Sender applet invokes a JavaScript function to send a request to the Receiver applet. Upon receiving the request, the Receiver applet increments a counter variable and displays the value of the variable.

Sender Applet

<br />
<br />

Receiver Applet

To enable communication with another applet, obtain a reference to an instance of the `netscape.javascript.JSObject` class. Use this instance to invoke JavaScript functions. The 
[`Sender`](examples/applet_SenderReceiver/src/Sender.java) applet uses an instance of the `netscape.javascript.JSObject` class to invoke a JavaScript function called `sendMsgToIncrementCounter`.

```

try {
    JSObject window = JSObject.getWindow(this);
    window.eval("sendMsgToIncrementCounter()");
} catch (JSException jse) {
    // ...
}

```

Write the JavaScript function that will receive requests from one applet and invoke methods of another applet on the web page. The `sendMsgToIncrementCounter` JavaScript function invokes the Receiver applet's `incrementCounter` method.

```

&lt;script&gt;
    function sendMsgToIncrementCounter() {
        var myReceiver = document.getElementById("receiver");
        myReceiver.incrementCounter();
    } 
&lt;script&gt;

```

Note that the JavaScript code uses the name `receiver` to obtain a reference to the Receiver applet on the web page. This name should be the same as the value of the `id` attribute that is specified when you deploy the Receiver applet.

The 
[`Receiver`](examples/applet_SenderReceiver/src/Receiver.java) applet's `incrementCounter` method is shown next.

```

public void incrementCounter() {
    ctr++;
    String text = " Current Value Of Counter: "
        + (new Integer(ctr)).toString();
    ctrLbl.setText(text);
}

```

Deploy the applets on the web page as shown in the following code snippet. You can view the Sender and Receiver applets and associated JavaScript code in 
[`<code>AppletPage.html`</code>](examples/dist/applet_SenderReceiver/AppletPage.html).

```

&lt;!-- Sender Applet --&gt;
&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt; 
    var attributes = { code:'Sender.class',
        archive:'examples/dist/applet_SenderReceiver/applet_SenderReceiver.jar',
        width:300, height:50} ;
    var parameters = { permissions:'sandbox' };
    deployJava.runApplet(attributes, parameters, '1.6');
&lt;/script&gt;

&lt;!-- Receiver Applet --&gt;
&lt;script&gt; 
    var attributes = { **id:'receiver'**, code:'Receiver.class',
        archive:'examples/dist/applet_SenderReceiver/applet_SenderReceiver.jar',
        width:300, height:50} ;
    var parameters = { permissions:'sandbox' };
    deployJava.runApplet(attributes, parameters, '1.6');
&lt;/script&gt;

```


[Download source code](examplesIndex.html#SenderReceiver) for the **Sender Receiver Applets** example to experiment further.
