---
title: Running a Java Web Start Application
author: lijiabao
date: 2020-12-06 12:43:14.570585600 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Running a Java Web Start Application


Users can run Java Web Start applications in the following ways:



<li>
[Running a Java Web Start Application From a Browser](#web)
</li>

<li>
[Running a Java Web Start Application From the Java Cache Viewer](#cache)
</li>

<li>
[Running a Java Web Start Application From the Desktop](#desktop)
</li>


<a name="web" id="web"></a>

## Running a Java Web Start Application From a Browser

You can run a Java Web Start application from a browser by clicking a link to the application's JNLP file. The following text is an example of a link to a JNLP file.

```

&lt;a href="/some/path/Notepad.jnlp"&gt;Launch Notepad Application&lt;/a&gt;

```

Java Web Start software loads and runs the application based on instructions in the JNLP file.

Try it now: 
[Run Notepad](https://docs.oracle.com/javase/tutorialJWS/samples/deployment/NotepadJWSProject/Notepad.jnlp)


## <a name="cache" id="cache">Running a Java Web Start Application From the Java Cache Viewer</a>

If you are using at least Java Platform, Standard Edition 6 or later, you can run a Java Web Start application through the Java Cache Viewer.

When Java Web Start software first loads an application, information from the application's JNLP file is stored in the local Java Cache Viewer. To launch the application again, you do not need to return to the web page where you first launched it; you can launch it from the Java Cache Viewer.

To open the Java Cache Viewer:

1. Open the Control Panel.
1. Double click on the Java icon. The Java Control Panel opens.
1. Select the General tab.
1. Click View. The Java Cache Viewer opens.

The application is listed on the Java Cache Viewer screen.



To run the application, select it and click the Run button, <img src="../../figures/deployment/webstart/JCRun.png" width="53" height="38" align="bottom" alt="The Run button" />, or double click the application. The application starts just as it did from the web page. <a name="desktop" id="desktop"></a>

## Running a Java Web Start Application From the Desktop

You can add a desktop shortcut to a Java Web Start application. Select the application in the Java Cache Viewer. Right-click and select Install Shortcuts or click the Install button, <img src="../../figures/deployment/webstart/JCInstall.png" width="45" height="38" align="bottom" alt="The Install button" />.

A shortcut is added to the desktop.

You can then launch the Java Web Start application just as you would launch any native application.
