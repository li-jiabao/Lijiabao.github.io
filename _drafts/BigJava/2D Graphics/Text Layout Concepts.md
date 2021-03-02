
# Text Layout Concepts

Before a piece of text can be displayed, it must be properly shaped and positioned using the appropriate glyphs and ligatures. This process is referred to as **text layout**. The text layout process involves the following:

  - Shaping text using the appropriate glyphs and ligatures
  - Properly ordering the text
  - Measuring and positioning the text

The information used to lay out text is also necessary for performing
text operations such as caret positioning, hit detection, and
highlighting. See
[Working with Bidirectional Text](../text/textlayoutbidirectionaltext.html) for more information about these text operations.

To develop software that can be deployed in international markets, text must be laid out in different languages in a way that conforms to the rules of the appropriate writing system.

This section covers the following topics:

  - [Shaping Text](#shaping_text)
  - [Ordering Text](#ordering_text)
  - [Measuring and Positioning Text](#measuring_and_positioning_text)

## <a name="shaping_text" id="shaping_text">Shaping Text</a>

A **glyph** is the visual representation of one or more characters. The shape, size, and position of a glyph is
dependent on its context. Many different glyphs can be used to represent a single character or combination of characters, depending on the font and style.

For example, in handwritten cursive text, a particular character can take on different shapes depending on how it is connected to adjacent
characters.


In some writing systems, particularly Arabic, the context of a glyph must always be taken into account. Unlike in English, cursive forms are mandatory in Arabic; it is unacceptable to present text without using cursive forms.


Depending on the context, these cursive forms can differ radically in shape. For example, the Arabic letter **heh** has the four cursive forms shown in the following figure:

Although these four forms are quite different from one another, such
cursive shape-changing is not fundamentally different from cursive
writing in English.



In some contexts, two glyphs can change shape even more radically and
merge to form a single glyph. This type of merged glyph is called a **ligature**. For example, most English fonts contain the ligature **fi** shown in the following figure:


The merged glyph takes into account the overhang on the letter **f** and combines the characters in a natural-looking way, instead of simply letting the letters collide.

Ligatures are also used in Arabic and the use of some ligatures is
mandatory; it is unacceptable to present certain character combinations
without using the appropriate ligature. When ligatures are formed from
Arabic characters, the shapes change even more radically than they do in
 English. For example, the following figure illustrates how two Arabic characters are combined into a single ligature when they appear together.


## <a name="ordering_text" id="ordering_text">Ordering Text</a>

In the Java programming language, text is encoded using Unicode character encoding. Text that uses Unicode character encoding is stored in memory in **logical order**. Logical order is the order in which characters and words are read and written. The logical order is not necessarily the same as the **visual order**, the order in which the corresponding glyphs are displayed.

The visual order for glyphs in a particular writing system (script) is called the **script order**. For example, the script order for Roman text is left-to-right and the script order for Arabic and Hebrew is right-to-left.

Some writing systems have rules in addition to script order for arranging glyphs and words on lines of text. For example, Arabic and Hebrew numbers run left to right, even though the letters run right to left. This means that Arabic and Hebrew, even with no embedded English text, are truly bidirectional. See
[Working with Bidirectional Text](textlayoutbidirectionaltext.html) for more information.

## <a name="measuring_and_positioning_text" id="measuring_and_positioning_text">Measuring and Positioning Text</a>


Unless you are working with a monospaced font, different characters in a
font have different widths. This means that all positioning and
measuring of text has to take into account exactly which characters are
used, not just how many. For example, to right-align a column of numbers
displayed in a proportional font, you can't simply use extra spaces to
position the text. To properly align the column, you need to know the
exact width of each number so that you can adjust accordingly.



Text is often displayed using multiple fonts and styles, such as bold or
 italic. In this case, even the same character can have different shapes
 and widths, depending on how it is styled. To properly position,
measure, and render text, you need to keep track of each individual
character **and** the style applied to that character. Fortunately, the
[`TextLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/font/TextLayout.html) class does this for you.



To properly display text in languages such as Hebrew and Arabic, each
individual character needs to be measured and positioned within the
context of neighboring characters. Because the shapes and positions of
the characters can change depending on the context, measuring and
positioning such text without taking the context into account produces
unacceptable results.


In addition, Java SE provides you with the
[`FontMetrics`](https://docs.oracle.com/javase/8/docs/api/java/awt/FontMetrics.html) class, which enables you to obtain measurements of text rendered by a
[`Font`](https://docs.oracle.com/javase/8/docs/api/java/awt/Font.html) object such as the height of a line of text in the font. You can use this
information to precisely position text in Java graphical applications. See
[Measuring Text](measuringtext.html) for more information.
