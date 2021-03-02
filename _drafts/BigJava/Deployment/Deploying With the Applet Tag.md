
# Deploying With the Applet Tag

If you are not sure whether your end users' browsers will have the JavaScript interpreter enabled, you can deploy your Java applet by manually coding the `&lt;applet&gt;` HTML tag, instead of using the Deployment Toolkit functions. Depending on the browsers you need to support, you may need to deploy your Java applet using the `&lt;object&gt;` or `&lt;embed&gt;` HTML tag. Check the 
[W3C HTML Specification](http://www.w3.org/TR/1999/REC-html401-19991224/) for details on the usage of these tags.

You can launch your applet using Java Network Launch Protocol (JNLP) or specify the launch attributes directly in the `&lt;applet&gt;` tag.

## Preparing for Deployment

Follow the steps described in the 
[Deploying An Applet](deployingApplet.html) topic to compile your source code, create and sign the JAR file, and create the JNLP file if necessary. The overall steps for deployment are still relevant. Only the contents of your HTML page containing the applet will change.

## Manually Coding Applet Tag, Launching Using JNLP

The 
[`AppletPage_WithAppletTag.html`](examples/dist/applet_ComponentArch_DynamicTreeDemo/AppletPage_WithAppletTagUsingJNLP.html) page deploys the Dynamic Tree Demo applet with an `&lt;applet&gt;` tag that has been manually coded (meaning, the applet is not deployed using the Deployment Toolkit which automatically generates the required HTML). The applet is still launched using JNLP. The JNLP file is specified in the `jnlp_href` attribute.

```

&lt;applet code = 'appletComponentArch.DynamicTreeApplet' 
        jnlp_href = 'dynamictree_applet.jnlp'
        width = 300
        height = 300 /&gt;

```

## Manually Coding Applet Tag, Launching Without JNLP

Using JNLP is the preferred way to deploy an applet, however, you can also deploy your applet without a JNLP file.

The 
[`AppletPage_WithAppletTagNoJNLP.html`](examples/dist/applet_ComponentArch_DynamicTreeDemo/AppletPage_WithAppletTagNoJNLP.html) deploys the Dynamic Tree Demo applet as shown in the following code snippet.

```

&lt;applet code = 'appletComponentArch.DynamicTreeApplet' 
    archive = 'DynamicTreeDemo.jar'
    width = 300
    height = 300&gt;
    &lt;param name="permissions" value="sandbox" /&gt;
&lt;/applet&gt;

```

where

- `code` is the name of the applet class.
- `archive` is the name of jar file containing the applet and its resources.
- `width` is the width of the applet.
- `height` is the height of the applet.
- `permissions` indicates if the applet runs in the security sandbox. Specify "sandbox" for the value to run in the sandbox. Specify "all-permissions" to run outside the sandbox. If the `permissions` parameter is not present, signed applets default to "all-permissions" and unsigned applets default to "sandbox".
