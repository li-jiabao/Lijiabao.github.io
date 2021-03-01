
# Java Network Launch Protocol

The Java Network Launch Protocol (JNLP) enables an application to be launched on a client desktop by using resources that are hosted on a remote web server. Java Plug-in software and Java Web Start software are considered JNLP clients because they can launch remotely hosted applets and applications on a client desktop. See 
[Java Network Launching Protocol and API Specification Change Log](http://www.oracle.com/technetwork/java/javase/jnlp-spec-log-139509.html) for details.

Recent improvements in deployment technologies enable us to launch rich Internet applications (RIAs) by using JNLP. Both applets and Java Web Start applications can be launched by using this protocol. RIAs that are launched by using JNLP also have access to JNLP APIs. These JNLP APIs allow the RIAs to access the client desktop with the user's permission.

JNLP is enabled by a RIA's JNLP file. The JNLP file describes the RIA. The JNLP file specifies the name of the main JAR file, the version of Java Runtime Environment software that is required to run the RIA, name and display information, optional packages, runtime parameters, system properties, and so on.

You can find more information about deploying RIAs by using JNLP in the following topics:

<li>
[Deploying an Applet](../applet/deployingApplet.html)</li>
<li>
[Deploying a Java Web Start Application](../webstart/deploying.html)</li>
<li>
[JNLP API](../doingMoreWithRIA/jnlpAPI.html)</li>
<li>
[Structure of the JNLP File](../deploymentInDepth/jnlpFileSyntax.html)</li>
