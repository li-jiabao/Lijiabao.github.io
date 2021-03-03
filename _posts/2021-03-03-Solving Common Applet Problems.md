---
title: Solving Common Applet Problems
author: lijiabao
date: 2020-12-06 12:42:54.791244400 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Solving Common Applet Problems

This section covers some common problems that you might encounter when writing Java applets. After each problem is a list of possible reasons and solutions.

**Problem:** My applet does not display.

- Check the Java Console log for errors.
- Check the syntax of the applet's Java Network Launch Protocol (JNLP) file. Incorrect JNLP files are the most common reason for failures without obvious errors.
<li>Check the JavaScript syntax if deploying using the `runApplet` function of the Deployment Toolkit. See 
[Deploying an Applet](../deploymentInDepth/runAppletFunction.html) for details.</li>

**Problem:** The Java Console log displays java.lang.ClassNotFoundException.

- Make sure your Java source files compiled correctly.
- If deploying using the `&lt;applet&gt;` tag, check that the path to the applet JAR file is specified accurately in the `archive` attribute.
- If launching using a JNLP file, check the path in the `jar` tag in the JNLP file.
- Make sure the applet's JAR file, JNLP file, and web page are located in the correct directory and reference each other accurately.

**Problem:** I was able to build the code once, but now the build fails even though there are no compilation errors.

- Close your browser and run the build again. The browser most likely has a lock on the JAR file, because of which the build process is unable to regenerate the JAR file.

**Problem:** When I try to load a web page that has an applet, my browser redirects me to `www.java.com` without any warning

<li>The applet on the web page is most likely deployed using the Deployment Toolkit script. The applet may require a later version of the Java Runtime Environment software than the version that currently exists on the client. Check the `minimumVersion` parameter of the `runApplet` function in the applet's web page. See 
[Deploying an Applet](../deploymentInDepth/runAppletFunction.html) for details.</li>

**Problem:** I fixed some bugs and re-built my applet's source code. When I reload the applet's web page, my fixes are not showing up.

- You may be viewing a previously cached version of the applet. Close the browser. Open the Java Control Panel and delete temporary internet files. This will remove your applet from cache. Try viewing your applet again.
