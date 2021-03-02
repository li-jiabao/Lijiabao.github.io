
# What You May Already Know About Networking in Java

The word **networking** strikes fear in the hearts of many programmers. Fear not! Using the networking capabilities provided in the Java environment is quite easy. In fact, you may be using the network already without even realizing it!

## Loading Applets from the Network

If you have access to a Java-enabled browser, you have undoubtedly already executed many applets. The applets you've run are referenced by a special tag in an HTML file &#151; the `&lt;APPLET&gt;` tag. Applets can be located anywhere, whether on your local machine or somewhere out on the Internet. The location of the applet is completely invisible to you, the user. However, the location of the applet is encoded within the `&lt;APPLET&gt;` tag. The browser decodes this information, locates the applet, and runs it. If the applet is on some machine other than your own, the browser must download the applet before it can be run.

This is the highest level of access that you have to the Internet from the Java development environment. Someone else has taken the time to write a browser that does all of the grunt work of connecting to the network and getting data from it, thereby enabling you to run applets from anywhere in the world.

**For more information:**<br />
[The "Hello World!" Application](../../getStarted/cupojava/index.html) shows you how to write your first applet and run it.

The 
[Java Applets](../../deployment/applet/index.html) trail describes how to write Java applets from A to Z.

## Loading Images from URLs

If you've ventured into writing your own Java applets and applications, you may have run into a class in the java.net package called URL. This class represents a Uniform Resource Locator and is the address of some resource on the network. Your applets and applications can use a URL to reference and even connect to resources out on the network. For example, to load an image from the network, your Java program must first create a URL that contains the address to the image.

This is the next highest level of interaction you can have with the Internet &#151; your Java program gets an address of something it wants, creates a URL for it, and then uses some existing function in the Java development environment that does the grunt work of connecting to the network and retrieving the resource.

**For more information:**<br />
[How to Use Icons](../../uiswing/components/icon.html) shows you how to load an image into your Java program (whether applets or applications) when you have its URL. Before you can load the image you must create a URL object with the address of the resource in it.


[Working with URLs](../urls/index.html), the next lesson in this trail, provides a complete discussion about URLs, including how your programs can connect to them and read from and write to that connection.
