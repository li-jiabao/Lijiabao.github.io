
# Trail: Deployment

Java rich internet applications (RIA) are applications that have traits similar to desktop applications, but are deployed via the Internet. Java RIAs may be developed and deployed as Java applets or Java Web Start applications.

- Applets - Java applets run in the context of a browser. The Java Plug-in software controls the execution and lifecycle of Java applets.
- Java Web Start applications - Java Web Start applications are launched via a browser the first time. They may subsequently be launched from a desktop shortcut. Once a Java Web Start application is downloaded and its security certificate has been accepted by the user, it behaves **almost** like a standalone application.

## <a name="componentBasedArch" id="componentBasedArch">Component-Based Architecture for RIAs</a>

In the past, the decision of whether to deploy a Java rich internet application inside the browser as an applet, or outside the browser as a Java Web Start application, could significantly impact the design of the application. With the latest Java Plug-in, this decision has been greatly simplified.

Traditionally, applications construct their user interfaces, including the top-level `Frame`, in the `main` method. This programming style prevents easy re-deployment of the application in the browser, because it assumes that the application creates its own `Frame`. When running in the browser as an applet, the applet is the top level container that should hold the user interface for the application. A top-level `Frame` is not needed.

Use *component-based architecture* when designing your Java rich internet application. Try to organize its functionality into one or more components that can be composed together. In this context, the term "component" refers to a GUI element that is a subclass of the AWT `Component` class, the Swing `JComponent` class, or another subclass. For example, you could have a top level `JPanel` which contains other UI components in it (like a combination of more nested JPanels and text fields, combo boxes etc.). With such a design, it becomes relatively easy to deploy the core functionality as an applet or a Java Web Start application.

To deploy as a Java applet, you just need to wrap the core functionality in an `Applet` or `JApplet` and add the browser specific functionality, if necessary. To deploy as a Java Web Start application, wrap the functionality in a `JFrame`.

## Choosing Between Java Applets and Java Web Start Applications

The 
[Rich Internet Applications Decision Guide](./_riaDecisionGuide.html) contains detailed information to help you decide whether to deploy your code as a Java applet or Java Web Start application.

## The Self-Contained Application Alternative

Self-contained applications provide a deployment option that does not require a browser. Users install your application locally and run it similar to native applications. Self-contained applications include the JRE needed to run the application, so users always have the correct JRE.

This trail discusses the development and deployment of RIAs and self-contained applications. See 
[What's New](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/whatsnew_deployment.html) for capabilities introduced in various versions of the client Java Runtime Environment (JRE) software.

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Developing and Deploying Java Applets**](applet/index.html) <!--    Web Start    -->

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Developing and Deploying Java Web Start Applications**](webstart/index.html) <!--    Doing More With RIA    -->

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Doing More With Java Rich Internet Applications**](doingMoreWithRIA/index.html) <!--    Deployment In-Depth    -->

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Deployment In-Depth**](deploymentInDepth/index.html)

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Deploying Self-Contained Applications**](selfContainedApps/index.html)

Supporting Tools

[<img src="../images/coreIcon.gif" align="left" width="20" height="20" border="0" alt="trail icon" /> **Packaging Programs in JAR Files**](jar/index.html)
