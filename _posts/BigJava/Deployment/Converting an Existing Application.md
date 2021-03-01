
# Converting an Existing Application

Any standalone Java application or Java Web Start application can be packaged as a self-contained application. If you have a Java applet, see 
[Re-writing a Java Applet as a Java Web Start Application](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/applet_dev_guide.html#JSDPG1036) for information on coverting the applet to a Java Web Start application, which can then be packaged as a self-contained application.

Before converting an application, make sure that you have the required pre-requisites installed for your platform. See
[Pre-Requisites for Packaging Self-Contained Applications](../selfContainedApps/prereqs.html) for information.

This section converts the Dynamic Tree Demo from  
[Deploying a Java Web Start Application](../webstart/deploying.html) to a self-contained application. 
You can download the source files for this demo from 
[Self-Contained Application Examples](../selfContainedApps/examplesIndex.html).

## Setting Up the Directories

Identify and organize the files that are needed by your application. A simple application might require only a JAR file. A more complex application might also require additional libraries or resources. Custom resources such as icons or configuration files can also be used by self-contained applications.

The Dynamic Tree Demo requires only the `DynamicTreeDemo.jar` file, which is in the `/dist` directory of the project. The HTML and JNLP files that were needed for the Java Web Start version of the application are not needed and are ignored by the bundlers for self-contained applications.

To provide a custom icon for the Dynamic Tree Demo, which represents the application when it is installed on a user's desktop, an icon is provided for each platform supported. These icons are placed in the `/src/package/<var>platform</var>` directories. The icon is provided in a different format for each supported platform: `.ico` format for Windows,  `.png` format for Linux, and `.icns` format for OS X.

The following example shows the directory structure for the Dynamic Tree Demo  project before the self-contained bundles are created:

```

/packager_DynamicTreeDemo     &lt;--- application project
   /dist
      DynamicTreeDemo.jar
      ...
   /src
      /package                &lt;--- custom resources
         /linux
         /macosx
         /windows
      /webstartComponentArch  &lt;--- application source files
      ...

```

## Setting Up the Build File

Set up the Ant tasks for the packaging tasks that are needed. These tasks can be added to the `build.xml` file for the project, or placed in a separate file that is imported by the `build.xml` file.

For the Dynamic Tree Demo, the `packager.xml` file in the root directory of the project contains the Ant tasks for generating the self-contained application bundles. The source for the `packager.xml` file is shown in the following example:

```

&lt;project name="DynamicTreePackaging" default="default" basedir="." xmlns:fx="javafx:com.sun.javafx.tools.ant"&gt;
    &lt;echo&gt;${java.home}/../lib/ant-javafx.jar&lt;/echo&gt;
    &lt;target name="package" depends="jar"&gt;
        &lt;taskdef resource="com/sun/javafx/tools/ant/antlib.xml"
                 uri="javafx:com.sun.javafx.tools.ant"
                 classpath="${java.home}/../lib/ant-javafx.jar;src"/&gt;

        &lt;fx:deploy outdir="${basedir}/build/packager" 
                   outfile="DynamicTreeDemo"
                   nativeBundles="all"
                   verbose="false"&gt;

            &lt;fx:application name="Dynamic Tree Demo"
                        mainClass="webstartComponentArch.DynamicTreeApplication"
                        version="1.0"
            /&gt;

            &lt;fx:resources&gt;
                &lt;fx:fileset dir="dist" includes="DynamicTreeDemo.jar"/&gt;
            &lt;/fx:resources&gt;

            &lt;fx:info title="Dynamic Tree Demo"
                     vendor="My Company"
                     description="A Demo of a Dynamic Swing Tree"
                     category="Demos"
                     copyright="(c) 2014 My Company"
                     license="3 Clause BSD"
            /&gt;

            &lt;fx:bundleArgument arg="linux.bundleName" value="dynamic-tree-demo"/&gt;
            &lt;fx:bundleArgument arg="email" value="maintainer@example.com"/&gt;
            &lt;fx:bundleArgument arg="mac.CFBundleName" value="Java Tree Demo"/&gt;
            &lt;fx:bundleArgument arg="win.menuGroup" value="Java Demos"/&gt;

        &lt;/fx:deploy&gt;
    &lt;/target&gt;
&lt;/project&gt;

```

Use the following information to set up the Ant tasks: 

  - Use `xmlns:fx="javafx:com.sun.javafx.tools.ant` for the namespace.
  
  <li>The `taskdef` task must be executed before the `fx:deploy` task. The `classpath` attribute contains the location of the `ant-javafx.jar` file from the JDK and the directory that contains the custom resources. For the Dynamic Tree Demo, the `classpath` attribute includes the `/src` directory, which contains the custom icons.
  </li>
  
   <li>Place the `fx:deploy` task inside the desired target. Specify the output directory where the native binaries are placed, and specify the native binaries that you want to produce.
   If `all` is specified for the native binaries, all possible binaries for the platform on which you execute this task file are generated, including the disk image. Valid values for all platforms are `all`; `image`, which generates the file directory on Windows and Linux and the `.app` file on OSX; and `installer`, which generates only installable bundles for the platform, not the disk image. Valid values for platform-specific binaries are `exe` and `msi` for Windows; `deb` and `rpm` for Linux; `deb`, `pkg`, and `mac.appStore` for OS X. You must have the required tools installed to build the binary of your choice.
  For the Dynamic Tree Demo, the `outdir` attribute is set to `${basedir}/build/packager`. `basedir` is defined in the `project` element, in this case it is set to the current directory. The `nativeBundles` attribute is set to `all` so all formats for the platform on which the packaging task is run are built.
  </li>
  
  - The `verbose` attribute is optional. Use this attribute to provide diagnostic information.
  
  - Provide information about the application. Set the name of the application in the `name` attribute of the `fx:application` element and the `title` attribute of the `fx:info` element. Set the version of the application in the `version` attribute of the `fx:application` element. Use the `fx:info` element to provide a description of the application, the name of the vendor, license information, and other metadata. 
  
  - Information about the JAR file and other resources is set in the `fx:resources` element.
  
  <li>Launch information is set in the `mainclass` attribute of the `fx:application` element.
  For the Dynamic Tree Demo, a simple single launcher is used, `webstartComponentArch.DynamicTreeApplication`, which is the main class for the application.</li>
  
  <li>Other platform-specific customizations are provided in the `fx:bundleArgument` elements. Arguments that are not recognized by a bundler are ignored, so one build file can contain the packaging information for all platforms.
  For the Dynamic Tree Demo, the following customizations are applied:
  <ul>
    - The bundle name for Linux is set to `dynamic-tree-demo`.
    - An email address is provided.
    - The name that appears in the menu bar for OS X is set to `Java Tree Demo`.
    - The name of the menu group in which the application is stored for Windows is set to `Java Demos`.
  
## Generating the Bundles

Run the packaging tasks that you created on the platform for which you want to build the bundle for your self-contained application.

For the Dynamic Tree Demo, run the following command from the root folder for the project:

```

     ant package

```

When the packaging task completes, the `build/packager/bundles` directory in the application project contains the native binaries that were produced.

The following example shows the directory structure for the Dynamic Tree Demo  project after the self-contained bundles are generated for Windows:

```

/packager_DynamicTreeDemo     &lt;--- application project
   /build
      /packager
         /bundles
            Dynamic Tree Demo         &lt;---folder image
            Dynamic Tree Demo-1.0.exe &lt;---EXE installer
            Dynamic Tree Demo-1.0.msi &lt;---MSI installer
      ...   
   /dist
      DynamicTreeDemo.jar
      ...
   /src
      /package                &lt;--- custom resources
         /linux
         /macosx
         /windows
      /webstartComponentArch  &lt;--- application source files
      ...

```

Note that in addition to the self-contained bundles, the packaging tool always generates the JAR, JNLP, annd HTML files for an application. These files provide other options for distributing your application.

## Additional References

For more information about self-contained applications, see 
[Self-Contained Application Packaging](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html).

For more information about the Ant tasks for the Java packaging tools, see <macroinline>
[JavaFX Ant Tasks](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_tasks.html), which are used for Java and JavaFX applications.</macroinline>

