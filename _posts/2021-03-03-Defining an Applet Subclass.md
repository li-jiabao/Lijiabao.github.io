---
title: Defining an Applet Subclass
author: lijiabao
date: 2020-12-06 12:41:45.004855000 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Defining an Applet Subclass

Every Java applet must define a subclass of the `Applet` or `JApplet` class. In the Hello World applet, this subclass is called `HelloWorld`. The following is the source for the 
[`<code>HelloWorld`</code>](examples/applet_HelloWorld/src/HelloWorld.java) class.

```


import javax.swing.JApplet;
import javax.swing.SwingUtilities;
import javax.swing.JLabel;

public class HelloWorld extends JApplet {
    //Called when this applet is loaded into the browser.
    public void init() {
        //Execute a job on the event-dispatching thread; creating this applet's GUI.
        try {
            SwingUtilities.invokeAndWait(new Runnable() {
                public void run() {
                    JLabel lbl = new JLabel("Hello World");
                    add(lbl);
                }
            });
        } catch (Exception e) {
            System.err.println("createGUI didn't complete successfully");
        }
    }
}

```

Java applets inherit significant functionality from the `Applet` or `JApplet` class, including the capabilities to communicate with the browser and present a graphical user interface (GUI) to the user.

An applet that will be using GUI components from Swing (Java's GUI toolkit) should extend the 
[`javax.swing.JApplet`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JApplet.html) base class, which provides the best integration with Swing's GUI facilities.

`JApplet` provides a root pane, which is the same top-level component structure as Swing's `JFrame` and `JDialog` components, whereas `Applet` provides just a basic panel. See 
[How to Use Root Panes](../../uiswing/components/rootpane.html) for more details on how to use this feature.

An applet can extend the 
[`java.applet.Applet`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html) class when it does not use Swing's GUI components.
