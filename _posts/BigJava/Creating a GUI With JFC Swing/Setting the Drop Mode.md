
# Setting the Drop Mode

When enabling drop on a component, such as a list, you need to decide how you want the drop location to be interpreted. 
For example, do you want to restrict the user to replacing existing entries? Do you want to only allow adding or inserting new entries? Do you want to allow both? To configure this behavior, the 
[`JList`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html) class provides the 
[`setDropMode`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#setDropMode-javax.swing.DropMode-) method which supports the following drop modes.

- The default drop mode for `JList` is `DropMode.USE_SELECTION`. When dragging in this mode, the selected item in the list moves to echo the potential drop point. On a drop the selected item shifts to the drop location. This mode is provided for backwards compatibility but is otherwise not recommended.
- In `DropMode.ON`, the selected item in the list moves to echo the potential drop point, but the selected item is not affected on the drop. This mode can be used to drop on top of existing list items.
- In `DropMode.INSERT`, the user is restricted to selecting the space between existing list items, or before the first item or after the last item in the list. Selecting existing list items is not allowed.
- `DropMode.ON_OR_INSERT` is a combination of the `ON` and `INSERT` modes.

The `JTree` class provides the same set of 
[drop modes](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#setDropMode-javax.swing.DropMode-) and the `JTable` class has 
[several more](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.html#setDropMode-javax.swing.DropMode-) specific to adding rows or columns.

To obtain the location of the drop, the `TransferSupport` class provides the 
[`getDropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getDropLocation--) method that returns the precise point where the drop has occurred. But for a list component, the index of the drop is more useful than a pixel location, so `JList` provides a special subclass, called 
[`JList.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html). This class provides the 
[`getIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html#getIndex--) and 
[`isInsert`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html#isInsert--) methods, which handle the math for you.

The table, tree, and text components each provide an implementation of `DropLocation` with methods that make the most sense for each component. The 
[`JTable.setDropMode`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.html#setDropMode-javax.swing.DropMode-) method has the most choices. The following table shows the methods for all four classes:
<th id="h1">[`JList.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html)</th><th id="h2">[`JTree.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.DropLocation.html)</th><th id="h3">[`JTable.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html)</th><th id="h4">[`JTextComponent.DropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.DropLocation.html)</th>
<td headers="h1">[`isInsert`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html#isInsert--)</td><td headers="h2">[`getChildIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.DropLocation.html#getChildIndex--)</td><td headers="h3">[`isInsertRow`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html#isInsertRow--)</td><td headers="h4">[`getIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.DropLocation.html#getIndex--)</td>
<td headers="h1">[`getIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.DropLocation.html#getIndex--)</td><td headers="h2">[`getPath`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.DropLocation.html#getPath--)</td><td headers="h3">[`isInsertColumn`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html#isInsertColumn--)</td><td headers="h4">[`getBias`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.DropLocation.html#getBias--)</td>
<td headers="h1">&#160;</td><td headers="h2">&#160;</td><td headers="h3">[`getRow`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html#getRow--)</td><td headers="h4">&#160;</td>
<td headers="h1">&#160;</td><td headers="h2">&#160;</td><td headers="h3">[`getColumn`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.DropLocation.html#getColumn--)</td><td headers="h4">&#160;</td>

Next is a demo that implements a custom transfer handler for a list component so that it fully participates in drag and drop.
