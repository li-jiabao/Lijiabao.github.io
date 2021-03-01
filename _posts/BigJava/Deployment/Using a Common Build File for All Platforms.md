
# Using a Common Build File for All Platforms

To generate a self-contained application bundle for each platform on which your application runs, you must run the packaging tool on each platform. You have the option of using platform-specific build files or setting up one build file that can be run on all platforms. Platform-specific files can be simpler to set up, but then you must maintain multiple files.

The File Association Demo described in 
[Using File Associations](../selfContainedApps/fileassociation.html) uses a single build file that works on all platforms. 

The following elements of the build file support its use for all platforms: 

  <li>The main class for the application is `ScriptRunnerApplication.java` for Linux and Windows and `ScriptRunnerApplicationMac.java` for OS X. The following code in the `-pre-init` task is used to determine which class to use:
<pre><code>
&lt;condition property="main.class" 
           value="sample.fa.ScriptRunnerApplication"
           else="sample.fa.ScriptRunnerApplicationMac"&gt;
    &lt;not&gt;&lt;os family="mac"/&gt;&lt;/not&gt;
&lt;/condition&gt;
</code></pre>
  </li>

    <li>The following code in the `-pre-init` task is used to prevent the main class for OS X from being compiled when running on Linux or Windows:
<pre><code>
&lt;condition property="excludes" value="**/*Mac.java"&gt;
    &lt;not&gt;&lt;os family="mac"/&gt;&lt;/not&gt;
&lt;/condition&gt;
</code></pre>
  </li>

  <li>&lt;fx:bundleArgument&gt; elements are used to pass arguments to the different bundlers available. Arguments that are not used by a bundler are ignored, so the build file can contain the arguments needed by all platforms. The following code defines arguments for Linux, OS X, and Windows:
<pre><code>
&lt;fx:bundleArgument arg="classpath" value="FileAssociationsDemo.jar lib/groovy-all-2.3.8.jar"/&gt;

&lt;fx:bundleArgument arg="win.exe.systemWide" value="true"/&gt;

&lt;fx:bundleArgument arg="linux.bundleName" value="file-association-demo"/&gt;
&lt;fx:bundleArgument arg="email" value="maintainer@example.com"/&gt;
&lt;fx:bundleArgument arg="mac.CFBundleName" value="File Assoc Demo"/&gt;
&lt;fx:bundleArgument arg="win.menuGroup" value="Java Demos"/&gt;
</code></pre>
</li>

See 
[`build.xml`](examples/packager_FileAssociations/build.xml) for the complete build code.

You can download the source files for the File Association Demo from 
[Self-Contained Application Examples](../selfContainedApps/examplesIndex.html).
