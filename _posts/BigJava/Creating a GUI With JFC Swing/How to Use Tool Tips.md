
# How to Use Tool Tips

Creating a tool tip for any `JComponent` object is easy. Use the `setToolTipText` method to set up a tool tip for the component. For example, to add tool tips to three buttons, you add only three lines of code:

```

b1.setToolTipText("Click this button to disable the middle button.");
b2.setToolTipText("This middle button does not react when you click it.");
b3.setToolTipText("Click this button to enable the middle button.");

```

When the user of the program pauses with the cursor over any of the program's buttons, the tool tip for the button comes up. You can see this by running the `ButtonDemo` example, which is explained in [How to Use Buttons, Check Boxes, and Radio Buttons](button.html). Here is a picture of the tool tip that appears when the cursor pauses over the left button in the `ButtonDemo` example.

For components such as tabbed panes that have multiple parts, it often makes sense to vary the tool tip text to reflect the part of the component under the cursor. For example, a tabbed pane might use this feature to explain what will happen when you click the tab under the cursor. When you implement a tabbed pane, you can specify the tab-specific tool tip text in an argument passed to the `addTab` or `setToolTipTextAt` method.

Even in components that have no API for setting part-specific tool tip text, you can generally do the job yourself. If the component supports renderers, then you can set the tool tip text on a custom renderer. The [table](table.html) and [tree](tree.html) sections provide examples of tool tip text determined by a custom renderer. An alternative that works for all `JComponent`s is creating a subclass of the component and overriding its `getToolTipText(MouseEvent)` method.

## <a name="api" id="api">The Tool Tip API</a>

Most of the API you need in order to set up tool tips belongs to the `JComponent` class, and thus is inherited by most Swing components. More tool tip API can be found in individual classes such as `JTabbedPane`. In general, those APIs are sufficient for specifying and displaying tool tips; you usually do not need to deal directly with the implementing classes 
[`JToolTip`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToolTip.html) and 
[`ToolTipManager`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ToolTipManager.html).

The following table lists the tool tip API in the `JComponent` class. For information on individual components' support for tool tips, see the how-to section for the component in question.
<th id="h1">Method</th><th id="h2">Purpose</th>
<td headers="h1">[setToolTipText(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setToolTipText-java.lang.String-)</td><td headers="h2">If the specified string is not null, then this method registers the component as having a tool tip and, when displayed, gives the tool tip the specified text. If the argument is null, then this method turns off the tool tip for this component.</td>
<td headers="h1">[String getToolTipText()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getToolTipText--)</td><td headers="h2">Returns the string that was previously specified with `setToolTipText`.</td>
<td headers="h1">[String getToolTipText(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getToolTipText-java.awt.event.MouseEvent-)</td><td headers="h2">By default, returns the same value returned by `getToolTipText()`. Multi-part components such as [`JTabbedPane`](tabbedpane.html), [`JTable`](table.html), and [`JTree`](tree.html) override this method to return a string associated with the mouse event location. For example, each tab in a tabbed pane can have different tool tip text.</td>
<td headers="h1">[Point getToolTipLocation(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getToolTipLocation-java.awt.event.MouseEvent-)</td><td headers="h2">Returns the location (in the receiving component's coordinate system) where the upper left corner of the component's tool tip appears. The argument is the event that caused the tool tip to be shown. The default return value is null, which tells the Swing system to choose a location.</td>

## <a name="eg" id="eg">Examples That Use Tool Tips</a>

This table lists some examples that use tool tips and points to where those examples are described.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
<td headers="h101">[`ButtonDemo`](../examples/components/index.html#ButtonDemo)</td><td headers="h102">This section and [How to Use Buttons, Check Boxes, and Radio Buttons](button.html)</td><td headers="h103">Uses a tool tip to provide instructions for a button.</td>
<td headers="h101">[`IconDemo`](../examples/components/index.html#IconDemo)</td><td headers="h102">[How to Use Icons](../components/icon.html)</td><td headers="h103">Uses a tool tip in a label to provide name and size information for an image.</td>
<td headers="h101">[`TabbedPaneDemo`](../examples/components/index.html#TabbedPaneDemo)</td><td headers="h102">[How to Use Tabbed Panes](tabbedpane.html)</td><td headers="h103">Uses tab-specific tool tip text specified in an argument to the `addTab` method.</td>
<td headers="h101">[`TableRenderDemo`](../examples/components/index.html#TableRenderDemo)</td><td headers="h102">[Specifying Tool Tips for Cells](table.html#celltooltip)</td><td headers="h103">Adds tool tips to a table using a renderer.</td>
<td headers="h101">[`TableToolTipsDemo`](../examples/components/index.html#TableToolTipsDemo)</td><td headers="h102">[Specifying Tool Tips for Cells](table.html#celltooltip), [Specifying Tool Tips for Column Headers](table.html#headertooltip)</td><td headers="h103">Adds tool tips to a table using various techniques.</td>
<td headers="h101">[`TreeIconDemo2`](../examples/components/index.html#TreeIconDemo2)</td><td headers="h102">[Customizing a Tree's Display](tree.html#display)</td><td headers="h103">Adds tool tips to a tree using a custom renderer.</td>
<td headers="h101">[`ActionDemo`](../examples/misc/index.html#ActionDemo)</td><td headers="h102">[How to Use Actions](../misc/action.html)</td><td headers="h103">Adds tool tips to buttons that have been created using `Action`s.</td>
