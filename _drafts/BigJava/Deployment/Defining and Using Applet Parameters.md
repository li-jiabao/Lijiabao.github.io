
# Defining and Using Applet Parameters

Parameters are to Java applets what command-line arguments are to applications. They enable the user to customize the applet's operation. By defining parameters, you can increase your applet's flexibility, making your applet work in multiple situations without recoding and recompiling it.

## Specifying an Applet's Input Parameters

You can specify an applet's input parameters in the applet's Java Network Launch Protocol (JNLP) file or in the `&lt;parameter&gt;` element of the `&lt;applet&gt;` tag. It is usually better to specify the parameters in the applet's JNLP file so that the parameters can be supplied consistently even if the applet is deployed on multiple web pages. If the applet's parameters will vary by web page, then you should specify the parameters in the `&lt;parameter&gt;` element of the `&lt;applet&gt;` tag.

If you are unfamiliar with JNLP, see the 
[Java Network Launch Protocol](../deploymentInDepth/jnlp.html) topic for more information.

Consider an applet that takes three parameters. The `paramStr` and `paramInt` parameters are specified in the applet's JNLP file, 
[`<code>applettakesparams.jnlp`</code>](examples/applet_AppletWithParameters/src/applettakesparams.jnlp).

```

&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;jnlp spec="1.0+" codebase="" href=""&gt;
    &lt;!-- ... --&gt;
    &lt;applet-desc
         name="Applet Takes Params"
         main-class="AppletTakesParams"
         width="800"
         height="50"&gt;
             &lt;param name="paramStr"
                 value="someString"/&gt;
             &lt;param name="paramInt" value="22"/&gt;
     &lt;/applet-desc&gt;
     &lt;!-- ... --&gt;
&lt;/jnlp&gt;

```

The `paramOutsideJNLPFile` parameter is specified in the `parameters` variable passed to the Deployment Toolkit script's `runApplet` function in 
[`<code>AppletPage.html`</code>](examples/dist/applet_AppletWithParameters/AppletPage.html).

```

&lt;html&gt;
  &lt;head&gt;
    &lt;title&gt;Applet Takes Params&lt;/title&gt;
    &lt;meta http-equiv="Content-Type" content="text/html;
        charset=windows-1252"&gt;
  &lt;/head&gt;
  &lt;body&gt;
    &lt;h1&gt;Applet Takes Params&lt;/h1&gt;

    &lt;script
      src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
    &lt;script&gt;
        var attributes = { code:'AppletTakesParams.class',
            archive:'applet_AppletWithParameters.jar',
            width:800, height:50 };
        var parameters = {jnlp_href: 'applettakesparams.jnlp',
            **paramOutsideJNLPFile: 'fooOutsideJNLP'** };
        deployJava.runApplet(attributes, parameters, '1.7');
    &lt;/script&gt;

  &lt;/body&gt;
&lt;/html&gt;

```

See 
[Deploying an Applet](../deploymentInDepth/runAppletFunction.html) for more information about the `runApplet` function.

## Retrieving the Applet's Input Parameters

You can retrieve the applet's input parameters by using the 
[`getParameter`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html#getParameter-java.lang.String-) method of the `Applet` class. The 
[`<code>AppletTakesParams.java`</code>](examples/applet_AppletWithParameters/src/AppletTakesParams.java) applet retrieves and displays all its input parameters (`paramStr`, `paramInt`, and `paramOutsideJNLPFile`).

```


import javax.swing.JApplet;
import javax.swing.SwingUtilities;
import javax.swing.JLabel;

public class AppletTakesParams extends JApplet {
    public void init() {
        final String  inputStr = getParameter("paramStr");        
        final int inputInt = Integer.parseInt(getParameter("paramInt"));
        final String inputOutsideJNLPFile = getParameter("paramOutsideJNLPFile");

        try {
            SwingUtilities.invokeAndWait(new Runnable() {
                public void run() {
                    createGUI(inputStr, inputInt, inputOutsideJNLPFile);
                }
            });
        } catch (Exception e) {
            System.err.println("createGUI didn't successfully complete");
        }
    }
    private void createGUI(String inputStr, int inputInt, String inputOutsideJNLPFile) {
        String text = "Applet's parameters are -- inputStr: " + inputStr +
                ",   inputInt: " + inputInt +
                ",   paramOutsideJNLPFile: " + inputOutsideJNLPFile;
        JLabel lbl = new JLabel(text);
        add(lbl);
    }
}

```

The `AppletTakesParams` applet is shown next.


[Download source code](examplesIndex.html#AppletWithParameters) for the **Applet With Parameters** example to experiment further.
