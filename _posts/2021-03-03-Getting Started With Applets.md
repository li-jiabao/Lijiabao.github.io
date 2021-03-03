---
title: Getting Started With Applets
author: lijiabao
date: 2020-12-06 12:41:42.262204500 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Getting Started With Applets

The HelloWorld applet shown next is a Java class that displays the string "Hello World".

Following is the source code for the HelloWorld applet:

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

An applet such as this is typically managed and run by the **Java Plug-in** software in the browser.


[Download source code](examplesIndex.html#HelloWorld) for the **Hello World** example to experiment further.
