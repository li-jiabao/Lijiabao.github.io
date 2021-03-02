
# Deploying Without Codebase

Beginning in the Java SE 7 release, you do not have to specify an absolute path for the `codebase` attribute in the Java Web Start application's Java Network Launch Protocol (JNLP) file. You can develop and test your applications in different environments without having to modify the path in the `codebase` attribute. If no codebase is specified, the Java Web Start software assumes that the codebase is relative to the web page from which the Java Web Start application is launched.

The following functions of the Deployment Toolkit script can be used to deploy Java Web Start applications in a web page when the JNLP file does not contain the `codebase` attribute:

- [`launchWebStartApplication`](#launchWebStartApplication) &#8211; Use this function in an HTML link to deploy your Java Web Start application.
- [`createWebStartLaunchButtonEx`](#createWebStartLaunchButtonEx) &#8211; Use this function to create a Launch button for your Java Web Start application.

<a id="launchWebStartApplication" name="launchWebStartApplication"></a> **Function signature:** `launchWebStartApplication: function(jnlp)`

Parameter:

`jnlp` &#8211; The path to the JNLP file containing deployment information for the Java Web Start application. This path can be relative to the web page in which the Java Web Start application is deployed.

Usage:

In the following example, the `launchWebStartApplication` function is invoked in the `href` attribute of an HTML `anchor (a)` tag.

The 
[`<code>dynamictree_webstart_no_codebase.jnlp`</code>](../webstart/examples/webstart_ComponentArch_DynamicTreeDemo/src/dynamictree_webstart_no_codebase.jnlp) JNLP file is used to deploy the Dynamic Tree Demo application.

```

&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;a href="javascript:deployJava.launchWebStartApplication('dynamictree_webstart_no_codebase.jnlp');"&gt;Launch&lt;/a&gt;

```

The Java Web Start application is launched when the user clicks the resulting HTML link.

<br />

<a id="createWebStartLaunchButtonEx" name="createWebStartLaunchButtonEx"></a> **Function signature:** `createWebStartLaunchButtonEx: function(jnlp)`

Parameter:

`jnlp` &#8211; The path to the JNLP file containing deployment information for the Java Web Start application. This path can be relative to the web page in which the Java Web Start application is deployed.

Usage:

The following example shows the usage of the `createWebStartLaunchButtonEx` function.

The 
[`<code>dynamictree_webstart_no_codebase.jnlp`</code>](../webstart/examples/webstart_ComponentArch_DynamicTreeDemo/src/dynamictree_webstart_no_codebase.jnlp) JNLP file is used to deploy the Dynamic Tree Demo application.

```

&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;        
    var jnlpFile = "dynamictree_webstart_no_codebase.jnlp";
    deployJava.createWebStartLaunchButtonEx(jnlpFile);
&lt;/script&gt;

```

The Java Web Start application is launched when the user clicks the resulting Launch button.

Open 
[`<code>JavaWebStartAppPage_No_Codebase.html`</code>](../webstart/examples/dist/webstart_ComponentArch_DynamicTreeDemo/JavaWebStartAppPage_No_Codebase.html) in a browser to view the Dynamic Tree Demo application that is deployed by using the functions described in this topic.

You can also launch the Java Web Start application at the system command prompt by invoking the `javaws` command with the complete url of the JNLP file as shown in the following code snippet.

```

javaws http://example.com/dynamictree_webstart_no_codebase.jnlp

```


[Download source code](../webstart/examplesIndex.html#DynamicTreeDemo) for the **Dynamic Tree Demo** example to experiment further.
