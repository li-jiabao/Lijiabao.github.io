
# Using Text Attributes to Style Text

Applications typically need the capability to apply the following text attributes:

- **Underline** &#8211; A line that is drawn underneath text
- **Strikethrough** &#8211; A horizontal line that is drawn through the text
- **Superscript** or **Subscript** &#8211; A text or a letter that appears slightly above a line or correspondingly below a line
- **Kerning** &#8211; The adjustment of the space between characters

These and other text attributes can be applied by using the Java 2D `TextAttribute` class.

To apply these text attributes by add them to a `Font` object. For example:

```

Map&lt;TextAttribute, Object&gt; map =
    new Hashtable&lt;TextAttribute, Object&gt;();
map.put(TextAttribute.KERNING,
    TextAttribute.KERNING_ON);
font = font.deriveFont(map);
graphics.setFont(font);

```

The code example represented below shows the application of text attributes in the following order:

1. Sample string (no text attributes applied)
1. Kerning
1. Kerning and Underlining
1. Kerning,Underlining and Strikethrough
1. Kerning,Underlining, Strikethrough and Color

<applet code="AttributedText" archive="examples/lib/AttributedTextApplet.jar" alt="AttributedText applet" width="300" height="250"><param name="permissions" value="sandbox" /></applet>

The complete code for this applet is in 
[`AttributedText.java`](examples/AttributedText.java).
