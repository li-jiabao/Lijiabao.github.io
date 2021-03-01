
# Images

In the Java 2D API an image is typically a rectangular two-dimensional array of pixels, where each **pixel** represents the color at that position of the image and where the dimensions represent the horizontal extent (width) and vertical extent (height) of the image as it is displayed.

The most important image class for representing such images is the `java.awt.image.BufferedImage` class. The Java 2D API stores the contents of such images in memory so that they can be directly accessed.

Applications can directly create a `BufferedImage` object or obtain an image from an external image format such as PNG or GIF.

In either case, the application can then draw on to image by using Java 2D API graphics calls. So, images are not limited to displaying photographic type images. Different objects such as line art, text, and other graphics and even other images can be drawn onto an image (as shown on the following images).

The Java 2D API enables you to apply image filtering operations to `BufferedImage` and includes several built-in filters. For example, the `ConvolveOp` filter can be used to blur or sharpen images.

The resulting image can then be drawn to a screen, sent to a printer, or saved in a graphics format such as PNG, GIF etc. To learn more about images see the 
[Working with Images](../images/index.html) lesson.
