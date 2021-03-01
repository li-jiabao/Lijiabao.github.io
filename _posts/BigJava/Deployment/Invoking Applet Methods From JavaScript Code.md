
# Invoking Applet Methods From JavaScript Code

JavaScript code on a web page can interact with Java applets embedded on the page. JavaScript code can perform operations such as the following:

- Invoke methods on Java objects
- Get and set fields in Java objects
- Get and set Java array elements

The 
[LiveConnect Specification](http://www.oracle.com/technetwork/java/javase/plugin2-142482.html#LIVECONNECT) describes details about how JavaScript code communicates with Java code.

Security warnings are shown when JavaScript code makes calls to a Java applet. To suppress these warnings, add the `Caller-Allowable-Codebase` attribute to the JAR file manifest. Specify the location of the JavaScript code that is allowed to make calls to the applet. See
[JAR File Manifest Attributes for Security](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html) for information about the `Caller-Allowable-Codebase` attribute.

This topic explores JavaScript code to Java applet communication using the Math applet example. The `MathApplet` class and supporting `Calculator` class provide a set of public methods and variables. The JavaScript code on the web page invokes and evaluates these public members to pass data and retrieve calculated results.

## Math Applet and Related Classes

Here is the source code for the 
[`<code>MathApplet`</code>](examples/applet_InvokingAppletMethodsFromJavaScript/src/jstojava/MathApplet.java) class. The `getCalculator` method returns a reference to the `Calculator` helper class.

```

 
package jstojava;
import java.applet.Applet;

public class MathApplet extends Applet{

    public String userName = null;
         
    public String getGreeting() {
        return "Hello " + userName;
    }
    
    public Calculator getCalculator() {
        return new Calculator();
    } 
    
    public DateHelper getDateHelper() {
        return new DateHelper();
    }
    
    public void printOut(String text) {
        System.out.println(text);
    }
}

```

The methods in the 
[`<code>Calculator`</code>](examples/applet_InvokingAppletMethodsFromJavaScript/src/jstojava/Calculator.java) class let the user set two values, add numbers, and retrieve the numbers in a range.

```


package jstojava;

public class Calculator {
    private int a = 0;
    private int b = 0; // assume b &gt; a
    
    public void setNums(int numA, int numB) {
        a = numA;
        b = numB;
    }
    
    public int add() {
        return a + b;
    }
    
    public int [] getNumInRange() {    
        int x = a;
        int len = (b - a) + 1;
        int [] range = new int [len];
        for (int i = 0; i &lt; len; i++) {
            range[i]= x++;
            System.out.println("i: " + i + " ; range[i]: " + range[i]);
        }
        return range;
    }
}

```

The `getDate` method of the 
[`<code>DateHelper`</code>](examples/applet_InvokingAppletMethodsFromJavaScript/src/jstojava/DateHelper.java) class returns the current date.

```


package jstojava;
import java.util.Date;
import java.text.SimpleDateFormat;

public class DateHelper {
    
    public static String label = null;
        
    public String getDate() {
        return label + " " + new SimpleDateFormat().format(new Date());
    }

}

```

## Deploying the Applet

Deploy the applet in a web page, 
[`<code>AppletPage.html`</code>](examples/dist/applet_InvokingAppletMethodsFromJavaScript/AppletPage.html) When deploying the applet, make sure that you specify an id for the applet. The applet id is used later to obtain a reference to the applet object.

```

&lt;script src=
  "https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;
    &lt;!-- applet id can be used to get a reference to
    the applet object --&gt;
    var attributes = { **id:'mathApplet'**,
        code:'jstojava.MathApplet',  width:1, height:1} ;
    var parameters = { jnlp_href: 'math_applet.jnlp'} ;
    deployJava.runApplet(attributes, parameters, '1.6');
&lt;/script&gt;

```

Next, add some JavaScript code to the 
[`<code>AppletPage.html`</code>](examples/dist/applet_InvokingAppletMethodsFromJavaScript/AppletPage.html) web page. The JavaScript code can use the applet id as a reference to the applet object and invoke the applet's methods. In the example shown next, the JavaScript code sets the applet's public member variables, invokes public methods, and retrieves a reference to another object referenced by the applet (`Calculator`). The JavaScript code is able to handle primitive, array, and object return types.

```

&lt;script language="javascript"&gt;
    function enterNums(){
        var numA = prompt('Enter number \'a\'?','0');
        var numB = prompt(
            'Enter number \'b\' (should be greater than number \'a\' ?','1');
        // set applet's public variable
        mathApplet.userName = "John Doe";

        // invoke public applet method
        var greeting = mathApplet.getGreeting();

        // get another class referenced by applet and
        // invoke its methods
        var calculator = mathApplet.getCalculator();
        calculator.setNums(numA, numB);

        // primitive datatype returned by applet
        var sum = calculator.add();

        // array returned by applet
        var numRange = calculator.getNumInRange();

        // check Java console log for this message
        mathApplet.printOut("Testing printing to System.out");

        // get another class, set static field and invoke its methods
        var dateHelper = mathApplet.getDateHelper();
        dateHelper.label = "Today\'s date is: ";
        var dateStr = dateHelper.getDate();
        &lt;!-- ... --&gt;
&lt;/script&gt;

```

The Math applet displays the following results on the web page when the number a = 0 and b = 5:

```

Results of JavaScript to Java Communication

Hello John Doe

a = 0 ; b = 5

Sum: 5

Numbers in range array: [ 0, 1, 2, 3, 4, 5 ]

Today's date is: 5/28/13 4:12 PM //*shows current date*

```

Open 
[`<code>AppletPage.html`</code>](examples/dist/applet_InvokingAppletMethodsFromJavaScript/AppletPage.html) in a browser to view the Math applet.

Check 
[security restrictions](security.html#jsNote) placed on applets invoked by JavaScript code.


[Download source code](examplesIndex.html#InvokingAppletMethodsFromJavaScript) for the **Invoking Applet Methods From JavaScript Code** example to experiment further.
