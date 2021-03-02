
# How to Use Separators

The 
[`JSeparator`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSeparator.html) class provides a horizontal or vertical dividing line or empty space. It's most commonly used in menus and tool bars. In fact, you can use separators without even knowing that a `JSeparator` class exists, since [menus](menu.html) and [tool bars](toolbar.html) provide convenience methods that create and add separators customized for their containers. Separators are somewhat similar to 
[borders](../components/border.html), except that they are genuine components and, as such, are drawn inside a container, rather than around the edges of a particular component.

Here is a picture of a menu that has three separators, used to divide the menu into four groups of items:

The code to add the menu items and separators to the menu is extremely simple, boiling down to something like this:

```

menu.add(menuItem1);
menu.add(menuItem2);
menu.add(menuItem3);
**menu.addSeparator();**
menu.add(rbMenuItem1);
menu.add(rbMenuItem2);
**menu.addSeparator();**
menu.add(cbMenuItem1);
menu.add(cbMenuItem2);
**menu.addSeparator();**
menu.add(submenu);

```

Adding separators to a tool bar is similar. You can find the full code explained in the how-to sections for [menus](menu.html) and [tool bars](toolbar.html). If you want more control over separators in menus and tool bars, you can directly use the `JSeparator` subclasses that implement them: 
[JPopupMenu.Separator](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.Separator.html) and 
[JToolBar.Separator](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.Separator.html). In particular, `JToolBar.Separator` has API for specifying the separator's size.

## <a name="elsewhere" id="elsewhere">Using JSeparator</a>

You can use the `JSeparator` class directly to provide a dividing line in any container. The following picture shows a GUI that has a separator to the right of the button labeled Fire.

Separators have almost no API and are extremely easy to use as long as you keep one thing in mind: In most implementations, a vertical separator has a preferred height of 0, and a horizontal separator has a preferred width of 0. This means a separator **is not visible** unless you either set its preferred size or put it in under the control of a layout manager such as `BorderLayout` or `BoxLayout` that stretches it to fill its available display area.

The vertical separator does have a bit of width (and the horizontal a bit of height), so you should see some space where the separator is. However, the actual dividing line isn't drawn unless the width and height are both non-zero.

The following code snippet shows how ListDemo puts together the panel that contains the vertical separator. You can find the full source code for ListDemo in 
[`ListDemo.java`](../examples/components/ListDemoProject/src/components/ListDemo.java).

```

JPanel buttonPane = new JPanel();
buttonPane.setLayout(new BoxLayout(buttonPane,
                                   BoxLayout.LINE_AXIS));
buttonPane.add(fireButton);
buttonPane.add(Box.createHorizontalStrut(5));
**buttonPane.add(new JSeparator(SwingConstants.VERTICAL));**
buttonPane.add(Box.createHorizontalStrut(5));
buttonPane.add(employeeName);
buttonPane.add(hireButton);
buttonPane.setBorder(BorderFactory.createEmptyBorder(5,5,5,5));

```

As the code shows, the buttons, separator, and text field all share a single container &#151; a `JPanel` instance that uses a left-to-right 
[box layout](../layout/box.html). Thanks to the layout manager (and to the fact that separators have unlimited maximum sizes), the separator is automatically made as tall as its available display area.

In the preceding code, the horizontal struts are invisible components used to provide space around the separator. A 5-pixel empty border provides a cushion around the panel, and also serves to prevent the separator from extending all the way to the component above it and the window's edge below it.

Here's a picture of another GUI that uses a separator, this time to put a dividing line between a group of controls and a display area.

