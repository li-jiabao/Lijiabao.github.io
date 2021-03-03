---
title: Setting Trusted Arguments and Secure Properties
author: lijiabao
date: 2020-12-06 12:43:29.801198900 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Setting Trusted Arguments and Secure Properties

You can set certain Java Virtual Machine arguments and secure properties for your rich Internet application (RIA) in the RIA's Java Network Launch Protocol (JNLP) file. For applets, you can also set arguments in the `java_arguments` parameter of the `&lt;applet&gt;` tag. Although there is a predefined set of secure properties, you can also define new secure properties by prefixing the property name with "`jnlp.`" or "`javaws.`". Properties can be retrieved in your RIA by using the `System.getProperty` method.

Consider the Properties and Arguments Demo applet. The following Java Virtual Machine arguments and properties are set in the applet's JNLP file, 
[`appletpropsargs.jnlp`](examples/applet_PropertiesAndVMArgs/src/appletpropsargs.jnlp).

- `-Xmx` &#8211; A secure argument set equal to "256M"
- `sun.java2d.noddraw` &#8211; A predefined secure property set equal to "true"
- `jnlp.myProperty` &#8211; A user-defined secure property set equal to "a user-defined property"

```

&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;jnlp spec="1.0+" codebase="" href=""&gt;
    &lt;information&gt;
        &lt;title&gt;Properties and Arguments Demo Applet&lt;/title&gt;
        &lt;vendor&gt;Dynamic Team&lt;/vendor&gt;
    &lt;/information&gt;
    &lt;resources&gt;
        &lt;!-- Application Resources --&gt;
        &lt;j2se version="1.6+"
              href="http://java.sun.com/products/autodl/j2se"
              &lt;!-- secure java vm argument --&gt;
              java-vm-args="-Xmx256M"/&gt;
        &lt;jar href="applet_PropertiesAndVMArgs.jar"
            main="true" /&gt;
            &lt;!-- secure properties --&gt;
        &lt;property name="sun.java2d.noddraw"
            value="true"/&gt;
        &lt;property name="jnlp.myProperty"
            value="a user-defined property"/&gt;
    &lt;/resources&gt;
    &lt;applet-desc 
         name="Properties and Arguments Demo Applet"
         main-class="PropertiesArgsDemoApplet"
         width="800"
         height="50"&gt;             
     &lt;/applet-desc&gt;
     &lt;update check="background"/&gt;
&lt;/jnlp&gt;

```

The 
[`PropertiesArgsDemoApplet`](examples/applet_PropertiesAndVMArgs/src/PropertiesArgsDemoApplet.java) class uses the `System.getProperty` method to retrieve the `java.version` property and other properties that are set in the JNLP file. The `PropertiesArgsDemoApplet` class also displays the properties.

```


import javax.swing.JApplet;
import javax.swing.SwingUtilities;
import javax.swing.JLabel;

public class PropertiesArgsDemoApplet extends JApplet {
    public void init() {
        final String javaVersion = System.getProperty("java.version");
        final String  swing2dNoDrawProperty = System.getProperty("sun.java2d.noddraw");
        final String  jnlpMyProperty = System.getProperty("jnlp.myProperty");        

        try {
            SwingUtilities.invokeAndWait(new Runnable() {
                public void run() {
                    createGUI(javaVersion, swing2dNoDrawProperty, jnlpMyProperty);
                }
            });
        } catch (Exception e) {
            System.err.println("createGUI didn't successfully complete");
        }
    }
    private void createGUI(String javaVersion, String swing2dNoDrawProperty, String jnlpMyProperty) {
        String text = "Properties: java.version = " + javaVersion + 
                ",  sun.java2d.noddraw = " + swing2dNoDrawProperty +
                ",   jnlp.myProperty = " + jnlpMyProperty;
        JLabel lbl = new JLabel(text);
        add(lbl);
    }
}

```

The Properties and Arguments Demo applet is shown next. You can also see the applet running in 
[`AppletPage.html`](examples/dist/applet_PropertiesAndVMArgs/AppletPage.html).


[Download source code](examplesIndex.html#PropertiesAndVMArgs) for the **Properties and Arguments Demo Applet** example to experiment further.

See 
[System Properties](properties.html) for a complete set of system properties that can be accessed by RIAs.
