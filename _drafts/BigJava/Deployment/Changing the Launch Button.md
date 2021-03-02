
# Changing the Launch Button

You can change your Java Web Start application's Launch button image, if you don't like the default <img src="../../images/jws-launch-button.png" alt="Launch button" /> button or if you have another image that you have standardized on.

Use the `deployJava.launchButtonPNG` variable to point to the location of your Launch button's image.

**Variable:** `deployJava.launchButtonPNG`

**Usage:** Providing an alternate image URL

In this example, the Notepad application's Launch button is now an image of Duke waving.

```
          
&lt;script src="https://www.java.com/js/deployJava.js"&gt;&lt;/script&gt;
&lt;script&gt;
    deployJava.launchButtonPNG='https://docs.oracle.com/javase/tutorial/images/DukeWave.gif';
    var url = "https://docs.oracle.com/javase/tutorialJWS/deployment/webstart/examples/Notepad.jnlp";
    deployJava.createWebStartLaunchButton(url, '1.6.0');
&lt;/script&gt;

```

The Notepad application's new Launch button (Duke waving) follows. Click on Duke's image to launch the Notepad application.
