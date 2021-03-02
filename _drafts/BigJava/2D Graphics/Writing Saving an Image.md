
# Writing/Saving an Image

This lesson started with an explanation for using the `javax.imageio` package, to load images from an external image format into the internal <tt>BufferedImage</tt> format used by Java 2D. Then it explains how to use the `Graphics.drawImage()` to draw that image, with optional filtering.

The final stage is saving a `BufferedImage` object into an external image format. This may be an image that was originally loaded by the `Image I/O` class from an external image format and perhaps modified using the Java 2D APIs, or it may be one that was created by Java 2D.

The `Image I/O` class provides a simple way to save images in a variety of image formats in the following example:

```

static boolean ImageIO.write(RenderedImage im, 
                             String formatName,
                             File output)  throws IOException

```

.

The `formatName` parameter selects the image format in which to save the `BufferedImage`.

```

try {
    // retrieve image
    BufferedImage bi = getMyImage();
    File outputfile = new File("saved.png");
    ImageIO.write(bi, "png", outputfile);
} catch (IOException e) {
    ...
}

```

The `ImageIO.write` method calls the code that implements PNG writing a &#8220;PNG writer plug-in&#8221;. The term **plug-in** is used since `Image I/O` is extensible and can support a wide range of formats.

But the following standard image format plugins : JPEG, PNG, GIF, BMP and WBMP are always be present.

Each image format has its advantages and disadvantages:
<th id="h1">Format</th><th id="h2">Plus</th><th id="h3">Minus</th>
<td headers="h1">GIF</td><td headers="h2">Supports animation, and transparent pixels</td><td headers="h3">Supports only 256 colors and no translucency</td>
<td headers="h1">PNG</td><td headers="h2">Better alternative than GIF or JPG for high colour lossless images, supports translucency</td><td headers="h3">Doesn't support animation</td>
<td headers="h1">JPG</td><td headers="h2">Great for photographic images</td><td headers="h3">Loss of compression, not good for text, screenshots, or any application where the original image must be preserved exactly</td>

For most applications it is sufficient to use one of these standard plugins. They have the advantage of being readily available. The `Image I/O` class provides a way to plug in support for additional formats which can be used, and many such plug-ins exist. If you are interested in what file formats are available to load or save in your system, you may use the `getReaderFormatNames` and `getWriterFormatNames` methods of the `ImageIO` class. These methods return an array of strings listing all of the formats supported in this JRE.

```

String writerNames[] = ImageIO.getWriterFormatNames();

```

The returned array of names will include any additional plug-ins that are installed and any of these names may be used as a format name to select an image writer. The following code example is a simple version of a complete image edit/touch up program which uses a revised version of the 
[`ImageDrawingApplet.java`](examples/ImageDrawingApplet.java) sample program which can be used as follows :

- An image is first loaded via Image I/O
- The user selects a filter from the drop down list and a new updated image is drawn
- The user selects a save format from the drop down list
- Next a file chooser appears and the user selects where to save the image
- The modified image can now be viewed by other desktop applications

The complete code of this example is represented in 
[`SaveImage.java`](examples/SaveImage.java).

In this lesson you have learned just the basics of `Image I/O`, which provides extensive support for writing images, including working directly with an `ImageWriter` plug-in to achieve finer control over the encoding process. ImageIO can write multiple images, image metadata, and determine quality vs. size tradeoffs. For more information see 
[Java Image I/O API Guide](https://docs.oracle.com/javase/8/docs/technotes/guides/imageio/spec/title.fm.html).
