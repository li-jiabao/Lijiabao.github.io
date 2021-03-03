---
title: Working with Bidirectional Text with the JTextComponent Class
author: lijiabao
date: 2020-12-06 01:51:35.038594600 +0800
category: Internationalization
categories: Internationalization
tags: Internationalization
---

# Working with Bidirectional Text with the JTextComponent Class

This section discusses how to work with bidirectional text with the
[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html) class. Bidirectional text is text that contains text that runs in two directions, left-to-right and right-to-left. An example of bidirectional text is Arabic text (which runs right-to-left) that contain numbers (which run left-to-right). It is more difficult to display and manage bidirectional text; however the
[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html) handles these issues for you.

The following topics are covered:

  - [Determining Directionality of Bidirectional Text](#directionality)
  - [Displaying and Moving Carets](#displaying-and-moving-carets)
  - [Hit Testing](#hit-testing)
  - [Highlighting Selections](#highlighting-selections)
  - [Setting Component Orientation](#setting-component-orientation)

For more information about these issues, or if you want more control in handling these issues, see
[Working with Bidirectional Text](../../2d/text/textlayoutbidirectionaltext.html) in the
[2D Graphics](../../2d/index.html) trail.

## <a name="directionality" id="directionality">Determining Directionality of Bidirectional Text</a>

The sample
[`BidiTextComponentDemo.java`](examples/BidiTextComponentDemo.java), which is based on  
[`TextComponentDemo.java`](../../uiswing/examples/components/index.html#TextComponentDemo), displays bidirectional text in a
[`JTextPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html) object. In most cases, the Java platform can determine the directionality of bidirectional Unicode text:

### Explicitly Specifying Text Run Direction in JTextComponent Objects

You can specify the run direction of the
[`Document`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html) object of a
[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html) object. For example, the following statement specifies that the text in the
[`JTextPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html) object `textPane` runs right-to-left:

```

textPane.getDocument().putProperty(
    TextAttribute.RUN_DIRECTION,
    TextAttribute.RUN_DIRECTION_RTL);

```

Alternatively, you can specify the component orientation of a particular Swing component based on locale. For example, the following statements specify that the component orientation of the object `textPane` is based on the ar-SA locale:

```

Locale arabicSaudiArabia = 
    new Locale.Builder().setLanguage("ar").setRegion("SA").build();

textPane.setComponentOrientation(
    ComponentOrientation.getOrientation(arabicSaudiArabia));

```

Because the run direction of the Arabic language is right-to-left, the run direction of the text contained in the `textPane` object is right-to-left also.

See the section [Setting Component Orientation](#setting-component-orientation) for more information.

## <a name="displaying-and-moving-carets" id="displaying-and-moving-carets">Displaying and Moving Carets</a>

In editable text, a **caret** is used to graphically represent the current insertion point, the position in the text where new characters will be inserted. In the
[`BidiTextComponentDemo.java`](examples/BidiTextComponentDemo.java) sample, the caret contains a small triangle that points toward the direction where an inserted character will be displayed.

By default, a
[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html) object creates a keymap (of type
[`Keymap`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Keymap.html)) that is shared by all
[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html) instances as the default keymap. A keymap lets an application bind key strokes to action. A default keymap (for
[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html) objects that support caret movement) includes the binding between caret movement forward and backward with the left and right arrow keys, which supports caret movement through bidirectional text.

## <a name="hit-testing" id="hit-testing">Hit Testing</a>

Often, a location in device space must be converted to a text offset. For example, when a user clicks the mouse on selectable text, the location of the mouse is converted to a text offset and used as one end of the selection range. Logically, this is the inverse of positioning a caret.

You can attach a caret listener to an instance of an
[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html). A caret listener enables you to handle caret events, which occur when the caret moves or when the selection in a text component changes. You attach a caret listener with the
[`addCaretListener`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#addCaretListener-javax.swing.event.CaretListener-) method. See
[How to Write a Caret Listener](../../uiswing/events/caretlistener.html) for more information.

## <a name="highlighting-selections" id="highlighting-selections">Highlighting Selections</a>

A selected range of characters is represented graphically by a highlight region, an area in which glyphs are displayed with inverse video or against a different background color.


[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html) objects implement logical highlighting. This means that the selected characters are always contiguous in the text model, and the highlight region is allowed to be discontiguous. The following is an example of logical highlighting:

## <a name="setting-component-orientation" id="setting-component-orientation">Setting Component Orientation</a>

Swing's layout managers understand how locale affects a UI; it is not necessary to create a new layout for each locale. For example, in a locale where text flows right to left, the layout manager will arrange components in the same orientation.

The sample
[`InternationalizedMortgageCalculator.java`](../format/examples/InternationalizedMortgageCalculator.java) has been localized for English, United States; English, United Kingdom; French, France; French, Canada; and Arabic, Saudi Arabia.

The following uses the en-US locale:

The following uses the ar-SA locale:

Note that the components have been laid out in the same direction as the corresponding locale: left-to-right for en-US and right-to-left for ar-SA. The
[`InternationalizedMortgageCalculator.java`](../format/examples/InternationalizedMortgageCalculator.java) sample calls the methods
[`applyComponentOrientation`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#applyComponentOrientation-java.awt.ComponentOrientation-) and
[`getOrientation`](https://docs.oracle.com/javase/8/docs/api/java/awt/ComponentOrientation.html#getOrientation-java.util.Locale-) to specify the direction of its components by locale:

```

private static JFrame frame;

// ...

private static void createAndShowGUI(Locale currentLocale) {

    // Create and set up the window.
    // ...
    // Add contents to the window.
    // ...
    frame.applyComponentOrientation(
        ComponentOrientation.getOrientation(currentLocale));
    // ...
  }

```

The sample
[`InternationalizedMortgageCalculator.java`](../format/examples/InternationalizedMortgageCalculator.java) requires the following resource files:

<li>
[`resources/Resources.properties`](../format/examples/resources/Resources.properties)</li>
<li>
[`resources/Resources_ar.properties`](../format/examples/resources/Resources_ar.properties)</li>
<li>
[`resources/Resources_fr.properties`](../format/examples/resources/Resources_fr.properties)</li>
