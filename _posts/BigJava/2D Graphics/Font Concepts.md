
# Font Concepts

This section introduces you to the 
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) class, which supports the specification of detailed font information and the use of sophisticated typographic features.

A 
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) object represents an instance of a font face from the collection of font faces available on the system. Examples of common font faces include Helvetica Bold and Courier Bold Italic. Three names are associated with a  

[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) object: its logical name, family name, and font face name:

  <li><p>A 

[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) object's** logical name** is a name mapped onto a physical font, which is one of the specific fonts available on the system. When specifying a 

[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html)
 in Java, use the **font face name** instead of the logical name. You can get the logical name from the `Font` by calling the 

[`getName`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html#getName--)
 method. To get a list of the logical names that are mapped onto the specific fonts available on a system, call the 

[java.awt.GraphicsEnvironment.getAvailableFontFamilyNames](https://docs.oracle.com/javase/8/docs/api/java/awt/GraphicsEnvironment.html#getAvailableFontFamilyNames--)
 method.</p>

<p>See [ **Physical and Logical Fonts**](fonts.html) for more information.
</p></li>

<li><p>A
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) object's **family name** is the name of the font family that determines the typographic design across several faces, such as Helvetica. Retrieve the family name through the 
[`getFamily`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html#getFamily--) method.</p></li>

<li><p>A  
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) object's **font face name** refers to an actual font installed on a system. This is the name you should use when specifying a font. It's often referred to as just the **font name**. Retrieve the font name by calling 
[`getFontName`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html#getFontName--). To determine which font faces are available on the system, call the  

[`java.awt.GraphicsEnvironment.getAllFonts`](https://docs.oracle.com/javase/8/docs/api/java/awt/GraphicsEnvironment.html#getAllFonts--)
 method.</p></li>


You can access information about a
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) through the 
[`getAttributes`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html#getAttributes--) method. A
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) objects's attributes include its name, size, transform, and font features such as weight and posture.


A
[`LineMetrics`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/LineMetrics.html) object encapsulates the measurement information associated with a
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html), such as its ascent, descent, and leading:

  - **Ascent** is the distance from the baseline to the ascender line. This distance represents the typical height of capital letters, but some characters might extend above the ascender line.

  - **Descent** is the distance from the baseline to the descender line. The lowest point of most characters will fall within the descent, but some characters might extend below the descender line.

  - **Leading** is the recommended distance from the bottom of the descender line to the top of the next line.


The following figure shows the position of the ascender line, baseline, and descender line:

This information is used to properly position characters along a line, and to position lines relative to one another.

You can access these line metrics through the
[`getAscent`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/LineMetrics.html#getAscent--),&#160;
[`getDescent`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/LineMetrics.html#getDescent--), and
[`getLeading`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/LineMetrics.html#getLeading--) methods. You can also access information about a
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) objects's height, baseline, and underline and strikethrough characteristics through the `LineMetrics` class.
