
# The Synth Look and Feel

Creating a custom look and feel, or modifying an existing one, can be a daunting task. The 
[`javax.swing.plaf.synth`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/package-summary.html) package can be used to create a custom look and feel with much less effort. You can create a Synth look and feel either programatically or through the use of an external XML file. The discussion below is devoted to the creation of a Synth look and feel using an external XML file. Creating a Synth c programatically is discussed in the API documentation.

With the Synth look and feel, you provide the "look." Synth itself provides the "feel." Thus, you can think of the Synth L&amp;F as a "skin."

## The Synth Architecture

<!--Recall from the previous topic that it is the responsibility of each L&amp;F to provide a concrete 
implementation for each of the many `ComponentUI` subclasses defined by Swing. With the Synth L&amp;F, 
by comparison, you must create a 
[`SynthStyle`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/SynthStyle.html) for every component.
<p>
In order to use Synth you need to provide a 
[`SynthStyleFactory`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/SynthStyleFactory.html)and `SynthStyles`, either directly 
(that is, programatically) or indirectly (from an XML file). Synth itself creates the necessary 
`ComponentUI` implementations from the `SynthStyles`. 
<p>
Each `ComponentUI` implementation in Synth derives from one `SynthStyle` per 
[`Region`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/Region.html)&mdash;most components only have one `Region` and therefor only one 
`SynthStyle`. `SynthStyle` is 
 used to access all style related properties: fonts, colors and other component properties. 
 In addition `SynthStyles` are used to obtain 
[`SynthPainters`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/SynthPainter.html) for painting the background, 
 border, focus and other portions of a component. 
 <p>
The `ComponentUIs` obtain `SynthStyles` from the 
`getStyle(JComponent c, Region id)` method of a 
`SynthStyleFactory`. A `SynthStyleFactory` can be provided directly 
(programatically) by
<pre><code>
SynthLookAndFeel.setStyleFactory(javax.swing.plaf.synth.SynthStyleFactory)
</code></pre>
 or indirectly (external file) by:
<pre><code>
SynthLookAndFeel.load(java.io.InputStream, java.lang.Class)
</code></pre>
<p>When you use an external XML file, you define &lt;style&gt; elements which Synth maps to the `SynthStyles`.
<p>-->
 Recall from the previous topic that it is the responsibility of each L&amp;F to provide a concrete implementation for each of the many `ComponentUI` subclasses defined by Swing. The Synth L&amp;F takes care of this for you. To use Synth, you need not create any `ComponentUI`s&#8212;rather you need only specify how each component is painted, along with various properties that effect the layout and size.

```

SynthLookAndFeel.load(java.io.InputStream, java.lang.Class)

```

Synth operates at a more granular level than a component&#8212;this granular level is called a "region." Each component has one or more regions. Many components have only one region, such as `JButton`. Others have multiple regions, such as `JScrollBar`. Each of the `ComponentUIs` provided by Synth associates a `SynthStyle` with each of the regions defined by the `ComponentUI`. For example, Synth defines three regions for `JScrollBar`: the track, the thumb and the scroll bar itself. The `ScrollBarUI` (the `ComponentUI` subclass defined for `JScrollBar`) implementation for Synth associates a `SynthStyle` with each of these regions.

`SynthStyle` provides style information used by the Synth `ComponentUI` implementation. For example, `SynthStyle` defines the foreground and background color, font information, and so forth. In addition, each `SynthStyle` has a `SynthPainter` that is used to paint the region. For example, `SynthPainter` defines the two methods `paintScrollBarThumbBackground` and `paintScrollBarThumbBorder`, which are used to paint the scroll bar thumb regions.

Each of the `ComponentUIs` in Synth obtain `SynthStyles` using a `SynthStyleFactory`. There are two ways to define a `SynthStyleFactory`: through a Synth XML file, or programatically. The following code shows how to load an XML file dictating the look of Synth&#8212;beneath the covers this creates a `SynthStyleFactory` implementation populated with `SynthStyles` from the XML file:

```

  SynthLookAndFeel laf = new SynthLookAndFeel();
  laf.load(MyClass.class.getResourceAsStream("laf.xml"), MyClass.class);
  UIManager.setLookAndFeel(laf);

```

