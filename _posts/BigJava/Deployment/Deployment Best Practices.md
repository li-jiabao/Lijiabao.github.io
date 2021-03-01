
# Deployment Best Practices

You can improve the user experience of your rich Internet application (RIA) using the best practices described in this topic.

<li>Sign the RIA using a certificate from a recognized certificate authority. Make sure that all artifacts are signed, and that the certificate has not expired. See
[Signing and Verifying JAR Files](../jar/signindex.html) for information on signing.</li>
<li>Request the minimum level of permissions that is needed. If the RIA does not require unrestricted access to a user's system, specify the permission level to be sandbox. See
[Security in Rich Internet Applications ](../doingMoreWithRIA/security.html) for more security guidelines.</li>
<li>Optimize the size of JAR files and related resources so that your RIA can load quickly. See 
[Reducing the Download Time](../deploymentInDepth/reducingDownloadTime.html) for optimization techniques.</li>
<li>Enable the version download protocol and use background update checks to enable your RIA to start quickly. See 
[Avoiding Unnecessary Update Checks](../deploymentInDepth/avoidingUnnecessaryUpdateChecks.html) to learn more about the version download protocol and update checks.</li>
<li>Make sure that the client has the required version of the Java Runtime Environment software. See 
[Ensuring the Presence of the JRE Software](../deploymentInDepth/ensuringJRE.html) for details on how the Deployment Toolkit script can be used for this purpose.</li>
<li>Embed the contents of your applet's JNLP file in the `&lt;applet&gt;` tag to avoid loading the JNLP file from the network. This feature was introduced in the Java SE 7 release. See 
[Embedding JNLP File in Applet Tag](../deploymentInDepth/embeddingJNLPFileInWebPage.html) to learn how to embed the contents of the applet's JNLP file in the web page.</li>
<li>Preload your Java Web Start application, if possible. If you plan to deploy your RIA as a Java Web Start application in an enterprise where you have some administrative control, you can preload your application to various clients so that it is cached and ready to use. Use the following command to preload your Java Web Start application:
<pre><code>
javaws -import -silent **&lt;jnlp url&gt;**
</code></pre>
</li>
