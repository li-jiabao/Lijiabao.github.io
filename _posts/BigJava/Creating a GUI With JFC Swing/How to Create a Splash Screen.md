
# How to Create a Splash Screen

Almost all modern applications have a splash screen. Typically splash screens are used for the following purposes:

- Advertising a product
- Indicating to the user that the application is launching during long startup times
- Providing information that is only needed once per visit

Java Foundation Classes, both Swing and Abstract Windowing Toolkit (AWT), enable a developer to create splash screens in Java technology applications. However, because the main purpose of a splash screen is to provide the user with feedback about the application's startup, the delay between the application's startup and the moment when the splash screen pops up should be minimal. Before the splash screen can pop up, the application has to load and initialize the Java&#8482; Virtual Machine (JVM), AWT, Swing, and sometimes application-dependent libraries as well. The resulting delay of several seconds has made the use of a Java&#8482; technology-based splash screen less than desirable.

Fortunately, Java&#8482; SE 6 provides a solution that allows the application to display the splash screen much earlier, even before the virtual machine starts. A Java application launcher is able to decode an image and display it in a simple non-decorated window.

The splash screen can display any `gif`, `png`, or `jpeg` image, with transparency, translucency, and animation. The figure below represents an example of the Java application splash screen developed as an animated `gif` file.

The 
[`SplashScreen`](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html) class is used to close the splash screen, change the splash-screen image, obtain the image position or size, and paint in the splash screen. An application cannot create an instance of this class. Only a single instance created within this class can exist, and this instance can be obtained using the `getSplashScreen()` static method. If the application has not created the splash screen at startup through the command-line or manifest-file option, the `getSplashScreen` method returns null.

Typically, a developer wants to keep the splash-screen image on the screen and display something over the image. The splash-screen window has an overlay surface with an alpha channel, and this surface can be accessed with a traditional `Graphics2D` interface.

The following code snippet shows how to obtain a `SplashScreen` object, then how to create a graphics context with the `createGraphics()` method:

```

...
        final SplashScreen splash = SplashScreen.getSplashScreen();
        if (splash == null) {
            System.out.println("SplashScreen.getSplashScreen() returned null");
            return;
        }
        Graphics2D g = splash.createGraphics();
        if (g == null) {
            System.out.println("g is null");
            return;
        }
...

```

Find the demo's complete code in the 
[`SplashDemo.java`](../examples/misc/SplashDemoProject/src/misc/SplashDemo.java) file.

The SplashDemo application uses fixed coordinates to display overlay information. These coordinates are image-dependent and calculated individually for each splash screen.

The native splash screen can be displayed in the following ways:

- Command-line argument
- Java&#8482; Archive (JAR) file with the specified manifest option

## <a name="cl" id="cl">How to Use the Command-Line Argument to Display a Splash Screen</a>

To display a splash screen from the command line use the `-splash:` command-line argument. This argument is a Java application launcher option that displays a splash screen:

```

java -splash:**&lt;file name&gt; &lt;class name&gt;**

```

<li>Compile the 
[`<code>SplashDemo.java`</code>](../examples/misc/SplashDemoProject/src/misc/SplashDemo.java) file.</li>
<li>Save the 
[`<code>splash.gif`</code>](../examples/misc/SplashDemoProject/src/misc/images/splash.gif) image in the `images` directory.</li>
<li>Run the application from the command line with the following arguments:
<pre><code>
java -splash:images/splash.gif SplashDemo
</code></pre>
</li>
1. Wait until the splash screen has been completely displayed.
1. The application window appears. To close the window choose File|Exit from the pop-up menu or click the X.
<li>Change the splash screen name to a non-existent image, for example, `nnn.gif`. Run the application as follows:
<pre><code>
java -splash:images/nnn.gif SplashDemo
</code></pre>
</li>
<li>You will see the following output string:
<pre><code>
SplashScreen.getSplashScreen() returned null
</code></pre>
</li>

## <a name="jar" id="jar">How to Use a JAR File to Display Splash Screen</a>

If your application is packaged in a JAR file, you can use the `SplashScreen-Image` option in a manifest file to show a splash screen. Place the image in the JAR file and specify the path in the option as follows:

```

Manifest-Version: 1.0
Main-Class: **&lt;class name&gt;**
SplashScreen-Image: **&lt;image name&gt;**

```

<li>Compile the 
[`<code>SplashDemo.java`</code>](../examples/misc/SplashDemoProject/src/misc/SplashDemo.java) file.</li>
<li>Save the 
[`<code>splash.gif`</code>](../examples/misc/SplashDemoProject/src/misc/images/splash.gif) image in the `images` directory.</li>
<li>Prepare the `splashmanifest.mf` file as follows:
<pre><code>
Manifest-Version: 1.0
Main-Class: SplashDemo
SplashScreen-Image: images/splash.gif
</code></pre>
</li>
<li>Create a JAR file using the following command:
<pre><code>
jar cmf splashmanifest.mf splashDemo.jar SplashDemo*.class images/splash.gif
</code></pre>
For more information about JAR files, see 
[Using JAR Files](../../deployment/jar/basicsindex.html) in the 
[Packaging Programs in JAR Files](../../deployment/jar/index.html) page.</li>
<li>Run the application:
<pre><code>
java -jar splashDemo.jar
</code></pre>
</li>
1. Wait until the splash screen has been completly displayed.
1. The application window appears. To close the window choose File|Exit from the pop-up menu or click the X.

## <a name="api" id="api">The Splash Screen API</a>

The `SplashScreen` class cannot be used to create the splash screen. Only a single instance created within this class can exist.
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[getSplashScreen()](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html#getSplashScreen--)</td><td headers="h2">Returns the `SplashScreen` object used for Java startup splash screen control.</td>
<td headers="h1">[createGraphics()](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html#createGraphics--)</td><td headers="h2">Creates a graphics context (as a `Graphics2D` object) for the splash screen overlay image, which allows you to draw over the splash screen.</td>
<td headers="h1">[getBounds()](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html#getBounds--)</td><td headers="h2">Returns the bounds of the splash screen window as a `Rectangle`.</td>
<td headers="h1">[close()](https://docs.oracle.com/javase/8/docs/api/java/awt/SplashScreen.html#close--)</td><td headers="h2">Closes the splash screen and releases all associated resources.</td>

## <a name="eg" id="eg">Example That Uses the SplashScreen API</a>

The following table lists the example that uses splash screen.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