The programmatic route involves creating an implementation of `SynthStyleFactory` that returns `SynthStyles`. The following code creates a custom `SynthStyleFactory` that returns distinct `SynthStyles` for buttons and trees:

```

 class MyStyleFactory extends SynthStyleFactory {
     public SynthStyle getStyle(JComponent c, Region id) {
         if (id == Region.BUTTON) {
             return buttonStyle;
         }
         else if (id == Region.TREE) {
             return treeStyle;
         }
         return defaultStyle;
     }
 }
 SynthLookAndFeel laf = new SynthLookAndFeel();
 UIManager.setLookAndFeel(laf);
 SynthLookAndFeel.setStyleFactory(new MyStyleFactory());

```

## Using an External XML File


Choosing a Synth look and feel for your application is simple. This code loads the XML file into Synth from an input 
stream and then sets the current look and feel to Synth:
<pre><code>
SynthLookAndFeel laf = new SynthLookAndFeel();
laf.load(MyApp.class.getResourceAsStream("synth_def.xml"), MyApp.class);
UIManager.setLookAndFeel(laf);
</code></pre>
<p>
The Synth feel (or, "skin"), is defined in the external file, `synth_def.xml`, which must be loaded 
as shown. The second parameter in the `load()` method defines the path to the XML file and any images used, 
indicating that they are 
to be found with the same path as the `MyApp.class` file.
<p>
<hr />**Note:**&nbsp;It is also possible to load a Synth XML file from an arbitrary URL:
<pre><code>
SynthLookAndFeel laf = new SynthLookAndFeel();
laf.load(new URL("file:///C:/java/synth/laf/synth_def.xml"));
UIManager.setLookAndFeel(laf);
</code></pre>
<hr />

<p>
The `load()` method throws a checked exception, `ParseException`, so the 
`laf.load(MyApp.class.getResourceAsStream("synth_def.xml"), MyApp.class);` 
statement should be in a `try` block. A more robust example is this `initLookAndFeel()` 
method in the `MyApp` class that will override the default look and feel to 
create a Synth look and feel from the external XML file.:

<pre><code>
private static void initLookAndFeel() {
   SynthLookAndFeel laf = new SynthLookAndFeel();
 
   try {
      laf.load(MyApp.class.getResourceAsStream("synth_def.xml"), MyApp.class);
      UIManager.setLookAndFeel(laf);
   } 

   catch (ParseException e) {
      System.err.println("Couldn't get specified look and feel ("
                                   + lookAndFeel + "), for some reason.");
      System.err.println("Using the default look and feel.");
      e.printStackTrace();
   }

}
</code></pre>
If the load method fails, or the XML file is not well formed, the Synth look and feel will not be installed and 
you will get the default look and feel.
-->
<h2>The XML File</h2>
<p>An explanation of the DTD for the Synth XML file can be found at 
[`javax.swing.plaf.synth/doc-files/synthFileFormat.html`.](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/doc-files/synthFileFormat.html)

```

SynthLookAndFeel laf = new SynthLookAndFeel();
laf.load(new URL("file:///C:/java/synth/laf/synth_def.xml"));
UIManager.setLookAndFeel(laf);

```

When you load a Synth look and feel, only those GUI components (or regions) for which there is a definition (a "style" bound to the region, as discussed below) are rendered. There is no default behavior for any components&#8212;without style definitions in the Synth XML file, the GUI is a blank canvas.

To specify the rendering of a component (or region), your XML file must contain a &lt;style&gt; element, which is then *bound* to the region using the &lt;bind&gt; element. As an example, let's define a style that includes the font, foreground color, and background color, and then bind that style to all components. It is a good idea to include such an element in your Synth XML file while you are developing it&#8212;then, any components you haven't yet defined will at least have colors and a font:

```

&lt;synth&gt;
  &lt;style id="basicStyle"&gt;
    &lt;font name="Verdana" size="16"/&gt;
    &lt;state&gt;
      &lt;color value="WHITE" type="BACKGROUND"/&gt;
      &lt;color value="BLACK" type="FOREGROUND"/&gt;
    &lt;/state&gt;
  &lt;/style&gt;
  &lt;bind style="basicStyle" type="region" key=".*"/&gt;
&lt;/synth&gt;

```

Let's analyse this style definition:

