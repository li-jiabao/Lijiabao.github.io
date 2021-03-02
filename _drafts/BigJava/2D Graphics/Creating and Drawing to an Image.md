
# Creating and Drawing to an Image

We already know how to load an existing image, which was created and stored in your system or in any network location. But, you probably would like also to create an new image as a pixel data buffer.

In this case, you can create a `BufferedImage` object manually, using three constructors of this class:

- new BufferedImage(width, height, type) - constructs a `BufferedImage` of one of the predefined image types.
- new BufferedImage(width, height, type, colorModel) - constructs a `BufferedImage` of one of the predefined image types: `TYPE_BYTE_BINARY` or `TYPE_BYTE_INDEXED`.
- `new BufferedImage(colorModel, raster, premultiplied, properties)` - constructs a new `BufferedImage` with a specified `ColorModel` and `Raster`.

On the other hand, we can use methods of the `Component` class. These methods can analyze the display resolution for the given `Component` or `GraphicsConfiguration` and create an image of an appropriate type.

- `Component.createImage(width, height)`
- `GraphicsConfiguration.createCompatibleImage(width, height)`
- `GraphicsConfiguration.createCompatibleImage(width, height, transparency)`

GraphicsConfiguration returns an object of BufferedImage type, but the Component returns an object of `Image type`, if you need a BufferedImage object instead then you can perform an `instanceof` and cast to a `BufferedImage` in your code.

As was already mentioned in the previous lessons, we can render images not only on screen. An images itself can be considered as a drawing surface. You can use a `createGraphics()` method of the `BufferedImage` class for this purpose:

```

...

BufferedImage off_Image =
  new BufferedImage(100, 50,
                    BufferedImage.TYPE_INT_ARGB);

Graphics2D g2 = off_Image.createGraphics();

```

Another interesting use of offscreen images is an automatic**double buffering**. This feature allows to avoid flicker in animated images by drawing an image to a back buffer and then copying that buffer onto the screen instead of drawing directly to the screen.

Java 2D also allows access to hardware acceleration for offscreen images, which can provide the better performance of rendering to and copying from these images. You can get the benefit of this functionality by using the following methods of the `Image` class:

- The `getCapabilities` method allows you to determine whether the image is currently accelerated.
- The `setAccelerationPriority` method lets you set a hint about how important acceleration is for the image.
- The `getAccelerationPriority` method gets a hint about the acceleration importance.
