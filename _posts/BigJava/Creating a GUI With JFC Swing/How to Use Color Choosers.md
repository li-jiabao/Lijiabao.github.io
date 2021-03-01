
# How to Use Color Choosers

Use the
[`JColorChooser`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html) class to enable users to choose from a palette of colors. A color chooser is a component that you can place anywhere within your program GUI. The `JColorChooser` API also makes it easy to bring up a [dialog](dialog.html) (modal or not) that contains a color chooser.

Here is a picture of an application that uses a color chooser to set the text color in a banner:

<li>Click the Launch button to run the ColorChooser Demo using
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download the JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ColorChooserDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ButtonDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ColorChooserDemoProject/ColorChooserDemo.jnlp)<br /></li>

The source code for the program is in
[`ColorChooserDemo.java`](../examples/components/ColorChooserDemoProject/src/components/ColorChooserDemo.java).

The color chooser consists of everything within the box labeled **Choose Text Color**. This is what a standard color chooser looks like in the Java Look &amp; Feel. It contains two parts, a tabbed pane and a preview panel. The three tabs in the tabbed pane select **chooser panels**. The **preview panel** below the tabbed pane displays the currently selected color.

Here is the code from the example that creates a `JColorChooser` instance and adds it to a container:

```

public class ColorChooserDemo extends JPanel ... {
    public ColorChooserDemo() {
        super(new BorderLayout());
        banner = new JLabel("Welcome to the Tutorial Zone!",
                            JLabel.CENTER);
        banner.setForeground(Color.yellow);
        . . .
        tcc = new JColorChooser(banner.getForeground());
        . . .
        add(tcc, BorderLayout.PAGE_END);
    }

```

