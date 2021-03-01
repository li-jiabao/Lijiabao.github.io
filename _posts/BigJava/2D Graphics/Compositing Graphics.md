
# Compositing Graphics

The 
[`AlphaComposite`](https://docs.oracle.com/javase/8/docs/api/java/awt/AlphaComposite.html) class encapsulates various compositing styles, which determine how overlapping objects are rendered. An `AlphaComposite` can also have an alpha value that specifies the degree of transparency: alpha&#160;=&#160;1.0 is totally opaque, alpha&#160;=&#160;0.0 totally transparent (clear). `AlphaComposite` supports most of the standard Porter-Duff compositing rules shown in the following table.

<th id="h1">Compositing Rule</th><th id="h2">Description</th>
<td headers="h1">Source-over (`SRC_OVER`)<br /><img src="../../figures/2d/2D-39.gif" width="173" height="72" alt="Source-over compositing" /></td><td headers="h2">If pixels in the object being rendered (the source) have the same location as previously rendered pixels (the destination), the source pixels are rendered over the destination pixels.</td>
<td headers="h1">Source-in (`SRC_IN`)<br /><img src="../../figures/2d/2D-40.gif" width="65" height="76" alt="Source-in compositing" /></td><td headers="h2">If pixels in the source and the destination overlap, only the source pixels in the overlapping area are rendered.</td>
<td headers="h1">Source-out (`SRC_OUT`)<br /><img src="../../figures/2d/2D-41.gif" width="84" height="93" alt="Source-out compositing" /></td><td headers="h2">If pixels in the source and the destination overlap, only the source pixels outside of the overlapping area are rendered. The pixels in the overlapping area are cleared.</td>
<td headers="h1">Destination-over (`DST_OVER`)<br /><img src="../../figures/2d/2D-42.gif" width="92" height="91" alt="Destination-over compositing" /></td><td headers="h2">If pixels in the source and the destination overlap, only the source pixels outside of the overlapping area are rendered. The pixels in the overlapping area are not changed.</td>
<td headers="h1">Destination-in (`DST_IN`)<br /><img src="../../figures/2d/2D-43.gif" width="80" height="74" alt="Destination-in compositing" /></td><td headers="h2">If pixels in the source and the destination overlap, the alpha from the source is applied to the destination pixels in the overlapping area. If the alpha = 1.0, the pixels in the overlapping area are unchanged; if the alpha is 0.0, pixels in the overlapping area are cleared.</td>
<td headers="h1">Destination-out (`DST_OUT`)<br /><img src="../../figures/2d/2D-44.gif" width="87" height="74" alt="Destination-out compositing" /></td><td headers="h2">If pixels in the source and the destination overlap, the alpha from the source is applied to the destination pixels in the overlapping area. If the alpha = 1.0, the pixels in the overlapping area are cleared; if the alpha is 0.0, the pixels in the overlapping area are unchanged.</td>
<td headers="h1">Clear (`CLEAR`)<br /><img src="../../figures/2d/2D-45.gif" width="90" height="85" alt="Clear with overlap compositing" /></td><td headers="h2">If the pixels in the source and the destination overlap, the pixels in the overlapping area are cleared.</td>

To change the compositing style used by the 
[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html) class, create an `AlphaComposite` object and pass it into the `setComposite` method.

## Example: Composite

This program illustrates the effects of various compositing style and alpha combinations.

<applet code="Composite" archive="examples/lib/CompositeApplet.jar" width="300" height="300" alt="applet showing compositing style and alpha combinations"><param name="permissions" value="sandbox" /></applet>


[`<code>Composite.java`</code>](examples/Composite.java). contains the full code for this applet.

A new `AlphaComposite` object **ac** is constructed by calling `AlphaComposite.getInstance` and specifying the desired compositing rule.

```

AlphaComposite ac =
  AlphaComposite.getInstance(AlphaComposite.SRC);

```

When a different compositing rule or alpha value is selected, `AlphaComposite.getInstance` is called again, and the new `AlphaComposite` is assigned to **ac**. The selected alpha is applied in addition to the per-pixel alpha value and is passed as a second parameter to `AlphaComposite`.`getInstance`.

```

ac = AlphaComposite.getInstance(getRule(rule), alpha);

```

The composite attribute is modified by passing the `AlphaComposite` object to `Graphics 2D` `setComposite`. The objects are rendered into a `BufferedImage` and are later copied to the screen, so the composite attribute is set on the `Graphics2D` context for the `BufferedImage`:

```

BufferedImage buffImg = new BufferedImage(w, h,
                        BufferedImage.TYPE_INT_ARGB);
Graphics2D gbi = buffImg.createGraphics();
...
gbi.setComposite(ac);

```
