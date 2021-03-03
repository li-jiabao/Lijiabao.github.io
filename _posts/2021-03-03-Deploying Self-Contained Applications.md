---
title: Deploying Self-Contained Applications
author: lijiabao
date: 2020-12-06 12:44:43.414391100 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Lesson: Deploying Self-Contained Applications

A self-contained application consists of a single, installable bundle that contains your application and a copy of the JRE needed to run the application. When the application is installed, it behaves the in the same way as any native application. Providing users with a self-contained application avoids the security issues related to running an application in a browser. 

You can customize a self-contained application by providing your own icons. File associations can be set up so when a user opens a file that your application can handle, your application is started automatically. Multiple entry points are supported so you can deliver a suite of applications in a single self-contained application bundle.

Self-contained applications can be packaged using the Java Packaging tools. The `javapackager` command creates the bundle for self-contained applications from the command line. NetBeans can also be used to created self-contained application bundles. This lesson describes how to use Ant tasks to create the bundles.

## Additional References

For more information about self-contained applications, see 
[Self-Contained Application Packaging](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html) in the Java Platform, Standard Edition Deployment Guide.

For information about Ant tasks for Java packaging, see 
[JavaFX Ant Tasks](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_tasks.html), which are used for packaging Java SE and JavaFX applications.

For information about the `javapackager` command, see 
[Java Deployment Tools](https://docs.oracle.com/javase/8/docs/technotes/tools/index.html#deployment).
