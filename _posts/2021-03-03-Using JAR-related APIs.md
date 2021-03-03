---
title: Using JAR-related APIs
author: lijiabao
date: 2020-12-06 12:46:04.843051600 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Using JAR-related APIs

The Java platform contains several classes for use with JAR files. Some of these APIs are:

<li>
[The <tt>**java.util.jar**</tt> package](https://docs.oracle.com/javase/8/docs/api/java/util/jar/package-summary.html)</li>
<li>
[The <tt>**java.net.JarURLConnection**</tt> class](https://docs.oracle.com/javase/8/docs/api/java/net/JarURLConnection.html)</li>
<li>
[The <tt>**java.net.URLClassLoader**</tt> class](https://docs.oracle.com/javase/8/docs/api/java/net/URLClassLoader.html)</li>

To give you an idea of the possibilities that are opened up by these new APIs, this lesson guides you through the inner workings of a sample application called JarRunner.

## An Example - The JarRunner Application

JarRunner enables you to run an application that's bundled in a JAR file by specifying the JAR file's URL on the command line. For example, if an application called <tt>TargetApp</tt> were bundled in a JAR file at <tt>http://www.example.com/TargetApp.jar</tt>, you could run the application using this command:

```

java JarRunner http://www.example.com/TargetApp.jar

```

In order for JarRunner to work, it must be able to perform the following tasks, all of which are accomplished by using the new APIs:

- Access the remote JAR file and establish a communications link with it.
- Inspect the JAR file's manifest to see which of the classes in the archive is the main class.
- Load the classes in the JAR file.

The JarRunner application consists of two classes, <tt>JarRunner</tt> and <tt>JarClassLoader</tt>. <tt>JarRunner</tt> delegates most of the JAR-handling tasks to the <tt>JarClassLoader</tt> class. <tt>JarClassLoader</tt> extends the <tt>java.net.URLClassLoader</tt> class. You can browse the source code for the <tt>JarRunner</tt> and <tt>JarClassLoader</tt> classes before proceeding with the lesson:

<li>
[`JarRunner.java`](examples/JarRunner.java)</li>
<li>
[`JarClassLoader.java`](examples/JarClassLoader.java)</li>

This lesson has two parts:

## [The JarClassLoader Class](jarclassloader.html)

This section shows you how <tt>JarClassLoader</tt> uses some of the new APIs to perform tasks required for the JarRunner application to work.

## [The JarRunner Class](jarrunner.html)

This section summarizes the <tt>JarRunner</tt> class that comprises the JarRunner application.
