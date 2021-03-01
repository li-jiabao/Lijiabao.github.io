
# How to Use Labels

With the 
[`JLabel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html) class, you can display unselectable text and images. If you need to create a component that displays a string, an image, or both, you can do so by using or extending `JLabel`. If the component is interactive and has a certain state, use a [button](button.html) instead of a label.

By specifying HTML code in a label's text, you can give the label various characteristics such as multiple lines, multiple fonts or multiple colors. If the label uses just a single color or font, you can avoid the overhead of HTML processing by using the `setForeground` or `setFont` method instead. See [Using HTML in Swing Components](html.html) for details.

Note that labels are not opaque by default. If you need to paint the label's background, it is recommended that you turn its opacity property to "true". The following code snippet shows how to do this.

```

label.setOpaque(true);

```

The following picture introduces an application that displays three labels. The window is divided into three rows of equal height; the label in each row is as wide as possible.

<li>Click the Launch button to run the Label Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#LabelDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the LabelDemo Application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/LabelDemoProject/LabelDemo.jnlp)<br /></li>
<li>Resize the window so you can see how the labels' contents are placed within the labels' drawing area.<br />
All the label contents have default vertical alignment &#151; that is, the label contents are centered vertically in the label's drawing area. The top label, which contains both an image and text, has horizontal center alignment. The second label, which contains just text, has left (leading) alignment, which is the default for text-only labels in left-to-right languages. The third label, which contains just an image, has horizontal center alignment, which is the default for image-only labels.</li>

Below is the code from 
[`LabelDemo.java`](../examples/components/LabelDemoProject/src/components/LabelDemo.java) that creates the labels in the previous example.

```

ImageIcon icon = createImageIcon("images/middle.gif");
. . .
label1 = new JLabel("Image and Text",
                    icon,
                    JLabel.CENTER);
//Set the position of the text, relative to the icon:
label1.setVerticalTextPosition(JLabel.BOTTOM);
label1.setHorizontalTextPosition(JLabel.CENTER);

label2 = new JLabel("Text-Only Label");
label3 = new JLabel(icon);

```

The code for the `createImageIcon` method is similar to that used throughout this tutorial. You can find it in 
[How to Use Icons](../components/icon.html).

Often, a label describes another component. When this occurs, you can improve your program's accessibility by using the `setLabelFor` method to identify the component that the label describes. For example:

```

amountLabel.setLabelFor(amountField);

```

The preceding code, taken from the `FormattedTextFieldDemo` example discussed in [How to Use Formatted Text Fields](formattedtextfield.html), lets assistive technologies know that the label (`amountLabel`) provides information about the formatted text field (`amountField`). For more information about assistive technologies, see 
[How to Support Assistive Technologies](../misc/access.html).

## <a name="api" id="api">The Label API</a>

The following tables list the commonly used `JLabel` constructors and methods. Other methods you are likely to call are defined by the `Component` and `JComponent` classes. They include `setFont`, `setForeground`, `setBorder`, `setOpaque`, and `setBackground`. See [The JComponent Class](jcomponent.html) for details. The API for using labels falls into three categories:

- [Setting or Getting the Label's Contents](#contentsapi)
- [Fine Tuning the Label's Appearance](#looksapi)
- [Supporting Accessibility](#accessapi)

In the following API, do not confuse label alignment with X and Y alignment. X and Y alignment are used by layout managers and can affect the way any component &#151; not just a label &#151; is sized or positioned. Label alignment, on the other hand, has no effect on a label's size or position. Label alignment simply determines where, inside the label's painting area, the label's contents are positioned. Typically, the label's painting area is exactly the size needed to paint on the label and thus label alignment is irrelevant. For more information about X and Y alignment, see 
[How to Use BoxLayout](../layout/box.html).
<th id="h1">Method or Constructor</th><th id="h2">Purpose</th>
<td headers="h1">[JLabel(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-javax.swing.Icon-)<br />[JLabel(Icon, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-javax.swing.Icon-int-)<br />[JLabel(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-java.lang.String-)<br />[JLabel(String, Icon, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-java.lang.String-javax.swing.Icon-int-)<br />[JLabel(String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel-java.lang.String-int-)<br />[JLabel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#JLabel--)</td><td headers="h2">Creates a `JLabel` instance, initializing it to have the specified text/image/alignment. The `int` argument specifies the horizontal alignment of the label's contents within its drawing area. The horizontal alignment must be one of the following constants defined in the [`SwingConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingConstants.html) interface (which `JLabel` implements): `LEFT`, `CENTER`, `RIGHT`, `LEADING`, or `TRAILING`. For ease of localization, we strongly recommend using `LEADING` and `TRAILING`, rather than `LEFT` and `RIGHT`.</td>
<td headers="h1">[void setText(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setText-java.lang.String-)<br />[String getText()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getText--)</td><td headers="h2">Sets or gets the text displayed by the label. You can use HTML tags to format the text, as described in [Using HTML in Swing Components](html.html).</td>
<td headers="h1">[void setIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setIcon-javax.swing.Icon-)<br />[Icon getIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getIcon--)</td><td headers="h2">Sets or gets the image displayed by the label.</td>
<td headers="h1">[void setDisplayedMnemonic(char)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setDisplayedMnemonic-char-)<br />[char getDisplayedMnemonic()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getDisplayedMnemonic--)</td><td headers="h2">Sets or gets the letter that should look like a keyboard alternative. This is helpful when a label describes a component (such as a text field) that has a keyboard alternative but cannot display it. If the labelFor property is also set (using `setLabelFor`), then when the user activates the mnemonic, the keyboard focus is transferred to the component specified by the labelFor property.</td>
<td headers="h1">[void setDisplayedMnemonicIndex(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setDisplayedMnemonicIndex-int-)<br />[int getDisplayedMnemonicIndex()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getDisplayedMnemonicIndex--)</td><td headers="h2">Sets or gets a hint as to which character in the text should be decorated to represent the mnemonic. This is useful when you have two instances of the same character and wish to decorate the second instance. For example, `setDisplayedMnemonicIndex(5)` decorates the character that is at position 5 (that is, the 6th character in the text). Not all types of look and feel may support this feature.</td>
<td headers="h1">[void setDisabledIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setDisabledIcon-javax.swing.Icon-)<br />[Icon getDisabledIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getDisabledIcon--)</td><td headers="h2">Sets or gets the image displayed by the label when it is disabled. If you do not specify a disabled image, then the look and feel creates one by manipulating the default image.</td>
<th id="h101">Method</th><th id="h102">Purpose</th>
<td headers="h101">[void setHorizontalAlignment(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setHorizontalAlignment-int-)<br />[void setVerticalAlignment(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setVerticalAlignment-int-)<br />[int getHorizontalAlignment()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getHorizontalAlignment--)<br />[int getVerticalAlignment()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getVerticalAlignment--)</td><td headers="h102">Sets or gets the area on the label where its contents should be placed. The [`SwingConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingConstants.html) interface defines five possible values for horizontal alignment: `LEFT`, `CENTER` (the default for image-only labels), `RIGHT`, `LEADING` (the default for text-only labels), `TRAILING`. For vertical alignment: `TOP`, `CENTER` (the default), and `BOTTOM`.</td>
<td headers="h101">[void setHorizontalTextPosition(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setHorizontalTextPosition-int-)<br />[void setVerticalTextPosition(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setVerticalTextPosition-int-)<br />[int getHorizontalTextPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getHorizontalTextPosition--)<br />[int getVerticalTextPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getVerticalTextPosition--)</td><td headers="h102">Sets or gets the location where the label's text will be placed, relative to the label's image. The [`SwingConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingConstants.html) interface defines five possible values for horizontal position: `LEADING`, `LEFT`, `CENTER`, `RIGHT`, and `TRAILING` (the default). For vertical position: `TOP`, `CENTER` (the default), and `BOTTOM`.</td>
<td headers="h101">[void setIconTextGap(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setIconTextGap-int-)<br />[int getIconTextGap()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getIconTextGap--)</td><td headers="h102">Sets or gets the number of pixels between the label's text and its image.</td>
<th id="h201">Method</th><th id="h202">Purpose</th>
<td headers="h201">[void setLabelFor(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#setLabelFor-java.awt.Component-)<br />[Component getLabelFor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html#getLabelFor--)</td><td headers="h202">Sets or gets which component the label describes.</td>

## <a name="eg" id="eg">Examples That Use Labels</a>

The following table lists some of the many examples that use labels.
<th id="h301" align="left">Example</th><th id="h302" align="left">Where Described</th><th id="h303" align="left">Notes</th>
<td headers="h301">[`LabelDemo`](../examples/components/index.html#LabelDemo)</td><td headers="h302">This section</td><td headers="h303">Shows how to specify horizontal and vertical alignment as well as how to align a label's text and image.</td>
<td headers="h301">[`HtmlDemo`](../examples/components/index.html#HtmlDemo)</td><td headers="h302">[Using HTML in Swing Components](html.html)</td><td headers="h303">Lets you experiment with specifying HTML text for a label.</td>
<td headers="h301">[`BoxAlignmentDemo`](../examples/layout/index.html#BoxAlignmentDemo)</td><td headers="h302">[Fixing Alignment Problems](../layout/box.html#alignment)</td><td headers="h303">Demonstrates possible alignment problems when using a label in a vertical box layout. Shows how to solve the problem.</td>
<td headers="h301">[`DialogDemo`](../examples/components/index.html#DialogDemo)</td><td headers="h302">[How to Use Dialogs](dialog.html)</td><td headers="h303">Uses a changeable label to display instructions and provide feedback.</td>
<td headers="h301">[`SplitPaneDemo`](../examples/components/index.html#SplitPaneDemo)</td><td headers="h302">[How to Use Split Panes](splitpane.html) and [How to Use Lists](list.html)</td><td headers="h303">Displays an image using a label inside of a scroll pane.</td>
<td headers="h301">[`SliderDemo2`](../examples/components/index.html#SliderDemo2)</td><td headers="h302">[How to Use Sliders](slider.html)</td><td headers="h303">Uses `JLabel` to provide labels for a slider.</td>
<td headers="h301">[`TableDialogEditDemo`](../examples/components/index.html#TableDialogEditDemo)</td><td headers="h302">[How to Use Tables](table.html)</td><td headers="h303">Implements a label subclass, `ColorRenderer`, to display colors in table cells.</td>
<td headers="h301">[`FormattedTextFieldDemo`](../examples/components/index.html#FormattedTextFieldDemo)</td><td headers="h302">[How to Use Formatted Text Fields](formattedtextfield.html)</td><td headers="h303">Has four rows, each containing a label and the formatted text field it describes.</td>
<td headers="h301">[`TextComponentDemo`](../examples/components/index.html#TextComponentDemo)</td><td headers="h302">[Text Component Features](generaltext.html)</td><td headers="h303">`TextComponentDemo` has an inner class (`CaretListenerLabel`) that extends `JLabel` to provide a label that listens for events, updating itself based on the events.</td>
<td headers="h301">[`ColorChooserDemo`](../examples/components/index.html#ColorChooserDemo)</td><td headers="h302">[How to Use Color Choosers](colorchooser.html)</td><td headers="h303">Uses an opaque label to display the currently chosen color against a fixed-color background.</td>


See the
[Using JavaFX UI Controls: Label](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/label.htm) tutorial to learn about JavaFX labeled controls.
