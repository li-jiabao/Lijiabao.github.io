
# Enhancing Security with Manifest Attributes

The following JAR file manifest attributes are available to help ensure the security of your applet or Java Web Start application. Only the <tt>Permissions</tt> attribute is required.

<li>The <tt>Permissions</tt> attribute is used to ensure that the application requests only the level of permissions that is specified in the applet tag or JNLP file used to invoke the application. Use this attribute to help prevent someone from re-deploying an application that is signed with your certificate and running it at a different privilege level.
<p>This attribute is required in the manifest for the main JAR file. See 
[Permissions Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG896) in the Java Platform, Standard Edition Deployment Guide for more information.</p></li>
<li><p>The <tt>Codebase</tt> attribute is used to ensure that the code base of the JAR file is restricted to specific domains. Use this attribute to prevent someone from re-deploying your application on another website for malicious purposes. See
[Codebase Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG897) in the Java Platform, Standard Edition Deployment Guide for more information.</p></li>
<li><p>The <tt>Application-Name</tt> attribute is used to provide the title that is shown in the security prompts for signed applications. See
[Application-Name Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG899) in the Java Platform, Standard Edition Deployment Guide for more information.</p></li>
<li><p>The <tt>Application-Library-Allowable-Codebase</tt> attribute is used to identify the locations where your application is expected to be found. Use this attribute to reduce the number of locations shown in the security prompt when the JAR file is in a different location than the JNLP file or the HTML page. See
[Application-Library-Allowable-Codebase Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG900) in the Java Platform, Standard Edition Deployment Guide for more information.</p></li>
<li><p>The <tt>Caller-Allowable-Codebase</tt> attribute is used to identify the domains from which JavaScript code can make calls to your application. Use this attribute to prevent unknown JavaScript code from accessing your application. See
[Caller-Allowable-Codebase Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG901) in the Java Platform, Standard Edition Deployment Guide for more information.</p></li>
<li><p>The <tt>Entry-Point</tt> attribute is used to identify the classes that are allowed to be used as entry points to your RIA. Use this attribute to prevent unauthorized code from being run from other available entry points in the JAR file. See
[Entry-Point Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG902) in the Java Platform, Standard Edition Deployment Guide for more information.</p></li>
<li><p>The <tt>Trusted-Only</tt> attribute is used to prevent untrusted components from being loaded. See
[Trusted-Only Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG903) in the Java Platform, Standard Edition Deployment Guide for more information.</p></li>
<li><p>The <tt>Trusted-Library</tt> attribute is used to allow calls between privileged Java code and sandbox Java code without prompting the user for permission. See
[Trusted-Library Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG904) in the Java Platform, Standard Edition Deployment Guide for more information.</p></li>

See 
[Modifying a Manifest File](modman.html) for information on adding these attributes to the manifest file.
