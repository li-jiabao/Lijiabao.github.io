
# Clipping the Drawing Region

Any 
[`Shape`](https://docs.oracle.com/javase/8/docs/api/java/awt/Shape.html) object can be used as a clipping path that restricts the portion of the drawing area that will be rendered. The clipping path is part of the 
[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html) context; to set the clip attribute, you call `Graphics2D.setClip` and pass in the `Shape` that defines the clipping path you want to use. You can shrink the clipping path by calling the `clip` method and passing in another `Shape`; the clip is set to the intersection of the current clip and the specified `Shape`.

## Example: ClipImage

This example animates a clipping path to reveal different portions of an image.


<applet code="ClipImage" alt="applet animates clipping path" archive="examples/lib/ClipImageApplet.jar" width="300" height="300">
   <param name="permissions" value="sandbox" />
</applet>


[`ClipImage.java`](examples/ClipImage.java) contains the complete code for this applet. The applet requires the [`clouds.jpg`](examples/images/clouds.jpg) image file.

The clipping path is defined by the intersection of an ellipse and a rectangle whose dimensions are set randomly. The ellipse is passed to the `setClip` method, and then `clip` is called to set the clipping path to the intersection of the ellipse and the rectangle.

```

private Ellipse2D ellipse = new Ellipse2D.Float();
private Rectangle2D rect = new Rectangle2D.Float();
...
ellipse.setFrame(x, y, ew, eh);
g2.setClip(ellipse);
rect.setRect(x+5, y+5, ew-10, eh-10);
g2.clip(rect);

```

## Example: Starry

A clipping area can also be created from a text string. The following example creates a `TextLayout` with the string *The Starry Night*. Then, it gets the outline of the `TextLayout`. The 
[`TextLayout.getOutline`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/TextLayout.html#getOutline-java.awt.geom.AffineTransform-) method returns a `Shape` object and a `Rectangle` is created from the bounds of this `Shape` object. The bounds contain all the pixels the layout can draw. The color in the graphics context is set to blue and the outline shape is drawn, as illustrated by the following image and code snippet.

```

FontRenderContext frc = g2.getFontRenderContext();
Font f = new Font("Helvetica", 1, w/10);
String s = new String("The Starry Night");
TextLayout textTl = new TextLayout(s, f, frc);
AffineTransform transform = new AffineTransform();
Shape outline = textTl.getOutline(null);
Rectangle r = outline.getBounds();
transform = g2.getTransform();
transform.translate(w/2-(r.width/2), h/2+(r.height/2));
g2.transform(transform);
g2.setColor(Color.blue);
g2.draw(outline);   

```

Next, a clipping area is set on the graphics context using the `Shape` object created from `getOutline`. The `starry.gif` image, which is Van Gogh's famous painting, *The Starry Night*, is drawn into this clipping area starting at the lower left corner of the `Rectangle` object.

```

g2.setClip(outline);
g2.drawImage(img, r.x, r.y, r.width, r.height, this);

```

<applet code="Starry" alt="applet showing Starry Night painting in clipping area" archive="examples/lib/Starry.jar" width="600" height="150"><param name="permissions" value="sandbox" /></applet>

<applet code="Starry" alt="applet showing Starry Night painting in clipping area" archive="examples/lib/StarryApplet.jar" width="600" height="150"><param name="permissions" value="sandbox" /></applet>


[`Starry.java`](examples/Starry.java) contains the complete code for this program. This applet requires the [`Starry.gif`](examples/images/Starry.gif) image file.
