---
title: Using Multiple Entry Points
author: lijiabao
date: 2020-12-06 12:45:09.122610700 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Using Multiple Entry Points

Self-contained applications are useful when you have a set of related applications that you want users to deploy. A self-contained application provides a single, installable bundle that installs all of the applications and the JRE needed to run them.

The Multiple Launchers Demo includes the Dynamic Tree Demo described in 
[Converting an Existing Application](../selfContainedApps/converting.html) and the File Association Demo described in 
[Using File Associations](../selfContainedApps/fileassociation.html). The `/src` directory of the project contains the source file for both applications.

You can download the source files for the Multiple Launchers Demo from 
[Self-Contained Application Examples](../selfContainedApps/examplesIndex.html).

The primary entry point for a self-contained application is identified by the `mainClass` attribute of the `&lt;fx:application&gt;` element. In the Multiple Launchers Demo, the primary entry point is the File Association Demo. The main class is `sample.fa.ScriptRunnerAppliation` for Linux and Windows, or `sample.fa.ScriptRunnerApplicationMac` for OS X. See 
[Using a Common Build File for All Platforms](../selfContainedApps/commonbuild.html) for information on how the class to use is determined when a single build file is used across platforms.

Each secondary entry point is identified by an instance of the  `&lt;fx:secondaryLauncher&gt;` element. See 
[&lt;fx:secondaryLauncher&gt;](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html#JSDPG1003) for information about this element.

In the Multiple Launchers Demo, the secondary entry point is the Dynamic Tree Demo. The following code in the `build.xml` file shows how the second entry point is defined:

```

&lt;fx:secondaryLauncher name="Dynamic Tree Demo"
    mainClass="webstartComponentArch.DynamicTreeApplication"
    version="1.0"
    title="Dynamic Tree Demo"
    vendor="My Company"
    description="A Demo of Multiple Launchers for JavaPackager"
    copyright="(c) 2014 My Company"
     menu="true"
     shortcut="false"
     &gt;
&lt;/fx:secondaryLauncher&gt;

```

See 
[`build.xml`](examples/packager_MultipleLaunchers/build.xml) for the complete build code.

To generate the installable bundles for the Multiple Launchers Demo, see the "Generating the Bundles" section in 
[Converting an Existing Application](../selfContainedApps/converting.html).

When your self-contained application is installed, the File Association Demo is installed with the Multiple Launchers entry point and the Dynamic Tree Demo is installed with its own entry point. For example, on Windows, the `Java Demos` folder in the Start menu contains two entries: Dynamic Tree Demo and Multiple Launchers Demo. Note that file associations are set up for the Multiple Launchers entry point, so opening a JavaScript or Groovy file starts Multiple Launchers.

## Additional Resources

For more information about multiple entry points, see 
[Supporting Multiple Entry Points](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html#JSDPG1000).

For more information about JavaFX Ant arguments, see 
[JavaFX Ant Task Reference](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html).
