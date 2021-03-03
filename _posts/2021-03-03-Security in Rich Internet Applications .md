---
title: Security in Rich Internet Applications 
author: lijiabao
date: 2020-12-06 12:43:46.702514500 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Security in Rich Internet Applications 

The security model behind rich Internet applications (RIAs) works to protect the user from malicious Internet applications. This topic discusses security aspects that are common to applets and Java Web Start applications. See the following topics for more information:

<li>
[What Applets Can and Cannot Do](../applet/security.html)</li>
<li>
[Java Web Start and Security](../webstart/security.html)</li>

RIAs can be restricted to the Java security sandbox or request permission to access resources outside the sandbox. The first time an RIA is launched, the user is prompted for permission to run. The dialog shown provides information about the signer's certificate and indicates if the RIA requests permission to run outside the sandbox. The user can then make an informed decision about running the application.

Apply the following guidelines to help secure your RIAs.

<li>Sign the JAR file of the RIA with a certificate from a recognized certificate authority. For more information, see the 
[Signing and Verifying JAR Files](../jar/signindex.html) topic.</li>
<li>If the RIA requires access outside of the security sandbox, specify the `all-permissions` element in the JNLP file for the RIA. Otherwise, let the RIA default to running in the security sandbox. The following code snippet shows the `all-permissions` element in the RIA's JNLP file.
<pre><code>
&lt;security&gt;
   &lt;all-permissions/&gt;
&lt;/security&gt;
</code></pre>
If the applet tag is used, see 
[Deploying With the Applet Tag](../applet/html.html) for information on setting the permissions level.</li>
<li>A JNLP file can only include JAR files signed by the same certificate. If you have JAR files that are signed using different certificates, specify them in separate JNLP files. In the RIA's main JNLP file, specify the `component-desc` element to include the other JNLP files as component extensions. See 
[Structure of the JNLP File](../deploymentInDepth/jnlpFileSyntax.html) for information.</li>
<li>The security model for RIAs does not allow JavaScript code from a web page to invoke security-sensitive code in a signed JAR file unless you explicitly enable this. In the signed JAR file, wrap the section of code that you want JavaScript code to be able to invoke in a 
[`AccessController.doPrivileged`](https://docs.oracle.com/javase/8/docs/api/java/security/AccessController.html) block. This allows the JavaScript code to run with elevated permissions when executing the code in the `doPrivileged` code block.</li>
<li>Avoid mixing privileged and sandbox components in a RIA, if possible, as they can raise security warnings about mixed code. See 
[Mixing Privileged Code and Sandbox Code](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/mixed_code.html) for more information.</li>
<li>Include the `Permissions` and `Codebase` attributes in the JAR file manifest to ensure that your RIA requests only the permissions you specify, and that the RIA is accessed from the correct location. See 
[JAR File Manifest Attributes for Security ](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html) for information.</li>
<li>JAR file manifest attributes enable you to restrict access to your RIA and help to ensure that your code is not tampered with. See 
[Enhancing Security with Manifest Attributes](../jar/secman.html) for information on all of the JAR file manifest attributes that are available.</li>