<li>
The &lt;style&gt; element is the basic building block of the Synth XML file. It contains all the information needed to describe a region's rendering. A &lt;style&gt; element can describe more than one region, as is done here. In general, though, it is best to create a &lt;style&gt; element for each component or region. Note that the &lt;style&gt; element is given an identifier, the string "basicStyle." This identifier will be used later in the &lt;bind&gt; element.
</li>
<li>
The &lt;font&gt; element of the &lt;style&gt; element sets the font to Verdana, size 16.
</li>
<li>
The &lt;state&gt; element of the &lt;style&gt; element will be discussed below. The &lt;state&gt; element of a region can have one, or a mixture, of seven possible values. When the value is not specified, the definition applies to all states, which is the intention here. Therefore, the background and foreground colors "for all states" are defined in this element.
</li>
<li>
Finally, the &lt;style&gt; element with the identifier "basicStyle" that has just been defined is *bound* to all regions. The &lt;bind&gt; element binds "basicStyle" to "region" types. Which region type or types the binding applies to is given by the "key" attribute, which is ".*" in this case, the regular expression for "all."
</li>

Let's look at the pieces of the Synth XML file before creating some working examples. We'll start with the &lt;bind&gt; element, showing how a given &lt;style&gt; is applied to a component or region.

## The &lt;bind&gt; Element

Whenever a &lt;style&gt; element is defined, it must be bound to one or more components or regions before it has an effect. The &lt;bind&gt; element is used for this purpose. It requires three attributes:

<li>
`style` is the unique identifier of a previously defined style.
</li>
<li>
`type` is either "name" or "region." If `type` is a name, obtain the name with the `component.getName()` method. If `type` is a region, use the appropriate constant defined in the `Region` class in the `javax.swing.plaf.synth` package.
</li>
<li>
`key` is a regular expression used to determine which components or regions the style is bound to.
</li>

<!--
A "region" is a distinct rendering area of a Swing component. Some components have only one 
region, such as a button. Other components have more than one region&mdash;for example, a scrollbar has three region 
constants: SCROLL_BAR, SCROLL_BAR_THUMB, and SCROLL_BAR_TRACK.
-->

A Region is a way of identifying a component or part of a component. Regions are based on the constants in the 
[`Region`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/Region.html) class, modified by stripping out underscores:

For example, to identify the SPLIT_PANE region you would use SPLITPANE, splitpane, or SplitPane (case insensitive).

<!--
Here is a table of a few of the region constants defined in the `Region` class.
<p>

<table width=60% border=1 cellpadding=4 cellspacing=3>
<caption>**Some Region Constants and Their Synth Format**</caption>
<tr>
<th>
Region Constant
</th>
<th width=50%>
Synth File Format
</th>
</tr>
<code>
<tr>
<td>
ARROW_BUTTON 
</td>
<td>
ArrowButton
</td>
</tr>
<tr>
<td>
BUTTON 
</td>
<td>
Button
</td>
</tr>
<tr>
<td>
CHECK_BOX
</td>
<td>
CheckBox
</td>
</tr>
<tr>
<td>
CHECK_BOX_MENU_ITEM 
</td>
<td>
CheckBoxMenuItem
</td>
</tr>
<tr>
<td>
COLOR_CHOOSER
</td>
<td>
ColorChooser
</td>
</tr>
<tr>
<td>
COMBO_BOX
</td>
<td>
ComboBox
</td>
</tr>
<tr>
<td>
DESKTOP_ICON
</td>
<td>
DesktopIcon
</td>
</tr>
<tr>
<td>
DESKTOP_PANE 
</td>
<td>
DesktopPane
</td>
</tr>
<tr>
<td>
EDITOR_PANE
</td>
<td>
EditorPane
</td>
</tr>
<tr>
<td>
FILE_CHOOSER
</td>
<td>
FileChooser
</td>
</tr>
<tr>
<td>
FORMATTED_TEXT_FIELD
</td>
<td>
FormattedTextField
</td>
</tr>
</code>
</table>
-->

When you bind a style to a region, that style will apply to *all* of the components with that region. You can bind a style to more than one region, and you can bind more than one style to a region. For example,

