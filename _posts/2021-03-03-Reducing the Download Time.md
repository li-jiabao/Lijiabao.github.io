---
title: Reducing the Download Time
author: lijiabao
date: 2020-12-06 12:44:31.144803000 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Reducing the Download Time

Rich Internet applications (RIAs) are downloaded from a web site when the user tries to access them. (RIAs can be cached after the initial download to improve performance). The time taken to download a RIA depends on the size of the RIA's JAR file. Larger JAR files take longer to download.

You can reduce the download time of your RIA by applying the following techniques:

<li>Compress your RIA's JAR file by using the 
[`pack200`](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/pack200.html) tool.</li>
- Remove unnecessary white space from the Java Network Launch Protocol (JNLP) file and the JavaScript files.
- Optimize images and animation.

The following steps describe how to create and deploy a compressed JAR file for a signed RIA.

<li>Normalize the JAR file using the `--repack` option.
This step ensures that the security certificate and JAR file will pass verification checks when the RIA is launched.
<pre><code>
pack200 --repack DynamicTreeDemo.jar
</code></pre>
</li>
<li>Sign the normalized JAR file.
<pre><code>
jarsigner -keystore myKeyStore DynamicTreeDemo.jar me
</code></pre>
where `myKeyStore` is the name of the keystore and `me` is the alias for the keystore.</li>
<li>Pack the signed JAR file
<pre><code>
pack200 DynamicTreeDemo.jar.pack.gz DynamicTreeDemo.jar    
</code></pre>
</li>
<li>Set the `jnlp.packEnabled` property to `true` in the RIA's JNLP file.
<pre><code>
&lt;resources&gt;    
    &lt;j2se version="1.6+"
        href="http://java.sun.com/products/autodl/j2se"
              max-heap-size="128m" /&gt;
    &lt;jar href="DynamicTreeDemo.jar"
        main="true"/&gt;
    <b>&lt;property name="jnlp.packEnabled"
        value="true"/&gt;</b>
    &lt;!-- ... --&gt;
&lt;/resources&gt;
</code></pre>    
</li>

When the `jnlp.packEnabled` property is set in the JNLP file, the Java Plug-in software looks for the compressed JAR file with the `.pack.gz` extension (for example, `DynamicTreeDemo.jar.pack.gz`). If found, the Java Plug-in software automatically unpacks and loads the JAR file. If a file with the `.pack.gz` extension is not found, then the Java Plug-in software attempts to load the regular JAR file (for example, `DynamicTreeDemo.jar`).
