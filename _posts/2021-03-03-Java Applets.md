---
title: Java Applets
author: lijiabao
date: 2020-12-06 12:41:39.746280000 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Lesson: Java Applets

This lesson discusses the basics of Java applets, how to develop applets that interact richly with their environment, and how to deploy applets.

A Java applet is a special kind of Java program that a browser enabled with Java technology can download from the internet and run. An applet is typically embedded inside a web page and runs in the context of a browser. An applet must be a subclass of the `java.applet.Applet` class. The `Applet` class provides the standard interface between the applet and the browser environment.

Swing provides a special subclass of the `Applet` class called `javax.swing.JApplet`. The `JApplet` class should be used for all applets that use Swing components to construct their graphical user interfaces (GUIs).

The browser's Java Plug-in software manages the lifecycle of an applet.

Use a web server to test the examples in this lesson. The use of local applets is not recommended, and local applets are blocked when the security level setting in the Java Control Panel is set to High or Very High.
