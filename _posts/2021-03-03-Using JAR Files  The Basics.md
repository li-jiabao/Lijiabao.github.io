---
title: Using JAR Files  The Basics
author: lijiabao
date: 2020-12-06 12:45:17.948632000 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Using JAR Files: The Basics

JAR files are packaged with the ZIP file format, so you can use them for tasks such as lossless data compression, archiving, decompression, and archive unpacking. These tasks are among the most common uses of JAR files, and you can realize many JAR file benefits using only these basic features.

Even if you want to take advantage of advanced functionality provided by the JAR file format such as electronic signing, you'll first need to become familiar with the fundamental operations.

To perform basic tasks with JAR files, you use the Java Archive Tool provided as part of the Java Development Kit (JDK). Because the Java Archive tool is invoked by using the <tt>jar</tt> command, this tutorial refers to it as 'the Jar tool'.

As a synopsis and preview of some of the topics to be covered in this section, the following table summarizes common JAR file operations:
<th id="h1">Operation</th><th id="h2">Command</th>
<td headers="h1">To create a JAR file</td><td headers="h2"><tt>jar cf *jar-file input-file(s)*</tt></td>
<td headers="h1">To view the contents of a JAR file</td><td headers="h2"><tt>jar tf *jar-file*</tt></td>
<td headers="h1">To extract the contents of a JAR file</td><td headers="h2"><tt>jar xf *jar-file*</tt></td>
<td headers="h1">To extract specific files from a JAR file</td><td headers="h2"><tt>jar xf *jar-file archived-file(s)*</tt></td>
<td headers="h1">To run an application packaged as a JAR file (requires the [<tt>Main-class</tt>](appman.html) manifest header)</td><td headers="h2" valign="middle"><tt>java -jar *app.jar*</tt></td>
<td headers="h1">To invoke an applet packaged as a JAR file</td><td headers="h2" valign="middle"><pre>`&lt;applet code=**AppletClassName.class**        archive="**JarFileName.jar**"        width=**width** height=**height**&gt;&lt;/applet&gt;`</pre></td>

This section shows you how to perform the most common JAR-file operations, with examples for each of the basic features:

## 
[Creating a JAR File](build.html)

This section shows you how to use the Jar tool to package files and directories into a JAR file.

## 
[Viewing the Contents of a JAR File](view.html)

You can display a JAR file's table of contents to see what it contains without actually unpacking the JAR file.

## 
[Extracting the Contents of a JAR File](unpack.html)

You can use the Jar tool to unpack a JAR file. When extracting files, the Jar tool makes copies of the desired files and writes them to the current directory, reproducing the directory structure that the files have in the archive.

## 
[Updating a JAR File](update.html)

This section shows you how to update the contents of an existing JAR file by modifying its manifest or by adding files.

## 
[Running JAR-Packaged Software](run.html)

This section shows you how to invoke and run applets and applications that are packaged in JAR files.

## Additional References

The documentation for the JDK includes reference pages for the Jar tool:

<li>
[Jar tool reference for the Windows platform](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jar.html)</li>
<li>
[Jar tool reference for UNIX-based platforms](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jar.html)</li>
