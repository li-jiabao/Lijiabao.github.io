
# Displaying Antialiased Text by Using Rendering Hints

Java 2D text rendering can be affected by **rendering hints**.

Recall that the most important text drawing method is the following:

```

Graphics.drawString(String s, int x, int y);

```

Usually, this method draws each glyph in a string of text with a solid color and each pixel that is &#8220;on&#8221; in that glyph is set to that colour. This type of drawing produces the highest contrast text, but sometimes with jagged (aliased) edges.

**Text antialiasing** is a technique used to smooth the edges of text on a screen. The Java 2D API enables applications to specify whether this technique should be used and what algorithm to use by applying a text rendering hint to the `Graphics`.

The most common rendering hint blends the foreground (text) color with the onscreen background pixels at the edges of the text. To request this hint an application must invoke the following:

```

graphics2D.setRenderingHint(
        RenderingHints.KEY_TEXT_ANTIALIASING,
        RenderingHints.VALUE_TEXT_ANTIALIAS_ON);

```

The following figure illustrates the antialiasing capability.

If used inappropriately this method can make the text appear overly fuzzy. In such cases, a better hint to use is the following:

```

graphics2D.setRenderingHint(
        RenderingHints.KEY_TEXT_ANTIALIASING,
        RenderingHints.VALUE_TEXT_ANTIALIAS_GASP);

```

This method automatically uses information in the font itself to decide whether to use antialiasing or to use solid colors.

LCD displays have a property that the Java 2D API can use to produce text that isn't as fuzzy as typical antialiasing but is more legible at small sizes. To request that text be drawn using the sub-pixel LCD text mode for a typical LCD display, an application must invoke the following:

```

graphics2D.setRenderingHint(
        RenderingHints.KEY_TEXT_ANTIALIASING,
        RenderingHints.VALUE_TEXT_ANTIALIAS_LCD_HRGB);

```

The code example represented below illustrates the antialiasing capability in the following order:

1. Antialiasing is off.
1. Antialiasing is on.
<li>Antialiasing using the `TEXT_ANTIALIAS_GASP` hint.
<hr />**Note:**&#160;Consequently the GASP table specifies to use only hinting at those sizes and not "smoothing". So in many cases the resulting text display is equivalent to `VALUE_TEXT_ANTIALIAS_OFF`.
<hr />
</li>
1. Antialiasing using the `TEXT_ANTIALIAS_LCD_HRGB` hint.

<applet code="AntialiasedText" archive="examples/lib/AntialiasedTextApplet.jar" alt="AntialiasedText applet" width="300" height="180"><param name="permissions" value="sandbox" /></applet>

The complete code for this applet is in 
[`AntialiasedText.java`](examples/AntialiasedText.java).
