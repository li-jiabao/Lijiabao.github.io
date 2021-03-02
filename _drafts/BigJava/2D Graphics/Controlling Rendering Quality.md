
# Controlling Rendering Quality

Use the 
[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html) class rendering hints attribute to specify whether you want objects to be rendered as quickly as possible or whether you prefer that the rendering quality be as high as possible.

To set or change the rendering hints attribute in the `Graphics2D` context, construct a 
[`RenderingHints`](https://docs.oracle.com/javase/8/docs/api/java/awt/RenderingHints.html) object and pass it into `Graphics2D` by using the `setRenderingHints` method. If you just want to set one hint, you can call `Graphics2D` `setRenderingHint` and specify the key-value pair for the hint you want to set. (The key-value pairs are defined in the `RenderingHints` class.)

For example, to set a preference for antialiasing to be used if possible, you could use `setRenderingHint`:

```

public void paint (graphics g){
    Graphics2D g2 = (Graphics2D)g;
    RenderingHints rh = new RenderingHints(
             RenderingHints.KEY_TEXT_ANTIALIASING,
             RenderingHints.VALUE_TEXT_ANTIALIAS_ON);
    g2.setRenderingHints(rh);
...
}

```

`RenderingHints` supports the following types of hints:
<th id="h1" valign="top">Hint</th><th id="h2" valign="top">Key</th><th id="h3" valign="top">Values</th>
<td headers="h1" valign="top">**Antialiasing**</td><td headers="h2" valign="top">`KEY_ANTIALIASING`</td><td headers="h3" valign="top">`VALUE_ANTIALIAS_ON`<br />`VALUE_ANTIALIAS_OFF`<br />`VALUE_ANTIALIAS_DEFAULT`</td>
<td headers="h1" valign="top">**Alpha Interpolation**</td><td headers="h2" valign="top">`KEY_ALPHA_INTERPOLATION`</td><td headers="h3" valign="top">`VALUE_ALPHA_INTERPOLATION_QUALITY`<br />`VALUE_ALPHA_INTERPOLATION_SPEED`<br />`VALUE_ALPHA_INTERPOLATION_DEFAULT`</td>
<td headers="h1" valign="top">**Color Rendering**</td><td headers="h2" valign="top">`KEY_COLOR_RENDERING`</td><td headers="h3" valign="top">`VALUE_COLOR_RENDER_QUALITY`<br />`VALUE_COLOR_RENDER_SPEED`<br />`VALUE_COLOR_RENDER_DEFAULT`</td>
<td headers="h1" valign="top">**Dithering**</td><td headers="h2" valign="top">`KEY_DITHERING`</td><td headers="h3" valign="top">`VALUE_DITHER_DISABLE`<br />`VALUE_DITHER_ENABLE`<br />`VALUE_DITHER_DEFAULT`</td>
<td headers="h1" valign="top">**Fractional Text Metrics**</td><td headers="h2" valign="top">`KEY_FRACTIONALMETRICS`</td><td headers="h3" valign="top">`VALUE_FRACTIONALMETRICS_ON`<br />`VALUE_FRACTIONALMETRICS_OFF`<br />`VALUE_FRACTIONALMETRICS_DEFAULT`</td>
<td headers="h1" valign="top">**Image Interpolation**</td><td headers="h2" valign="top">`KEY_INTERPOLATION`</td><td headers="h3" valign="top">`VALUE_INTERPOLATION_BICUBIC`<br />`VALUE_INTERPOLATION_BILINEAR`<br />`VALUE_INTERPOLATION_NEAREST_NEIGHBOR`</td>
<td headers="h1" valign="top">**Rendering**</td><td headers="h2" valign="top">`KEY_RENDERING`</td><td headers="h3" valign="top">`VALUE_RENDER_QUALITY`<br />`VALUE_RENDER_SPEED`<br />`VALUE_RENDER_DEFAULT`</td>
<td headers="h1" valign="top">**Stroke Normalization Control**</td><td headers="h2" valign="top">`KEY_STROKE_CONTROL`</td><td headers="h3" valign="top">`VALUE_STROKE_NORMALIZE`<br />`VALUE_STROKE_DEFAULT`<br />`VALUE_STROKE_PURE`</td>
<td headers="h1" valign="top">**Text Antialiasing**</td><td headers="h2" valign="top">`KEY_TEXT_ANTIALIASING`</td><td headers="h3" valign="top">`VALUE_TEXT_ANTIALIAS_ON`<br />`VALUE_TEXT_ANTIALIAS_OFF`<br />`VALUE_TEXT_ANTIALIAS_DEFAULT`<br />`VALUE_TEXT_ANTIALIAS_GASP`<br />`VALUE_TEXT_ANTIALIAS_LCD_HRGB`<br />`VALUE_TEXT_ANTIALIAS_LCD_HBGR`<br />`VALUE_TEXT_ANTIALIAS_LCD_VRGB`<br />`VALUE_TEXT_ANTIALIAS_LCD_VBGR`<br /></td>
<td headers="h1" valign="top">**LCD Text Contrast**</td><td headers="h2" valign="top">`KEY_TEXT_LCD_CONTRAST`</td><td headers="h3" valign="top">Values should be a positive integer in the range 100 to 250. A lower value (eg 100) corresponds to higher contrast text when displaying dark text on a light background. A higher value (eg 200) corresponds to lower contrast text when displaying dark text on a light background. A typical useful value is in the narrow range 140-180. If no value is specified, a system or implementation default value will be applied.</td>

When a hint is set to default, the platform rendering default is used.
