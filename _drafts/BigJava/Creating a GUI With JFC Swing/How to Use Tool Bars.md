
# How to Use Tool Bars

A 
[`JToolBar`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html) is a container that groups several components &#151; usually [buttons](button.html) with icons &#151; into a row or column. Often, tool bars provide easy access to functionality that is also in [menus](menu.html). [How to Use Actions](../misc/action.html) describes how to provide the same functionality in menu items and tool bar buttons. 


The following images show an application named `ToolBarDemo` that contains a tool bar above a text area. Click the Launch button to run ToolBarDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run it yourself, consult the [example index](../examples/components/index.html#ToolBarDemo).

By default, the user can drag the tool bar to another edge of its container or out into a window of its own. The next figure shows how the application looks after the user has dragged the tool bar to the right edge of its container.

For the drag behavior to work correctly, the tool bar must be in a container that uses the 
[`BorderLayout`](../layout/border.html) layout manager. The component that the tool bar affects is generally in the center of the container. The tool bar must be the only other component in the container, and it must not be in the center.

The next figure shows how the application looks after the user has dragged the tool bar outside its window.

The following code creates the tool bar and adds it to a container. You can find the entire program in 
[`ToolBarDemo.java`](../examples/components/ToolBarDemoProject/src/components/ToolBarDemo.java). 


```

public class ToolBarDemo extends JPanel
                         implements ActionListener {
    ...
    public ToolBarDemo() {
        super(new BorderLayout());
        ...
        JToolBar toolBar = new JToolBar("Still draggable");
        addButtons(toolBar);
        ...
        setPreferredSize(new Dimension(450, 130));
        add(toolBar, BorderLayout.PAGE_START);
        add(scrollPane, BorderLayout.CENTER);
    }
    ...
}

```

This code positions the tool bar above the scroll pane by placing both components in a panel controlled by a border layout, with the tool bar in the `PAGE_START` position and the scroll pane in the `CENTER` position. Because the scroll pane is in the center and no other components except the tool bar are in the container, by default the tool bar can be dragged to other edges of the container. The tool bar can also be dragged out into its own window, in which case the window has the title "Still draggable", as specified by the `JToolBar` constructor.

## <a name="button" id="button">Creating Tool Bar Buttons</a>

The buttons in the tool bar are ordinary `JButton` instances that use images from the Java Look and Feel Graphics Repository. Use images from the 
[Java Look and Feel Graphics Repository](http://www.oracle.com/technetwork/java/index-138612.html) if your tool bar has the Java look and feel.

Here is the code that creates the buttons and adds them to the tool bar.

```

protected void addButtons(JToolBar toolBar) {
    JButton button = null;

    //first button
    button = makeNavigationButton("Back24", PREVIOUS,
                                  "Back to previous something-or-other",
                                  "Previous");
    toolBar.add(button);

    //second button
    button = makeNavigationButton("Up24", UP,
                                  "Up to something-or-other",
                                  "Up");
    toolBar.add(button);

    **...//similar code for creating and adding the third button...**
}

protected JButton makeNavigationButton(String imageName,
                                       String actionCommand,
                                       String toolTipText,
                                       String altText) {
    //Look for the image.
    String imgLocation = "images/"
                         + imageName
                         + ".gif";
    URL imageURL = ToolBarDemo.class.getResource(imgLocation);

    //Create and initialize the button.
    JButton button = new JButton();
    button.setActionCommand(actionCommand);
    button.setToolTipText(toolTipText);
    button.addActionListener(this);

    if (imageURL != null) {                      //image found
        button.setIcon(new ImageIcon(imageURL, altText));
    } else {                                     //no image found
        button.setText(altText);
        System.err.println("Resource not found: " + imgLocation);
    }

    return button;
}

```

The first call to `makeNavigationButton` creates the image for the first button, using the 24x24 "Back" navigation image in the graphics repository. 


Besides finding the image for the button, the `makeNavigationButton` method also creates the button, sets the strings for its action command and tool tip text, and adds the action listener for the button. If the image is missing, the method prints an error message and adds text to the button, so that the button is still usable.

If any buttons in your tool bar duplicate the functionality of other components, such as menu items, you should probably create and add the tool bar buttons as described in 
[How to Use Actions](../misc/action.html).

## <a name="more" id="more">Customizing Tool Bars</a>

By adding a few lines of code to the preceding example, we can demonstrate some more tool bar features:

- Using `setFloatable(false)` to make a tool bar immovable.
- Using `setRollover(true)` to visually indicate tool bar buttons when the user passes over them with the cursor.
- Adding a separator to a tool bar.
- Adding a non-button component to a tool bar.

You can see these features by running ToolBarDemo2. Click the Launch button to run ToolBarDemo2 using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run it yourself, consult the [example index](../examples/components/index.html#ToolBarDemo2).

You can find the entire code for this program in 
[`ToolBarDemo2.java`](../examples/components/ToolBarDemo2Project/src/components/ToolBarDemo2.java). Below you can see a picture of a new UI using these customized features.

Because the tool bar can no longer be dragged, it no longer has bumps at its left edge. Here is the code that turns off dragging:

```

toolBar.setFloatable(false);

```

The tool bar is in rollover mode, so the button under the cursor has a visual indicator. The kind of visual indicator depends on the look and feel. For example, the Metal look and feel uses a gradient effect to indicate the button under the cursor while other types of look and feel use borders for this purpose. Here is the code that sets rollover mode:

```

toolBar.setRollover(true);

```

Another visible difference in the example above is that the tool bar contains two new components, which are preceded by a blank space called a [separator](separator.html). Here is the code that adds the separator:

```

toolBar.addSeparator();

```

Here is the code that adds the new components:

```

//fourth button
button = new JButton("Another button");
...
toolBar.add(button);

//fifth component is NOT a button!
JTextField textField = new JTextField("A text field");
...
toolBar.add(textField);

```

You can easily make the tool bar components either top-aligned or bottom-aligned instead of centered by invoking the `setAlignmentY` method. For example, to align the tops of all the components in a tool bar, invoke `setAlignmentY(TOP_ALIGNMENT)` on each component. Similarly, you can use the `setAlignmentX` method to specify the alignment of components when the tool bar is vertical. This layout flexibility is possible because tool bars use `BoxLayout` to position their components. For more information, see 
[How to Use BoxLayout](../layout/box.html).

## <a name="api" id="api">The Tool Bar API</a>

The following table lists the commonly used 
[`JToolBar`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html) constructors and methods. Other methods you might call are listed in the API tables in [The JComponent Class](jcomponent.html). 

<th id="h1">Method or Constructor</th><th id="h2">Purpose</th>
<td headers="h1">[JToolBar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#JToolBar--)<br />[JToolBar(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#JToolBar-int-)<br />[JToolBar(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#JToolBar-java.lang.String-)<br />[JToolBar(String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#JToolBar-java.lang.String-int-)</td><td headers="h2">Creates a tool bar. The optional int parameter lets you specify the orientation; the default is `HORIZONTAL`. The optional `String` parameter allows you to specify the title of the tool bar's window if it is dragged outside of its container.</td>
<td headers="h1">[Component add(Component)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.awt.Component-)</td><td headers="h2">Adds a component to the tool bar. You can associate a button with an `Action` using the `setAction(Action)` method defined by the `AbstractButton`.</td>
<td headers="h1">[void addSeparator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#addSeparator--)</td><td headers="h2">Adds a separator to the end of the tool bar.</td>
<td headers="h1">[void setFloatable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#setFloatable-boolean-)<br />[boolean isFloatable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#isFloatable--)</td><td headers="h2">The floatable property is true by default, and indicates that the user can drag the tool bar out into a separate window. To turn off tool bar dragging, use `toolBar.setFloatable(false)`. Some types of look and feel might ignore this property.</td>
<td headers="h1">[void setRollover(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#setRollover-boolean-)<br />[boolean isRollover()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#isRollover--)</td><td headers="h2">The rollover property is false by default. To make tool bar buttons be indicated visually when the user passes over them with the cursor, set this property to true. Some types of look and feel might ignore this property.</td>

## <a name="eg" id="eg">Examples That Use Tool Bars</a>

This table lists examples that use `JToolBar` and points to where those examples are described.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
<td headers="h101">[`ToolBarDemo`](../examples/components/index.html#ToolBarDemo)</td><td headers="h102">This page</td><td headers="h103">A basic tool bar with icon-only buttons.</td>
<td headers="h101">[`ToolBarDemo2`](../examples/components/index.html#ToolBarDemo2)</td><td headers="h102">This page</td><td headers="h103">Demonstrates a non-floatable tool bar in rollover mode that contains a separator and a non-button component.</td>
<td headers="h101">[`ActionDemo`](../examples/misc/index.html#ActionDemo)</td><td headers="h102">[How to Use Actions](../misc/action.html)</td><td headers="h103">Implements a tool bar using `Action` objects.</td>