You can find the code in the [example index](../examples/components/index.html#TextInputDemo). Here is the code that sets up the separator's container:

```

JPanel panel = new JPanel(new BorderLayout());
...
panel.setBorder(BorderFactory.createEmptyBorder(
                        GAP/2, //top
                        0,     //left
                        GAP/2, //bottom
                        0));   //right
panel.add(new JSeparator(JSeparator.VERTICAL),
          BorderLayout.LINE_START);
panel.add(addressDisplay,
          BorderLayout.CENTER);

```

As in the last example, the panel uses an empty border so that the separator doesn't extend all the way to the edges of its container. Placing the separator in the leftmost area of the `BorderLayout`-controlled container makes the separator as tall as the address-display component that's in the center of the container. See 
[How to Use BorderLayout](../layout/border.html) for details on how border layouts work.

## <a name="api" id="api">The Separator API</a>

The API for using separators is minimal, since they have no contents and don't respond to user input.
<th id="h1" align="left">Constructor or Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[void addSeparator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#addSeparator--)<br />[void addSeparator(Dimension)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.html#addSeparator-java.awt.Dimension-)<br />**(in `JToolBar`)**</td><td headers="h2">Append a tool bar separator (which is invisible in most, if not all, look and feels) to the current end of the tool bar. The optional argument specifies the size of the separator. The no-argument version of this method uses a separator with a default size, as determined by the current look and feel.</td>
<td headers="h1">[void addSeparator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#addSeparator--)<br />[void insertSeparator(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#insertSeparator-int-)<br />**(in `JMenu`)**</td><td headers="h2">Put a separator in the menu. The `addSeparator` method puts the separator at the current end of the menu. The `insertSeparator` method inserts the separator into the menu at the specified position.</td>
<td headers="h1">[void addSeparator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#addSeparator--)<br />**(in `JPopupMenu`)**</td><td headers="h2">Put a separator at the current end of the popup menu.</td>
<td headers="h1">[JSeparator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSeparator.html#JSeparator--)<br />[JSeparator(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSeparator.html#JSeparator-int-)</td><td headers="h2">Create a separator. If you don't specify an argument, the separator is horizontal. The argument can be either `SwingConstants.HORIZONTAL` or `SwingConstants.VERTICAL`.</td>
<td headers="h1">[void setOrientation(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSeparator.html#setOrientation-int-)<br />[int getOrientation()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JSeparator.html#getOrientation--)<br />**(in `JSeparator`)**</td><td headers="h2">Get or set the separator's orientation, which can be either `SwingConstants.HORIZONTAL` or `SwingConstants.VERTICAL`.</td>
<td headers="h1">[JToolBar.Separator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.Separator.html#JToolBar.Separator--)<br />[JToolBar.Separator(Dimension)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.Separator.html#JToolBar.Separator-java.awt.Dimension-)</td><td headers="h2">Create a separator for use in a tool bar. The optional argument specifies the separator's size.</td>
<td headers="h1">[setSeparatorSize(Dimension)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolBar.Separator.html#setSeparatorSize-java.awt.Dimension-)<br />**(in `JToolBar.Separator`)**</td><td headers="h2">Specify the separator's size. More specifically, the specified `Dimension` is used as the separator's minimum, preferred, and maximum sizes.</td>
<td headers="h1">[JPopupMenu.Separator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.Separator.html#JPopupMenu.Separator--)</td><td headers="h2">Create a separator for use in a menu.</td>

## <a name="eg" id="eg">Examples that Use Separators</a>

Several of this lesson's examples use separators, usually in menus. Here is a list of some of the more interesting examples.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
<td headers="h101">[`ListDemo`](../examples/components/index.html#ListDemo)</td><td headers="h102">This section and [How to Use Lists](list.html)</td><td headers="h103">Uses a vertical separator in a panel controlled by a horizontal box layout.</td>
<td headers="h101">[`TextInputDemo`](../examples/components/index.html#TextInputDemo)</td><td headers="h102">This section and [How to Use Formatted Text Fields](formattedtextfield.html)</td><td headers="h103">Uses a vertical separator at the left of a panel controlled by a border layout.</td>
<td headers="h101">[`MenuDemo`](../examples/components/index.html#MenuDemo)</td><td headers="h102">This section and [How to Use Menus](menu.html)</td><td headers="h103">Uses the `JMenu` method `addSeparator` to put separators in a menu.</td>
<td headers="h101">[`ToolBarDemo2`](../examples/components/index.html#ToolBarDemo2)</td><td headers="h102">[How to Use Tool Bars](toolbar.html)</td><td headers="h103">Uses the `JToolBar` method `addSeparator` to put space between two kinds of buttons.</td>

If you are programming in JavaFX, see
[Using JavaFX UI Controls](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/separator.htm).