```

&lt;style id="styleOne"&gt;
   &lt;!-- styleOne definition goes here --&gt;
&lt;/style&gt;

&lt;style id="styleTwo"&gt;
   &lt;!-- styleTwo definition goes here --&gt;
&lt;/style&gt;

&lt;bind style="styleOne" type="region" key="Button"/&gt;
&lt;bind style="styleOne" type="region" key="RadioButton"/&gt;
&lt;bind style="styleOne" type="region" key="ArrowButton"/&gt;

&lt;bind style="styleTwo" type="region" key="ArrowButton"/&gt;



```

You can bind to individual, named components, whether or not they are *also* bound as regions. For example, suppose you want to have the "OK" and "Cancel" buttons in your GUI treated differently than all the other buttons. First, you would give the OK and Cancel buttons names, using the `component.setName()` method. Then, you would define three styles: one for buttons in general (region = "Button"), one for the OK button (name = "OK"), and one for the Cancel button (name = "Cancel"). Finally, you would bind these styles like this:

```

&lt;bind style="styleButton" type="region" key="Button"&gt;
&lt;bind style="styleOK" type="name" key="OK"&gt;
&lt;bind style="styleCancel" type="name" key="Cancel"&gt;

```

As a result, the "OK" button is bound to *both* "styleButton" and "styleOK," while the "Cancel" button is bound to *both* "styleButton" and "styleCancel."

When a component or region is bound to more than one style, the styles are merged

Just as a style can be bound to multiple regions or names, multiple styles can be bound to a region or name. These multiple styles are merged for the region or name. Precedence is given to styles defined later in the file.

## The &lt;state&gt; Element

The &lt;state&gt; element allows you to define a look for a region that depends on its "state." For example, you will usually want a button that has been `PRESSED` to look different than the button in its `ENABLED` state. There are seven possible values for &lt;state&gt; that are defined in the Synth XML DTD. They are:

1. ENABLED
1. MOUSE_OVER
1. PRESSED
1. DISABLED
1. FOCUSED
1. SELECTED
1. DEFAULT

You can also have composite states, separated by 'and'&#8212;for example, ENABLED and FOCUSED. If you do not specify a value, the defined look will apply to all states.

As an example, here is a style that specifies painters per state. All buttons are painted a certain way, unless the state is "PRESSED," in which case they are painted differently:

```

&lt;style id="buttonStyle"&gt;
  &lt;property key="Button.textShiftOffset" type="integer" value="1"/&gt;
  &lt;insets top="10" left="10" right="10" bottom="10"/&gt;

  &lt;state&gt;
    &lt;imagePainter method="buttonBackground" path="images/button.png"
                         sourceInsets="10 10 10 10"/&gt;
  &lt;/state&gt;
  &lt;state value="PRESSED"&gt;
    &lt;color value="#9BC3B1" type="BACKGROUND"/&gt;
    &lt;imagePainter method="buttonBackground" path="images/button2.png"
                        sourceInsets="10 10 10 10"/&gt;
  &lt;/state&gt;
&lt;/style&gt;
&lt;bind style="buttonStyle" type="region" key="Button"/&gt;


```

Ignoring the &lt;property&gt; and &lt;insets&gt; elements for the moment, you can see that a pressed button is painted differently than an unpressed button.

The &lt;state&gt; value that is used is the defined state that most closely matches the state of the region. Matching is determined by the number of values that match the state of the region. If none of the state values match, then the state with no value is used. If there are matches, the state with the most individual matches will be chosen. For example, the following code defines three states:

```

&lt;state id="zero"&gt;
  &lt;color value="RED" type="BACKGROUND"/&gt;
&lt;/state&gt;
&lt;state value="SELECTED and PRESSED" id="one"&gt;
  &lt;color value="RED" type="BACKGROUND"/&gt;
&lt;/state&gt;
&lt;state value="SELECTED" id="two"&gt;
  &lt;color value="BLUE" type="BACKGROUND"/&gt;
&lt;/state&gt;

```

If the state of the region contains at least SELECTED and PRESSED, state one will be chosen. If the state contains SELECTED, but not does not contain PRESSED, state two will be used. If the state contains neither SELECTED nor PRESSED, state zero will be used.

