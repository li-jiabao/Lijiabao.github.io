
# Empty Table Drop

Dragging and dropping into an empty table presents a unique challenge. When adhering to the proper steps:

- Creating the empty table.
- Creating and attaching a `TransferHandler`.
- Enabling data transfer by calling `setDragEnabled(true)`.
- Creating a scroll pane and adding the table to the scroll pane.

You run the application and try to drag valid data into the table but it rejects the drop. What gives?

The reason is that the empty table (unlike an empty list or an empty tree) does not occupy any space in the scroll pane. The `JTable` does not automatically stretch to fill the height of a `JScrollPane`'s viewport &#8212; it only takes up as much vertical room as needed for the rows that it contains. So, when you drag over the empty table, you are not actually over the table and the drop fails.

You can configure the table to allow drop anywhere in the view port by calling 
[`JTable.setFillsViewportHeight(boolean)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.html#setFillsViewportHeight-boolean-). The default for this property is false to ensure backwards compatibility.

The following example, `FillViewportHeightDemo`, allows you to experiment with dropping onto an empty table. The demo contains an empty table with five columns that has its drop mode set to insert rows and a drag source that provides five comma-delimited values that autoincrement.

<li>Click the Launch button to run `FillViewportHeightDemo` using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/dnd/index.html#FillViewportHeightDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the FillViewportHeightDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FillViewportHeightDemoProject/FillViewportHeightDemo.jnlp)<br /></li>
1. Drag from the text field labeled "Drag from here" to the table.
1. Drop onto the table. The drop is rejected.
1. Double-click on the drag source. It deposits the current values (0, 0, 0, 0, 0) into the table and increments the values in the text field.
1. Once again, drag from the text field to the table. You can insert above or below the row, but not in the area underneath.
1. From the Options menu, choose "Fill Viewport Height" to enable the "fillsViewportHeight" property.
1. From the Options menu, choose "Reset" to empty the table.
1. Drag from the text component to the table. You can now drop anywhere on the view port and it inserts the data at row 0.

You can examine the source for 
[`<code>FillViewportHeightDemo.java`</code>](../examples/dnd/FillViewportHeightDemoProject/src/dnd/FillViewportHeightDemo.java), but the primary point to remember is that you should generally invoke `setFillsViewportHeight(true)` on any table that will accept dropped data.
