
# Invoking JavaScript Code From an Applet

Java applets can invoke JavaScript functions present in the same web page as the applet. The 
[LiveConnect Specification](http://www.oracle.com/technetwork/java/javase/plugin2-142482.html#LIVECONNECT) describes details about how JavaScript code communicates with Java code.

The `netscape.javascript.JSObject` class enables Java applets to retrieve a reference to JavaScript objects and interact with the web page. The Data Summary applet described next invokes JavaScript code to retrieve information from the web page and writes a data summary back to the web page.

Assume you have a web page with a few JavaScript functions. The example 
[`<code>AppletPage.html`</code>](examples/dist/applet_InvokingJavaScriptFromApplet/AppletPage.html) has JavaScript functions to retrieve age, address, and phone numbers. There is also a variable called `userName` which has no value at the outset.

```

&lt;head&gt;
&lt;title&gt;Data Summary Applet Page - Java to JavaScript LiveConnect&lt;/title&gt;
&lt;meta http-equiv="Content-Type" content="text/html; charset=windows-1252"/&gt;
&lt;script language="javascript"&gt;
    var userName = "";
    
    // returns number
    function getAge() { 
        return 25;
    }
    // returns an object
    function address() { 
        this.street = "1 Example Lane";
        this.city = "Santa Clara";
        this.state = "CA";
    }
    // returns an array
    function getPhoneNums() { 
        return ["408-555-0100", "408-555-0102"];
    } 
    function writeSummary(summary) {
        summaryElem =
            document.getElementById("summary");
        summaryElem.innerHTML = summary;
    }
    &lt;/script&gt;

    &lt;!-- ... --&gt;      
&lt;/head&gt;
&lt;body&gt;
    &lt;script src =
      "https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
    &lt;script&gt; 
        &lt;!-- ... --&gt;
        deployJava.runApplet(attributes, parameters, '1.6'); 
    &lt;/script&gt;          
    &lt;!-- ... --&gt;
    &lt;p id="summary"/&gt;  // this HTML element contains
                             // the summary 
    &lt;!-- ... --&gt;
&lt;/body&gt;

```

Next, consider an applet class called `DataSummaryApplet`. The `DataSummaryApplet` class performs the following operations.

- Invokes the `JSObject`'s `setMember` method to set the `userName` variable to "John Doe".
- Retrieves the age, address, and phone numbers and builds a string containing a summary of this data.
- Invokes the `writeSummary` JavaScript function to write the summary back to the web page.

This applet first needs to retrieve a reference to `JSObject` as follows:

```

...
JSObject window = JSObject.getWindow(this);
...

```

Put the preceding statement in a try ...catch.. block to handle `netscape.javascript.JSException`.

Now that the applet has a reference to `JSObject`, it can invoke the relevant JavaScript functions by using the `eval` and `call` methods of `JSObject`.

```


package javatojs;

import java.applet.Applet;
import netscape.javascript.*; // add plugin.jar to classpath during compilation

public class DataSummaryApplet extends Applet {
    public void start() {
        try {
            JSObject window = JSObject.getWindow(this);

            String userName = "John Doe";

            // set JavaScript variable
            window.setMember("userName", userName);

            // invoke JavaScript function
            Number age = (Number) window.eval("getAge()");

            // get a JavaScript object and retrieve its contents
            JSObject address = (JSObject) window.eval("new address();");
            String addressStr = (String) address.getMember("street") + ", " +
                    (String) address.getMember("city") + ", " +
                    (String) address.getMember("state");

            // get an array from JavaScript and retrieve its contents
            JSObject phoneNums = (JSObject) window.eval("getPhoneNums()");
            String phoneNumStr = (String) phoneNums.getSlot(0) + ", " +
                    (String) phoneNums.getSlot(1);

            // dynamically change HTML in page; write data summary
            String summary = userName + " : " + age + " : " +
                    addressStr + " : " + phoneNumStr;
            window.call("writeSummary", new Object[] {summary})   ;
        } catch (JSException jse) {
            jse.printStackTrace();
        }
    }
}

```

To compile Java code that has a reference to classes in the `netscape.javascript` package, include `&lt;your JDK path&gt;/jre/lib/plugin.jar` in your classpath. At runtime, the Java Plug-in software automatically makes these classes available to applets.

The Data Summary applet displays the following result on the web page:

```

Result of applet's Java calls to JavaScript on this page
                
John Doe : 25 : 1 Example Lane, Santa Clara, CA : 408-555-0100, 408-555-0102

```

Open 
[`<code>AppletPage.html`</code>](examples/dist/applet_InvokingJavaScriptFromApplet/AppletPage.html) in a browser to view the Data Summary applet .


[Download source code](examplesIndex.html#InvokingJavaScriptFromApplet) for the **Invoking JavaScript Code From Applet** example to experiment further.