When the current state matches the same number of values for two state definitions, the one that is used is the first one defined in the style. For example, the `MOUSE_OVER` state is always true of a `PRESSED` button (you can't press a button unless the mouse is over it). So, if the `MOUSE_OVER` state is declared first, it will always be chosen over `PRESSED`, and any painting defined for `PRESSED` will not be done.

```

&lt;state value="PRESSED"&gt; 
   &lt;imagePainter method="buttonBackground" path="images/button_press.png"
                          sourceInsets="9 10 9 10" /&gt;
   &lt;color type="TEXT_FOREGROUND" value="#FFFFFF"/&gt;      
&lt;/state&gt;
      
&lt;state value="MOUSE_OVER"&gt;    
   &lt;imagePainter method="buttonBackground" path="images/button_on.png"
                          sourceInsets="10 10 10 10" /&gt;
   &lt;color type="TEXT_FOREGROUND" value="#FFFFFF"/&gt;
&lt;/state&gt;

```

The code above will work properly. However, if you reverse the order of the `MOUSE_OVER` and `PRESSED` states in the file, the `PRESSED` state will never be used. This is because any state that is `PRESSED` state is *also* a `MOUSE_OVER` state. Since the `MOUSE_OVER` state was defined first, it is the one that will be used.

## Colors and Fonts

The &lt;color&gt; element requires two attributes:

<li>
`value` can be any one of the `java.awt.Color` constants, such as RED, WHITE, BLACK, BLUE, etc. It can also be a hex representation of RGB values, such as #FF00FF or #326A3B.
</li>
<li>
`type` describes where the color applies&#8212;it can be BACKGROUND, FOREGROUND, FOCUS, TEXT_BACKGROUND, OR TEXT_FOREGROUND.
</li>

For example:

```

  &lt;style id="basicStyle"&gt;
    &lt;state&gt;
      &lt;color value="WHITE" type="BACKGROUND"/&gt;
      &lt;color value="BLACK" type="FOREGROUND"/&gt;
    &lt;/state&gt;
  &lt;/style&gt;

```

The &lt;font&gt; element has three attributes:

<li>
`name`&#8212;the name of the font. For example, Arial or Verdana.
</li>
<li>
`size`&#8212;the size of the font in pixels.
</li>
<li>
`style` (optional)&#8212;BOLD, ITALIC, OR BOLD ITALIC. If omitted, you get a normal font.
</li>

For example:

```

  &lt;style id="basicStyle"&gt;
    &lt;font name="Verdana" size="16"/&gt;
  &lt;/style&gt;

```

Each of the &lt;color&gt; element and the &lt;font&gt; element has an alternate usage. Each can have an `id` attribute or an `idref` attribute. Using the `id` attribute, you can define a color that you can reuse later by using the `idref` attribute. For example,

```

&lt;color id="backColor" value="WHITE" type="BACKGROUND"/&gt;
&lt;font id="textFont" name="Verdana" size="16"/&gt;
...
...
...
&lt;color idref="backColor"/&gt;
&lt;font idref="textFont"/&gt;

```

## Insets

The `insets` add to the size of a component as it is drawn. For example, without insets, a button with a caption of `Cancel` will be just large enough to contain the caption in the chosen font. With an &lt;insets&gt; element like this

```

&lt;insets top="15" left="20" right="20" bottom="15"/&gt;,

```

the button will be made larger by 15 pixels above and below the caption and 20 pixels to the left and right of the caption.

## Painting With Images

Synth's file format allows customizing the painting by way of images. Synth's image painter breaks an image into nine distinct areas: top, top right, right, bottom right, bottom, bottom left, left, top left, and center. Each of the these areas is painted into the destination. The top, left, bottom, and right edges are tiled or stretched, while the corner portions (`sourceInsets`) remain fixed.

There is no relation between the &lt;insets&gt; element and the `sourceInsets` attribute. The &lt;insets&gt; element defines the space taken up by a region, while the `sourceInsets` attributes define how to paint an image. The &lt;insets&gt; and `sourceInsets` will often be similar, but they need not be.

You can specify whether the center area should be painted with the `paintCenter` attribute. The following image shows the nine areas:

Let's create a button as an example. To do this we can use the following image (shown larger than its actual size):

The red box at the upper left corner is 10 pixels square (including the box border)&#8212;it shows the corner region that should not be stretched when painting. To achieve this, the top and left `sourceInsets` should be set to 10. We'll use the following style and binding:

```

&lt;style id="buttonStyle"&gt;
   &lt;insets top="15" left="20" right="20" bottom="15"/&gt;
   &lt;state&gt;
      &lt;imagePainter method="buttonBackground" path="images/button.png"
        sourceInsets="10 10 10 10"/&gt;
   &lt;/state&gt;
&lt;/style&gt;
&lt;bind style="buttonStyle" type="region" key="button"/&gt;

```

The lines inside the &lt;state&gt; element specify that the background of buttons should be painted using the image `images/button.png`. That path is relative to the Class that is passed into SynthLookAndFeel's load method. The `sourceInsets` attribute specifies the areas of the image that are not to be stretched. In this case the top, left, bottom, and right insets are each 10. This will cause the painter not to stretch a 10 x 10 pixel area at each corner of the image.

The &lt;bind&gt; binds `buttonStyle` to all buttons.

The &lt;imagePainter&gt; element provides all the information needed to render a portion of a region. It requires only a few attributes:

<li>
method&#8212;this specifies which of the methods in the `javax.swing.plaf.synth.SynthPainter` class is to be used for painting. The `SynthPainter` class contains about 100 methods that begin with `paint`. When you determine which one you need, you remove the `paint` prefix, change the remaining first letter to lowercase, and use the result as the `method` attribute. For example, the `SynthPainter` method `paintButtonBackground` becomes the attribute `buttonBackground`.
</li>
<li>
path&#8212;the path to the image to be used, relative to the Class that is passed into SynthLookAndFeel's load method.
</li>
<li>
sourceInsets&#8212;the insets in pixels, representing the width and height of the corner areas that should not be stretched They map to the top, left, bottom, and right, in that order.
</li>
<li>
paintCenter (optional) : This attribute lets you keep the center of an image or get rid of it (in a text field, for example, so text can be drawn).
</li>

The listing below shows the XML code for loading different images depending on the &lt;state&gt; of the button

```

  &lt;style id="buttonStyle"&gt;
    &lt;property key="Button.textShiftOffset" type="integer" value="1"/&gt;
    &lt;insets top="15" left="20" right="20" bottom="15"/&gt;
    &lt;state&gt;
      &lt;imagePainter method="buttonBackground" path="images/button.png"
                    sourceInsets="10 10 10 10"/&gt;
    &lt;/state&gt;
    &lt;state value="PRESSED"&gt;
      &lt;imagePainter method="buttonBackground" path="images/button2.png"
                    sourceInsets="10 10 10 10"/&gt;
    &lt;/state&gt;
  &lt;/style&gt;
  &lt;bind style="buttonStyle" type="region" key="button"/&gt;

```

button2.png shows the depressed version of button.png, shifted one pixel to the right. The line

```

&lt;property key="Button.textShiftOffset" type="integer" value="1"/&gt;

```

shifts the button text accordingly, as discussed in the next section.

## The &lt;property&gt; Element

&lt;property&gt; elements are used to add key value pairs to a &lt;style&gt; element. Many components use the key value pairs for configuring their visual appearance.

The &lt;property&gt; element has three attributes:

<li>
`key`&#8212;the name of the property.
</li>
<li>
`type`&#8212;the data type of the property.
</li>
<li>
`value`&#8212;the value of the property.
</li>

There is a property table (`componentProperties.html`) that lists the properties each component supports: 
[`javax/swing/plaf/synth/doc-files/componentProperties.html`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/doc-files/componentProperties.html).

Since the button2.png image shifts the visual button one pixel when it is depressed, we should also shift the button text. There is a button property that does this:

```

&lt;property key="Button.textShiftOffset" type="integer" value="1"/&gt;

```

## An Example

Here is an example, using the button style defined above. The button style, plus a "backing style" with definitions of font and colors that are bound to all regions (similar to the "basicStyle" shown in the section titled "The XML File," above) are combined in 
[`<code>buttonSkin.xml`</code>](../examples/lookandfeel/SynthApplicationProject/src/lookandfeel/buttonSkin.xml)Here is a listing of `buttonSkin.xml`:

```

&lt;!-- Synth skin that includes an image for buttons --&gt;
&lt;synth&gt;
  &lt;!-- Style that all regions will use --&gt;
  &lt;style id="backingStyle"&gt;
    &lt;!-- Make all the regions that use this skin opaque--&gt;
    &lt;opaque value="TRUE"/&gt;
    &lt;font name="Dialog" size="12"/&gt;
    &lt;state&gt;
      &lt;!-- Provide default colors --&gt;
      &lt;color value="#9BC3B1" type="BACKGROUND"/&gt;
      &lt;color value="RED" type="FOREGROUND"/&gt;
    &lt;/state&gt;
  &lt;/style&gt;
  &lt;bind style="backingStyle" type="region" key=".*"/&gt;
  &lt;style id="buttonStyle"&gt;
    &lt;!-- Shift the text one pixel when pressed --&gt;
    &lt;property key="Button.textShiftOffset" type="integer" value="1"/&gt;
    &lt;insets top="15" left="20" right="20" bottom="15"/&gt;
    &lt;state&gt;
      &lt;imagePainter method="buttonBackground" path="images/button.png"
                    sourceInsets="10 10 10 10"/&gt;
    &lt;/state&gt;
    &lt;state value="PRESSED"&gt;
      &lt;imagePainter method="buttonBackground" path="images/button2.png"
                    sourceInsets="10 10 10 10"/&gt;
    &lt;/state&gt;
  &lt;/style&gt;
  &lt;!-- Bind buttonStyle to all JButtons --&gt;
  &lt;bind style="buttonStyle" type="region" key="button"/&gt; 
&lt;/synth&gt;

```

We can load this XML file to use the Synth look and feel for a simple application called `SynthApplication.java`. The GUI for this application includes a button and a label. Every time the button is clicked, the label increments.

The label is painted, even though `buttonSkin.xml` does not contain a style for it. This is because there is a general "backingStyle" that includes a font and colors.

Here is the listing of the 
[`<code>SynthApplication.java`</code>](../examples/lookandfeel/SynthApplicationProject/src/lookandfeel/SynthApplication.java)file.

Click the Launch button to run the SynthApplication example using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/lookandfeel/index.html#SynthApplication).

## Painting With Icons

Radio buttons and check boxes typically render their state by fixed-size icons. For these, you can create an icon and bind it to the appropriate property (refer to the properties table, 
[`javax/swing/plaf/synth/doc-files/componentProperties.html`](https://docs.oracle.com/javase/8/docs/api/javax/swing/plaf/synth/doc-files/componentProperties.html)). For example, to paint radio buttons that are selected or unselected, use this code:

```

&lt;style id="radioButton"&gt;
   &lt;imageIcon id="radio_off" path="images/radio_button_off.png"/&gt;
   &lt;imageIcon id="radio_on" path="images/radio_button_on.png"/&gt;
   &lt;property key="RadioButton.icon" value="radio_off"/&gt;
   &lt;state value="SELECTED"&gt;   
      &lt;property key="RadioButton.icon" value="radio_on"/&gt;
   &lt;/state&gt;
&lt;/style&gt;
&lt;bind style="radioButton" type="region" key="RadioButton"/&gt;        

```

## Custom Painters

Synth's file format allows for embedding arbitrary objects by way of the 
[`long-term persistence of JavaBeans components`](http://www.oracle.com/technetwork/java/persistence3-139471.html) . This ability is particularly useful in providing your own painters beyond the image-based ones Synth provides. For example, the following XML code specifies that a gradient should be rendered in the background of text fields:

```

&lt;synth&gt;
  &lt;object id="gradient" class="GradientPainter"/&gt;
  &lt;style id="textfield"&gt;
    &lt;painter method="textFieldBackground" idref="gradient"/&gt;
  &lt;/style&gt;
  &lt;bind style="textfield" type="region" key="textfield"/&gt;
&lt;/synth&gt;

```

Where the GradientPainter class looks like this:

```

public class GradientPainter extends SynthPainter {
   public void paintTextFieldBackground(SynthContext context,
                                        Graphics g, int x, int y,
                                        int w, int h) {
      // For simplicity this always recreates the GradientPaint. In a
      // real app you should cache this to avoid garbage.
      Graphics2D g2 = (Graphics2D)g;
      g2.setPaint(new GradientPaint((float)x, (float)y, Color.WHITE,
                 (float)(x + w), (float)(y + h), Color.RED));
      g2.fillRect(x, y, w, h);
      g2.setPaint(null);
   }
}

```

## Conclusion

In this lesson, we have covered the use of the `javax.swing.plaf.synth` package to create a custom look and feel. The emphasis of the lesson has been on using an external XML file to define the look and feel. The next lesson presents a sample application that creates a search dialog box using the Synth framework with an XML file.
