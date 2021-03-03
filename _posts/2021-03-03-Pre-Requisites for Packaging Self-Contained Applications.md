---
title: Pre-Requisites for Packaging Self-Contained Applications
author: lijiabao
date: 2020-12-06 12:44:47.190887600 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Pre-Requisites for Packaging Self-Contained Applications

The Java Development Kit (JDK) is required to compile and package your application. The installable bundle must be created on the platform on which the self-contained application will run. For example, if your application runs on Windows and Linux, you must run the packaging tool on Windows to create a `.exe` or `.msi` bundle, and run the packaging tool on Linux to create a `.rpm` or `.deb` file. 

Third party tools are required to create the installable bundle. The following table identifies the tools for each supported platform.
<th id="h1">Platform</th><th id="h2">Format</th><th id="h3">Tool</th>
<td headers="h1">Windows</td><td headers="h2">EXE</td><td headers="h3">Inno Setup 5 or later</td>
<td headers="h1">Windows</td><td headers="h2">MSI</td><td headers="h3">WiX Toolset 3.8 or later</td>
<td headers="h1">Linux</td><td headers="h2">RPM</td><td headers="h3">RPMBuild</td>
<td headers="h1">Linux</td><td headers="h2">DEB</td><td headers="h3">Debian packaging tools</td>
<td headers="h1">OS X</td><td headers="h2">DMG</td><td headers="h3">&#160;</td>
<td headers="h1">OS X</td><td headers="h2">PKG</td><td headers="h3">&#160;</td>
