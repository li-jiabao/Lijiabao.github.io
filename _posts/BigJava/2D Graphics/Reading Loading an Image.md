
# Reading/Loading an Image

When you think of digital images, you probably think of sampled image formats such as the JPEG image format used in digital photography, or GIF images commonly used on web pages. All programs that can use these images must first convert them from that external format into an internal format.

Java 2D supports loading these external image formats into its `BufferedImage` format using its Image I/O API which is in the `javax.imageio` package. Image I/O has built-in support for GIF, PNG, JPEG, BMP, and WBMP. Image I/O is also extensible so that developers or administrators can "plug-in" support for additional formats. For example, plug-ins for TIFF and JPEG 2000 are separately available.

To load an image from a specific file use the following code, which is from
[`LoadImageApp.java`](examples/LoadImageApp.java):

```

BufferedImage img = null;
try {
    img = ImageIO.read(new File("strawberry.jpg"));
} catch (IOException e) {
}

```

Image I/O recognises the contents of the file as a JPEG format image, and decodes it into a `BufferedImage` which can be directly used by Java 2D.

`LoadImageApp.java` shows how to display this image.

If the code is running in an applet, then its just as easy to obtain the image from the applet codebase. The following excerpt is from
[`LoadImageApplet.java`](examples/LoadImageApplet.java):

```

try {
    URL url = new URL(getCodeBase(), "examples/strawberry.jpg");
    img = ImageIO.read(url);
} catch (IOException e) {
}

```

The `getCodeBase` method used in this example returns the URL of the directory containing this applet when the applet is deployed on a web server. If the applet is deployed locally, `getCodeBase` returns null and the applet will not run.

The following example shows how to use the `getCodeBase` method to load the `strawberry.jpg` file.

<applet code="LoadImageApplet" archive="examples/lib/LoadImageApplet.jar" width="200" height="200" alt="LoadImageApplet example"><param name="permissions" value="sandbox" /></applet>


[`LoadImageApplet.java`](examples/LoadImageApplet.java) contains the complete code for this example and this applet requires the [`strawberry.jpg`](examples/strawberry.jpg) image file.

In addition to reading from files or URLS, Image I/O can read from other sources, such as an <tt>InputStream</tt>. `ImageIO.read()` is the most straightforward convenience API for most applications, but the `javax.imageio.ImageIO` class provides many more static methods for more advanced usages of the Image I/O API. The collection of methods on this class represent just a subset of the rich set of APIs for discovering information about the images and for controlling the image decoding (reading) process.

We will explore some of the other capabilities of Image I/O later in the
[Writing/Saving an Image](saveimage.html) section.
