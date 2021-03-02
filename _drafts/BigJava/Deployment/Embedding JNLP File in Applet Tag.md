
# Embedding JNLP File in Applet Tag

When applets are deployed by using the Java Network Launch Protocol (JNLP), the Java Plug-in software launches the applet after downloading the JNLP file from the network. Beginning in the Java SE 7 release, you can reduce the the time it takes for applets to launch, by embedding the JNLP file in the web page itself so that an additional network request can be avoided the first time the applet is loaded. This will result in applets launching quickly on the web browser.

A Base64 encoded JNLP file can be embedded in the `jnlp_embedded` parameter when deploying an applet in a web page. The attributes of the `&lt;jnlp&gt;` element should meet the following restrictions:

- The `href` attribute should contain a relative path.
- The `codebase` attribute should not be specified. This implies that the codebase will be derived from the URL of the web page in which the applet is loaded.

The following steps describe how to embed a JNLP file in a web page to deploy an applet.

<li>Create a 
[`<code>JNLP`</code>](examples/depl_EmbeddingJNLPInWebPage/src/dynamictree_applet.jnlp) file for your applet. A sample file is shown next.
<pre><code>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!-- href attribute contains relative path;
     codebase attribute not specified --&gt;
&lt;jnlp href="dynamictree_applet.jnlp"&gt;
    &lt;information&gt;
        &lt;title&gt;Dynamic Tree Demo&lt;/title&gt;
        &lt;vendor&gt;Dynamic Team&lt;/vendor&gt;
    &lt;/information&gt;
    &lt;resources&gt;
        &lt;!-- Application Resources --&gt;
        &lt;j2se version="1.7+" /&gt;
        &lt;jar href=
            "dist/applet_ComponentArch_DynamicTreeDemo/DynamicTreeDemo.jar" 
             main="true" /&gt;
    &lt;/resources&gt;
    &lt;applet-desc 
         name="Dynamic Tree Demo Applet"
         main-class="appletComponentArch.DynamicTreeApplet"
         width="300"
         height="300"&gt;
     &lt;/applet-desc&gt;
     &lt;update check="background"/&gt;
&lt;/jnlp&gt;
</code></pre>
</li>
<li>Encode the contents of the JNLP file using the Base64 scheme. You can use any Base64 encoding tool to encode the the JNLP file. Check the usage of the tool to create a string with Base64 encoding. Some examples of tools and web sites that may be used are as follows:
<ul>
1. UNIX commands &#8211; `base64`, `uuencode`
<li>Web sites &#8211; 
[Base64 Encode and Decode](http://base64encode.org/), 
[Base64 Encoder](http://www.opinionatedgeek.com/dotnet/tools/base64encode/)</li>
</ul>
</li>
<li>When deploying the applet in a web page, specify the `jnlp_embedded` parameter with it's value set to the Base64 encoded JNLP string. Make sure to include only the actual Base64 bytes without any encoding tool specific headers or footers.
<pre><code>
&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;
    var attributes = {} ;
    &lt;!-- Base64 encoded string truncated below for readability --&gt;
    var parameters = {jnlp_href: 'dynamictree_applet.jnlp',
        **jnlp_embedded: 'PCEtLSANCi8qDQogKiBDb ... bmxwPg=='**
    } ;
    deployJava.runApplet(attributes, parameters, '1.6');
&lt;/script&gt;
</code></pre>
Some encoding tools may wrap the encoded string into several 76-column lines. To use this multi-line attribute value in JavaScript code, specify the attribute value as a set of concatenated strings. You can include the multi-line attribute value as is if the applet is deployed directly with the `&lt;applet&gt;` HTML tag.
</li>

Open 
[`<code>AppletPage.html`</code>](examples/dist/depl_EmbeddingJNLPInWebPage/AppletPage.html) in a browser to view the Dynamic Tree Demo applet that is launched by using the JNLP file embedded in the web page.


[Download source code](examplesIndex.html#EmbeddedJNLP) for the **Embedded JNLP** example to experiment further.
