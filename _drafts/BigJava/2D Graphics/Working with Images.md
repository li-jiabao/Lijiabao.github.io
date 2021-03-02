
# Lesson: Working with Images

As you have already learned from the
[Images](../overview/images.html) lesson, `Image`s are described by a width and a height, measured in pixels, and have a coordinate system that is independent of the drawing surface.

There are a number of common tasks when working with images.

- Loading an external GIF, PNG JPEG image format file into the internal image representation used by Java 2D.
- Directly creating a Java 2D image and rendering to it.
- Drawing the contents of a Java 2D image on to a drawing surface.
- Saving the contents of a Java 2D image to an external GIF, PNG, or JPEG image file.

This lesson teaches you the basics of loading, displaying, and saving images.

The are two main classes that you must learn about to work with images:

<li>The 
[`java.awt.Image`](https://docs.oracle.com/javase/8/docs/api/java/awt/Image.html) class is the superclass that represents graphical images as rectangular arrays of pixels.</li>
<li>The 
[`java.awt.image.BufferedImage`](https://docs.oracle.com/javase/8/docs/api/java/awt/image/BufferedImage.html) class, which extends the `Image` class to allow the application to operate directly with image data (for example, retrieving or setting up the pixel color). Applications can directly construct instances of this class.</li>

The `BufferedImage` class is a cornerstone of the Java 2D immediate-mode imaging API. It manages the image in memory and provides methods for storing, interpreting, and obtaining pixel data. Since `BufferedImage` is a subclass of `Image` it can be rendered by the `Graphics` and `Graphics2D` methods that accept an `Image` parameter.

A `BufferedImage` is essentially an `Image` with an accessible data buffer. It is therefore more efficient to work directly with `BufferedImage`. A `BufferedImage` has a **ColorModel** and a **Raster** of image data. The ColorModel provides a color interpretation of the image's pixel data.

The Raster performs the following functions:

- Represents the rectangular coordinates of the image
- Maintains image data in memory
- Provides a mechanism for creating multiple subimages from a single image data buffer
- Provides methods for accessing specific pixels within the image

The basic operations with images are represented in the following sections:

## [Reading/Loading an image](loadimage.html)

This section explains how to load an image from an external image format into a Java application using the Image I/O API

## [Drawing an image](drawimage.html)

This section teaches how to display images using the `drawImage` method of the `Graphics` and `Graphics2D` classes.

## [Creating and drawing To an image](drawonimage.html)

This section describes how to create an image and how to use the image itself as a drawing surface.

## [Writing/saving an image](saveimage.html)

This section explains how to save created images in an appropriate format.
