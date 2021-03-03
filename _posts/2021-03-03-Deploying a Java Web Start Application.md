---
title: Deploying a Java Web Start Application
author: lijiabao
date: 2020-12-06 12:44:08.502206900 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Deploying a Java Web Start Application

You can deploy Java Web Start applications by using the `createWebStartLaunchButton` function of the 
[Deployment Toolkit](https://www.java.com/js/deployJava.txt) script. Java Web Start applications are launched using Java Network Launch Protocol (JNLP) The `createWebStartLaunchButton` function generates a link (HTML anchor tag - `&lt;a&gt;`) to the Java Web Start application's JNLP file.

This generated anchor tag is the Java Web Start application's <img src="../../images/jws-launch-button.png" alt="Launch button" /> button. When the end user clicks the Launch button, the Deployment Toolkit script ensures that the appropriate Java Runtime Environment (JRE) software is installed and then launches the Java Web Start application.

**Function signature:** `createWebStartLaunchButton: function(jnlp, minimumVersion)` or `createWebStartLaunchButton: function(jnlp)`

Parameters:

- `jnlp` &#8211; The URL of the JNLP file containing deployment information for the Java Web Start application. This URL should be an absolute path.
- `minimumVersion` &#8211; The minimum version of JRE software required to run this application

Usage:

<li><a name="minJre" id="minJre"></a>Specifying a minimum version of JRE software that is required to run the application
<pre><code>
&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;
    var url = "http://java.sun.com/javase/technologies/desktop/javawebstart/apps/notepad.jnlp";
    deployJava.createWebStartLaunchButton(url, '1.6.0');
&lt;/script&gt;
</code></pre>
</li>
<li>Enabling Java Web Start application to run on any JRE software version
Use the `createWebStartLaunchButton: function(jnlp)` function if your application does not have a minimum JRE software version requirement.
</li>
