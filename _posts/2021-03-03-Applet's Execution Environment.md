---
title: Applet's Execution Environment
author: lijiabao
date: 2020-12-06 12:41:53.039968300 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Applet's Execution Environment

A Java applet runs in the context of a browser. The Java Plug-in software in the browser controls the launch and execution of Java applets. The browser also has a JavaScript interpreter, which runs the JavaScript code on a web page. This topic describes the behavior of the Java Plug-in software released in Java Platform, Standard Edition 6 update 10.

## Java Plug-in

The Java Plug-in software creates a worker thread for every Java applet. It launches an applet in an instance of the Java Runtime Environment (JRE) software. Normally, all applets run in the same instance of the JRE. The Java Plug-in software starts a new instance of the JRE in the following cases:

- When an applet requests to be executed in a specific version of the JRE.
- When an applet specifies its own JRE startup parameters, for example, the heap size. A new applet uses an existing JRE if its requirements are a subset of an existing JRE, otherwise, a new JRE instance is started.

An applet will run in an existing JRE if the following conditions are met:

- The JRE version required by the applet matches an existing JRE.
- The JRE's startup parameters satisfy the applet's requirements.

The following diagram shows how applets are executed in the JRE.

## Java Plug-in and JavaScript Interpreter Interaction

Java applets can invoke JavaScript functions present in the web page. JavaScript functions are also allowed to invoke methods of an applet embedded on the same web page. The Java Plug-in software and the JavaScript interpreter orchestrate calls from Java code to JavaScript code and calls from JavaScript code to Java code.

The Java Plug-in software is multi-threaded, while the JavaScript interpreter runs on a single thread. Hence, to avoid thread-related issues, especially when multiple applets are running simultaneously, keep the calls between Java code and JavaScript code short, and avoid round trips, if possible. See the following topics to find out more about interactions between Java code and JavaScript code:

<li>
[Invoking JavaScript Code From an Applet](../applet/invokingJavaScriptFromApplet.html)</li>
<li>
[Invoking Applet Methods From JavaScript Code](../applet/invokingAppletMethodsFromJavaScript.html)</li>
