---
title: Deploying an Applet
author: lijiabao
date: 2020-12-06 12:44:02.659765100 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Deploying an Applet

You can deploy applets by using the `runApplet` function of the 
[Deployment Toolkit](https://www.java.com/js/deployJava.txt) script. The `runApplet` function ensures that the required minimum version of the Java Runtime Environment (JRE) software exists on the client and then runs the applet. The `runApplet` function generates an HTML `&lt;applet&gt;` tag with the information provided.

You can deploy applets by specifying the deployment options as attributes and parameters of the `&lt;applet&gt;` tag. You can also specify deployment options in a Java Network Launch Protocol (JNLP) file to take advantage of advanced features. See the 
[Java Network Launch Protocol](../deploymentInDepth/jnlp.html) topic for more information about this protocol.

If the client does not have the required minimum version of the JRE software, the Deployment Toolkit script redirects the browser to `http://www.java.com` to allow users to download the latest JRE software. On some platforms, users might be redirected before they can view the web page containing the applet.

The parameters to the `runApplet` function vary depending on whether you are using JNLP. Applets deployed by using JNLP can run only if the next generation Java Plug-in software exists on the client machine (the next generation Java Plug-in software was introduced in the Java Platform, Standard Edition 6 update 10 release).

The next section shows how to use the `runApplet` function in the HTML page that will display the applet. The following usage scenarios are described:

- [Specifying deployment options as attribute and parameter name-value pairs](#tagAttrsParams)
- [Using the `jnlp_href` parameter to specify deployment options in a JNLP file](#appletJnlp)
- [Specifying attribute and parameter name-value pairs **as well as** a JNLP file](#tagAndJnlp) (enables applet to run on the old and next generation Java Plug-in software)

**Function signature:** `runApplet: function(attributes, parameters, minimumVersion)`

Parameters:

- `attributes` &#8211; The names and values of the attributes of the generated `&lt;applet&gt;` tag
- `parameters` &#8211; The names and values of the `&lt;param&gt;` tags in the generated `&lt;applet&gt;` tag
- `minimumVersion` &#8211; The minimum version of the JRE software that is required to run this applet

Usage:

<li><a name="tagAttrsParams" id="tagAttrsParams"></a>Specifying deployment options as attribute and parameter name-value pairs
The attributes and parameters passed as name-value pairs are written out as attributes and nested `&lt;param&gt;` tags in the generated `&lt;applet&gt;` tag. Applets deployed in this manner can be run by the old Java Plug-in software.
<pre><code>
// launch the Java 2D applet on JRE version 1.6.0
// or higher with one parameter (fontSize)
&lt;script src=
    "https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;
    var attributes = {code:'java2d.Java2DemoApplet.class',
        archive:'Java2Demo.jar', width:710, height:540};
    var parameters = { fontSize:16, permissions:'sandbox' };
    var version = '1.6';
    deployJava.runApplet(attributes, parameters, version);
&lt;/script&gt;
</code></pre>

<p>Open 
[`<code>DeployUsingNameValuePairs.html`</code>](examples/dist/depltoolkit_Java2Demo/DeployUsingNameValuePairs.html) in a browser to view the the Java2D applet.</p>
<hr />**Note:** &#160;If you don't see the applet running, you need to install at least the [Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) release.<hr />
</li>
<li><a name="appletJnlp" id="appletJnlp"></a>Using the `jnlp_href` parameter to specify deployment options in a JNLP file
The attributes and parameters (`jnlp_href` in this case) passed as name-value pairs are written out as attributes and nested `&lt;param&gt;` tags in the generated `&lt;applet&gt;` tag. Applets deployed in this manner can be run by the next generation Java Plug-in software only. It is better to specify the applet's width and height as attributes as follows:
<pre><code>
&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt; 
    var attributes = { code:'java2d.Java2DemoApplet', width:710, height:540 }; 
    var parameters = { jnlp_href: 'java2d.jnlp' }; 
    deployJava.runApplet(attributes, parameters, '1.6'); 
&lt;/script&gt;
</code></pre>        

<p>Open 
[`<code>DeployUsingJNLP.html`</code>](examples/dist/depltoolkit_Java2Demo/DeployUsingJNLP.html) in a browser to view the the Java2D applet.</p>
<hr />**Note:** &#160;If you don't see the applet running, you need to install at least the [Java SE Development Kit (JDK) 6 update 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html) release.<hr />
</li>
<li><a name="tagAndJnlp" id="tagAndJnlp"></a>Specifying attribute and parameter name-value pairs **as well as** a JNLP file
Applets deployed by using JNLP will run only if end users have the next generation Java Plug-in software running on their browsers. If you would like your applet to run on the old Java Plug-in software also, specify deployment options using attribute and parameter name-value pairs **as well as** a JNLP file.
<pre><code>    
&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;  
    var attributes = {code:'java2d.Java2DemoApplet.class', 
            archive:'Java2Demo.jar', width:710, height:540}; 
    var parameters = { fontSize:16, jnlp_href:'java2d.jnlp' }; 
    var version = '1.6' ; 
    deployJava.runApplet(attributes, parameters, version);      
&lt;/script&gt;
</code></pre>
</li>

The following guidelines are helpful if some deployment options have different values in the attribute name-value pairs and in the JNLP file:

- Specify `width` and `height` as attribute name-value pairs (not in the JNLP file).
- Specify parameters such as `image` and `boxbgcolor` as parameter name-value pairs (not in the JNLP file). These parameters are needed early on in the applet startup process.
- In the JNLP file, leave the `codebase` attribute empty or specify an absolute URL. When the `codebase` attribute is left empty, it defaults to the directory containing the JNLP file.
- If the applet is launched using a JNLP file, the values for the `code`, `codebase`, and `archive` attributes are taken from the JNLP file. If these attributes are also specified separately as attribute name-value pairs, the attribute name-value pairs are ignored.

Open 
[`<code>DeployUsingNameValuePairsAndJNLP.html`</code>](examples/dist/depltoolkit_Java2Demo/DeployUsingNameValuePairsAndJNLP.html) in a browser to view the the Java2D applet.


[Download source code](examplesIndex.html#runApplet) for the **Run Applet** example to experiment further.
