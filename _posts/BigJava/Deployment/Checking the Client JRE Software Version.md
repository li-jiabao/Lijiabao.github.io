
# Checking the Client JRE Software Version

There are many reasons to check if a particular version of the Java Runtime Environment (JRE) software is available on a client machine. For example, you might want to launch a different version of your rich Internet application (RIA) or redirect the user to a different page depending on the client's JRE software version.

Use the Deployment Toolkit script's `versionCheck` function to check if a particular version or range of JRE versions is installed on the client.

**Function signature:** `versionCheck: function(versionPattern)`

Parameters:

- `versionPattern` &#8211; String specifying the version or range of versions to check for, such as such as "1.4", "1.5.0*" (1.5.x family), and "1.6.0_02+" (any version greater than or equal to 1.6.0_02).

**Usage:** Creating a different user experience depending on the client's JRE software version

In this example, a Launch button is created for the Notepad application only if the version of JRE software on the client is greater than or equal to 1.6. If not, the browser is redirected to `oracle.com`.

```
   
&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;
    if (deployJava.versionCheck('1.6+')) {            
        var url = "https://docs.oracle.com/javase/tutorialJWS/deployment/webstart/examples/Notepad.jnlp";
        
        &lt;!-- you can also invoke deployJava.runApplet here --&gt;
        deployJava.createWebStartLaunchButton(url, '1.6.0'); 
    } else {
        document.location.href="http://oracle.com";
    }
&lt;/script&gt;         

```
