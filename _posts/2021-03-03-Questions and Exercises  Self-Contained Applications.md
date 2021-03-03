---
title: Questions and Exercises  Self-Contained Applications
author: lijiabao
date: 2020-12-06 12:45:11.719094500 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Questions and Exercises: Self-Contained Applications

## Questions

<li>
Which of the following items is not an advantage of self-contained applications?
<ol type="A">
  1. Users install the application with an installer that is familiar to them.
  1. The application runs as a native application.
  1. The application requires less space on a user's machine.
  1. You control the version of the JRE that is used by the application.
  1. The application does not require a browser to run.

True or False: MIME type must always be used to set up a file association.

What elements are used to identify the entry points for self-contained applications in the `&lt;fx:deploy&gt;` Ant task?

## Exercises

<li>
Write the `&lt;fx:deploy&gt;` Ant task to generate a Windows MSI bundle for a simple application named My Sample App. The JAR file for the application is in the `dist` directory, the main class is `samples.MyApp`, and output files are to be written to the current directory.
</li>

<li>
Enhance the code from the previous exercise to create bundles for all Windows installers and define a file association for text files.
</li>


[Check your answers.](answers.html)
