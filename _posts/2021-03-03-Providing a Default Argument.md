---
title: Providing a Default Argument
author: lijiabao
date: 2020-12-06 12:45:03.308967500 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Providing a Default Argument

Arguments are passed to Java applications when an application is started. Self-contained applications can be set up with a default argument that is used when no argument is specified. The `&lt;fx:argument&gt;` element is used to define the argument. More than one argument can be passed by adding an `&lt;fx:argument&gt;` element for each argument. See 
[&lt;fx:argument&gt;](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html#JSDPG528) for information about this element.

The File Association Demo described in 
[Using File Associations](../selfContainedApps/fileassociation.html) is set up use the name of one of the sample files that is bundled with the application as the default argument. 

The following code in the `&lt;fx:deploy&gt;` task in the `build.xml` file shows how the default argument is defined:

```

&lt;fx:application id="fileassociationdemo"
                name="File Association Demo"
                mainClass="${main.class}"
                version="1.0"&gt;
    &lt;fx:argument&gt;sample.js&lt;/fx:argument&gt;
&lt;/fx:application&gt;

```

See 
[`build.xml`](examples/packager_FileAssociations/build.xml) for the complete build code.

You can download the source files for the File Association Demo from 
[Self-Contained Application Examples](../selfContainedApps/examplesIndex.html).

## Additional Resources

For more information about default arguments, see 
[Passing Arguments to a Self-Contained Application](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html#JSDPG995).

For more information about JavaFX Ant arguments, see 
[JavaFX Ant Task Reference](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_task_reference.html).
