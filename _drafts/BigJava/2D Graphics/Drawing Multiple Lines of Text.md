
# Drawing Multiple Lines of Text

If you have a paragraph of styled text that you would like to fit within a specific width, you can use the `LineBreakMeasurer` class. This class enables styled text to be broken into lines so that they fit within a particular visual advance. Each line is returned as a `TextLayout` object, which represents unchangeable, styled character data. However, this class also enables access to layout information. The `getAscent` and `getDescent` methods of `TextLayout` return information about the font that is used to position the lines in the component. The text is stored as an `AttributedCharacterIterator` object so that the font and point size attributes can be stored with the text.

The following applet positions a paragraph of styled text within a component, using `LineBreakMeasurer`, `TextLayout` and `AttributedCharacterIterator`.

<applet code="LineBreakSample" archive="examples/lib/LineBreakSampleApplet.jar" width="400" height="250" alt="LineBreakSample applet"><param name="permissions" value="sandbox" /></applet>

The complete code for this applet is in 
[`<code>LineBreakSample.java`</code>](examples/LineBreakSample.java).

The following code creates an iterator with the string `vanGogh`. The start and end of the iterator is retrieved and a new `LineBreakMeasurer` is created from the iterator.

```

    AttributedCharacterIterator paragraph = vanGogh.getIterator();
    paragraphStart = paragraph.getBeginIndex();
    paragraphEnd = paragraph.getEndIndex();
    FontRenderContext frc = g2d.getFontRenderContext();
    lineMeasurer = new LineBreakMeasurer(paragraph, frc);

```

The size of the window is used to determine where the line should break. Also a `TextLayout` object is created for each line in the paragraph.

```

// Set break width to width of Component.
float breakWidth = (float)getSize().width;
float drawPosY = 0;
// Set position to the index of the first
// character in the paragraph.
lineMeasurer.setPosition(paragraphStart);

// Get lines from until the entire paragraph
// has been displayed.
while (lineMeasurer.getPosition() &lt; paragraphEnd) {

    TextLayout layout = lineMeasurer.nextLayout(breakWidth);

    // Compute pen x position. If the paragraph
    // is right-to-left we will align the
    // TextLayouts to the right edge of the panel.
    float drawPosX = layout.isLeftToRight()
        ? 0 : breakWidth - layout.getAdvance();

    // Move y-coordinate by the ascent of the
    // layout.
    drawPosY += layout.getAscent();

    // Draw the TextLayout at (drawPosX,drawPosY).
    layout.draw(g2d, drawPosX, drawPosY);

    // Move y-coordinate in preparation for next
    // layout.
    drawPosY += layout.getDescent() + layout.getLeading();
}

```

The `TextLayout` class is not frequently created directly by applications. However, this class is useful when applications need to work directly with text that has had styles (text attributes) applied at specific positions in text. For example, to draw a single word italicized in a paragraph, an application would need to perform measurements and set the font for each substring. If the text is bidirectional, this task is not so easy to do correctly. Creating a `TextLayout` object from an `AttributedString` object handles this problem for you. Consult the Java SE specification for more information about 
[`TextLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/TextLayout.html).
