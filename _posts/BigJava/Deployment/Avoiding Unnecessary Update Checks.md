
# Avoiding Unnecessary Update Checks

Rich Internet applications (RIAs) are cached locally to improve startup time. However, before launching a RIA, the launch software checks to make sure that every JAR file referenced in the RIA's Java Network Launch Protocol (JNLP) file is up-to-date. In other words, the launch software makes sure that you are running the latest version of the RIA and not an older cached copy. These update checks can take up to a few hundred milliseconds depending on the number of JAR files and network speed. Use the techniques described in this topic to avoid unnecessary update checks and to enhance the start up time of your RIA.

The term "launch software" is used here to collectively refer to the Java Plug-in software and the Java Web Start software. The Java Plug-in software launches applets while the Java Web Start software launches Java Web Start applications.

## Leveraging the Version Download Protocol

You can leverage the **version download protocol** to eliminate unnecessary version checks. See the following steps to enable this protocol.

<li>Rename the JAR files to include a version number suffix with the following naming convention:
<pre><code>    
    **&lt;JAR file name&gt;**__V**&lt;version number&gt;**.jar
</code></pre>
For example, rename `DynamicTreeDemo.jar` to `DynamicTreeDemo__V1.0.jar`.</li>
<li>In the JNLP file, specify a version for every JAR file, and set the `jnlp.versionEnabled` property to `true`.
<pre><code>
&lt;resources&gt;
    &lt;!-- Application Resources --&gt;
    &lt;j2se version="1.6+"
        href="http://java.sun.com/products/autodl/j2se"
            max-heap-size="128m" /&gt;
    &lt;jar href="DynamicTreeDemo.jar"
        main="true" **version="1.0"**/&gt;   
    &lt;jar href="SomeOther.jar" version="2.0"/&gt;
    <b>&lt;property name="jnlp.versionEnabled"
        value="true"/&gt;</b>
    &lt;!-- ... --&gt;
&lt;/resources&gt;
</code></pre>
When the `jnlp.versionEnabled` property is enabled, the launch software performs only **one** update check to make sure that the JNLP file is up-to-date. The software compares the version numbers that are specified in the JNLP file with the corresponding JAR file versions (according to the naming convention mentioned in step 1) and updates only the outdated JAR files. This approach is efficient because only the update check for the JNLP file occurs over the network. All other version checks occur locally.
If a file with the correct version number is not found, the launch software attempts to load the default JAR file (for example, `DynamicTreeDemo.jar`).
</li>

## Performing Update Checks in the Background

If it is not critical for the user to immediately run the latest version of your RIA, you can specify that all update checks should occur in the background. In this case, the launch software launches the locally cached copy for immediate usage and downloads a newer version of the RIA in the background. The newer version of the RIA will be launched the next time the user attempts to use your RIA. To enable background update checks, add the following line to your JNLP file:

```

&lt;update check='background'/&gt;

```

The following code snippet shows a sample JNLP file with the background update check enabled:

```

&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;jnlp spec="1.0+" codebase="" href=""&gt;
    &lt;information&gt;
        &lt;title&gt;Applet Takes Params&lt;/title&gt;
        &lt;vendor&gt;Dynamic Team&lt;/vendor&gt;
    &lt;/information&gt;
    &lt;resources&gt;
        &lt;!-- Application Resources --&gt;
        &lt;j2se version="1.6+" href=
            "http://java.sun.com/products/autodl/j2se"/&gt;
        &lt;jar href="applet_AppletWithParameters.jar"
            main="true" /&gt;
    &lt;/resources&gt;
    &lt;applet-desc 
         name="Applet Takes Params"
         main-class="AppletTakesParams"
         width="800"
         height="50"&gt;
             &lt;param name="paramStr" value="someString"/&gt;
             &lt;param name="paramInt" value="22"/&gt;
     &lt;/applet-desc&gt;
     **&lt;update check="background"/&gt;**
&lt;/jnlp&gt;

```