The `JColorChooser` constructor in the previous code snippet takes a `Color` argument, which specifies the chooser's initially selected color. If you do not specify the initial color, then the color chooser displays `Color.white`. See the
[`Color` API documentation](https://docs.oracle.com/javase/8/docs/api/java/awt/Color.html) for a list of color constants you can use.

<a name="changelistener" id="changelistener">A color chooser uses an instance of</a>
[`ColorSelectionModel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/colorchooser/ColorSelectionModel.html) to contain and manage the current selection. The color selection model fires a change event whenever the user changes the color in the color chooser. The example program registers a change listener with the color selection model so that it can update the banner at the top of the window. The following code registers and implements the change listener:

```

tcc.getSelectionModel().addChangeListener(this);
. . .
public void stateChanged(ChangeEvent e) {
    Color newColor = tcc.getColor();
    banner.setForeground(newColor);
}

```

See [How to Write a Change Listener](../events/changelistener.html) for general information about change listeners and change events.

A basic color chooser, like the one used in the example program, is sufficient for many programs. However, the color chooser API allows you to customize a color chooser by providing it with a preview panel of your own design, by adding your own chooser panels to it, or by removing existing chooser panels from the color chooser. Additionally, the `JColorChooser` class provides two methods that make it easy to use a color chooser within a dialog.


The rest of this section discusses these topics:


- [Another Example: ColorChooserDemo2](#advancedexample)
- [Showing a Color Chooser in a Dialog](#dialog)
- [Removing or Replacing the Preview Panel](#previewpanel)
- [Creating a Custom Chooser Panel](#chooserpanel)
- [The Color Chooser API](#api)
- [Examples that Use Color Choosers](#eg)

## <a name="advancedexample__1" id="advancedexample__1">Another Example: ColorChooserDemo2</a>

Now let's turn our attention to [ColorChooserDemo2](../examples/components/index.html#ColorChooserDemo2), a modified version of the previous demo program that uses more of the `JColorChooser` API. <!-- *******  boilerplate stuff ******* -->

<li>Click the Launch button to run the ColorChooser Demo using
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download the JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ColorChooserDemo2).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ButtonDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ColorChooserDemo2Project/ColorChooserDemo2.jnlp)<br /></li>

Here is a picture of ColorChooserDemo2:

This program customizes the banner text color chooser in these ways:

- Removes the preview panel
- Removes all of the default chooser panels
- Adds a custom chooser panel

[Removing or Replacing the Preview Panel](#previewpanel) covers the first customization. [Creating a Custom Chooser Panel](#chooserpanel) discusses the last two.

This program also adds a button that brings up a color chooser in a dialog, which you can use to set the banner background color.

## <a name="dialog" id="dialog">Showing a Color Chooser in a Dialog</a>

The `JColorChooser` class provides two class methods to make it easy to use a color chooser in a dialog. ColorChooserDemo2 uses one of these methods, `showDialog`, to display the background color chooser when the user clicks the **Show Color Chooser...** button. Here is the single line of code from the example that brings up the background color chooser in a dialog:

```

Color newColor = JColorChooser.showDialog(
                     ColorChooserDemo2.this,
                     "Choose Background Color",
                     banner.getBackground());

```

The first argument is the parent for the dialog, the second is the dialog title, and the third is the initially selected color.

The dialog disappears under three conditions: the user chooses a color and clicks the **OK** button, the user cancels the operation with the **Cancel** button, or the user dismisses the dialog with a frame control. If the user chooses a color, the `showDialog` method returns the new color. If the user cancels the operation or dismisses the window, the method returns `null`. Here is the code from the example that updates the banner background color according to the value returned by `showDialog`:

```

if (newColor != null) {
    banner.setBackground(newColor);
}

```

The dialog created by `showDialog` is modal. If you want a non-modal dialog, you can use `JColorChooser`'s `createDialog` method to create the dialog. This method also lets you specify action listeners for the **OK** and **Cancel** buttons in the dialog window. Use `JDialog`'s `show` method to display the dialog created by this method. For an example that uses this method, see [Specifying Other Editors](table.html#editor) in the [How to Use Tables](table.html) section.

## <a name="previewpanel" id="previewpanel">Removing or Replacing the Preview Panel</a>

By default, the color chooser displays a preview panel. ColorChooserDemo2 removes the text color chooser's preview panel with this line of code:

```

tcc.setPreviewPanel(new JPanel());

```

This effectively removes the preview panel because a plain `JPanel` has no size and no default view. To set the preview panel back to the default, use `null` as the argument to `setPreviewPanel`.

To provide a custom preview panel, you also use `setPreviewPanel`. The component you pass into the method should inherit from `JComponent`, specify a reasonable size, and provide a customized view of the current color. To get notified when the user changes the color in the color chooser, the preview panel must register as a change listener on the color chooser's color selection model as described [previously](#changelistener).

## <a name="chooserpanel" id="chooserpanel">Creating a Custom Chooser Panel</a>

The default color chooser provides five chooser panels:

- Swatches &#151; for choosing a color from a collection of swatches.
- [HSV](http://en.wikipedia.org/wiki/HSL_and_HSV) &#151; for choosing a color using the Hue-Saturation-Value color representation. Prior to JDK 7, this was called HSB, for Hue-Saturation-Brightness.
- [HSL](http://en.wikipedia.org/wiki/HSL_and_HSV) &#151; for choosing a color using the Hue-Saturation-Lightness color representation. This is new in JDK 7.
- [RGB](http://en.wikipedia.org/wiki/RGB_color_model) &#151; for choosing a color using the Red-Green-Blue color model.
- [CMYK](http://en.wikipedia.org/wiki/Cmyk) &#151; for choosing a color using the process color or four color model. This is new in JDK 7.

You can extend the default color chooser by adding chooser panels of your own design with `addChooserPanel`, or you can limit it by removing chooser panels with `removeChooserPanel`.

If you want to remove all of the default chooser panels and add one or more of your own, you can do this with a single call to `setChooserPanels`. ColorChooserDemo2 uses this method to replace the default chooser panels with an instance of
[`CrayonPanel`](../examples/components/ColorChooserDemo2Project/src/components/CrayonPanel.java), a custom chooser panel. Here is the call to `setChooserPanels` from that example:

```

//Override the chooser panels with our own.
AbstractColorChooserPanel panels[] = { new CrayonPanel() };
tcc.setChooserPanels(panels);

```

The code is straighforward: it creates an array containing the `CrayonPanel`. Next the code calls `setChooserPanels` to set the contents of the array as the color chooser's chooser panels.

`CrayonPanel` is a subclass of
[`AbstractColorChooserPanel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/colorchooser/AbstractColorChooserPanel.html) and overrides the five abstract methods defined in its superclass:

```

public void updateChooser() {
    Color color = getColorFromModel();
    if (Color.red.equals(color)) {
        redCrayon.setSelected(true);
    } else if (Color.yellow.equals(color)) {
        yellowCrayon.setSelected(true);
    } else if (Color.green.equals(color)) {
        greenCrayon.setSelected(true);
    } else if (Color.blue.equals(color)) {
        blueCrayon.setSelected(true);
    }
}

```

```

public String getDisplayName() {
    return "Crayons";
}

```




## <a name="api" id="api">The Color Chooser API</a>

The following tables list the commonly used `JColorChooser` constructors and methods. Other methods you might call are listed in the API tables in [The JComponent Class](jcomponent.html). The API for using color choosers falls into these categories:

- [Creating and Displaying the Color Chooser](#creating)
- [Customizing the Color Chooser GUI](#customizing)
- [Setting or Getting the Current Color](#selection)
<th id="h1" align="left">Method or Constructor</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[JColorChooser()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#JColorChooser--)<br />[JColorChooser(Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#JColorChooser-java.awt.Color-)<br />[JColorChooser(ColorSelectionModel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#JColorChooser-javax.swing.colorchooser.ColorSelectionModel-)</td><td headers="h2">Create a color chooser. The default constructor creates a color chooser with an initial color of `Color.white`. Use the second constructor to specify a different initial color. The `ColorSelectionModel` argument, when present, provides the color chooser with a color selection model.</td>
<td headers="h1">[Color showDialog(Component, String, Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#showDialog-java.awt.Component-java.lang.String-java.awt.Color-)</td><td headers="h2">Create and show a color chooser in a modal dialog. The `Component` argument is the parent of the dialog, the `String` argument specifies the dialog title, and the `Color` argument specifies the chooser's initial color.</td>
<td headers="h1">[JDialog createDialog(Component, String,<br /> boolean, JColorChooser, ActionListener,<br /> ActionListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#createDialog-java.awt.Component-java.lang.String-boolean-javax.swing.JColorChooser-java.awt.event.ActionListener-java.awt.event.ActionListener-)</td><td headers="h2">Create a dialog for the specified color chooser. As with `showDialog`, the `Component` argument is the parent of the dialog and the `String` argument specifies the dialog title. The other arguments are as follows: the `boolean` specifies whether the dialog is modal, the `JColorChooser` is the color chooser to display in the dialog, the first `ActionListener` is for the **OK** button, and the second is for the **Cancel** button.</td>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[void setPreviewPanel(JComponent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#setPreviewPanel-javax.swing.JComponent-)<br />[JComponent getPreviewPanel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#getPreviewPanel--)</td><td headers="h102">Set or get the component used to preview the color selection. To remove the preview panel, use `new JPanel()` as an argument. To specify the default preview panel, use `null`.</td>
<td headers="h101">[void setChooserPanels(AbstractColorChooserPanel[])](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#setChooserPanels-javax.swing.colorchooser.AbstractColorChooserPanel:A-)<br />[AbstractColorChooserPanel[] getChooserPanels()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#getChooserPanels--)</td><td headers="h102">Set or get the chooser panels in the color chooser.</td>
<td headers="h101">[void addChooserPanel(AbstractColorChooserPanel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#addChooserPanel-javax.swing.colorchooser.AbstractColorChooserPanel-)<br />[AbstractColorChooserPanel removeChooserPanel(AbstractColorChooserPanel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#removeChooserPanel-javax.swing.colorchooser.AbstractColorChooserPanel-)</td><td headers="h102">Add a chooser panel to the color chooser or remove a chooser panel from it.</td>
<td headers="h101">[void setDragEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#setDragEnabled-boolean-)<br />[boolean getDragEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#getDragEnabled--)</td><td headers="h102">Set or get the `dragEnabled` property, which must be true to enable drag handling on this component. The default value is false. See[Drag and Drop and Data Transfer](../dnd/index.html) for more details.</td>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[void setColor(Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#setColor-java.awt.Color-)<br />[void setColor(int, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#setColor-int-int-int-)<br />[void setColor(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#setColor-int-)<br />[Color getColor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#getColor--)</td><td headers="h202">Set or get the currently selected color. The three integer version of the `setColor` method interprets the three integers together as an RGB color. The single integer version of the `setColor` method divides the integer into four 8-bit bytes and interprets the integer as an RGB color as follows:<img src="../../figures/uiswing/components/ColorChooserInt.gif" width="109" height="30" alt="How color chooser interprets an int as an RGB value." /></td>
<td headers="h201">[void setSelectionModel(ColorSelectionModel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#setSelectionModel-javax.swing.colorchooser.ColorSelectionModel-)<br />[ColorSelectionModel getSelectionModel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JColorChooser.html#getSelectionModel--)</td><td headers="h202">Set or get the selection model for the color chooser. This object contains the current selection and fires change events to registered listeners whenever the selection changes.</td>

## <a name="eg" id="eg">Examples that Use Color Choosers</a>

This table shows the examples that use `JColorChooser` and where those examples are described.
<th id="h301" align="left">Example</th><th id="h302" align="left">Where Described</th><th id="h303" align="left">Notes</th>
<td headers="h301">[ColorChooserDemo](../examples/components/index.html#ColorChooserDemo)</td><td headers="h302">This section</td><td headers="h303">Uses a standard color chooser.</td>
<td headers="h301">[ColorChooserDemo2](../examples/components/index.html#ColorChooserDemo2)</td><td headers="h302">This section</td><td headers="h303">Uses one customized color chooser and one standard color chooser in a dialog created with `showDialog`.</td>
<td headers="h301">[TableDialogEditDemo](../examples/components/index.html#TableDialogEditDemo)</td><td headers="h302">[How to Use Tables](table.html)</td><td headers="h303">Shows how to use a color chooser as a custom cell editor in a table. The color chooser used by this example is created with `createDialog`.</td>
<td headers="h301">[BasicDnD](../misc/../examples/dnd/index.html#BasicDnD)</td><td headers="h302">[Introduction to DnD](../dnd/intro.html)</td><td headers="h303">Uses a color chooser that is not in a dialog; demonstrates default drag-and-drop capabilities of Swing components, including color choosers.</td>
