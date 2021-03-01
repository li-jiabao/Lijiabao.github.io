
# Displaying Short Status Strings

All browsers allow Java applets to display a short status string. All Java applets on the page, as well as the browser itself, share the same status line.

Never put crucial information in the status line. If many users might need the information, display that information within the applet area. If only a few, sophisticated users might need the information, consider sending the information to standard output (see 
[Writing Diagnostics to Standard Output and Error Streams](stdout.html)).

The status line is not usually very prominent, and it can be overwritten by other applets or by the browser. For these reasons, it is best used for incidental, transitory information. For example, an applet that loads several image files might display the name of the image file it is currently loading.

Applets display status lines with the 
[`showStatus`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html#showStatus-java.lang.String-) method, inherited in the `JApplet` class from the `Applet` class.

Here is an example of its use:

```

showStatus("MyApplet: Loading image file " + file);

```
