
# TransferHandler Class

At the heart of the data transfer mechanism is the 
[`TransferHandler`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html) class. As its name suggests, the `TransferHandler` provides an easy mechanism for transferring data to and from a `JComponent` &#8212; all the details are contained in this class and its supporting classes. Most components are provided with a default transfer handler. You can create and install your own transfer handler on any component.

There are three methods used to engage a `TransferHandler` on a component:

<li>
[`setDragEnabled(boolean)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#setDragEnabled-boolean-) &#8212; turns on drag support. (The default is false.) This method is defined on each component that supports the drag gesture; the link takes you to the documentation for `JList`.</li>
<li>
[`setDropMode(DropMode)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#setDropMode-javax.swing.DropMode-) &#8212; configures how drop locations are determined. This method is defined for `JList`, `JTable`, and `JTree`; the link takes you to the documentation for `JList`.</li>
<li>
[`setTransferHandler(TransferHandler)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setTransferHandler-javax.swing.TransferHandler-) &#8212; used to plug in custom data import and export. This method is defined on `JComponent`, so it is inherited by every Swing component.</li>

As mentioned previously, the default Swing transfer handlers, such as those used by text components and the color chooser, provide the support considered to be most useful for both importing and exporting of data. However list, table, and tree do not support drop by default. The reason for this is that there is no all-purpose way to handle a drop on these components. For example, what does it mean to drop on a particular node of a `JTree`? Does it replace the node, insert below it, or insert as a child of that node? Also, we do not know what type of model is behind the tree &#8212; it might not be mutable.

While Swing cannot provide a default implementation for these components, the framework for drop is there. You need only to provide a custom `TransferHandler` that manages the actual import of data.

If you install a custom `TransferHandler` onto a Swing component, the default support is replaced. For example, if you replace `JTextField`'s `TransferHandler` with one that handles colors only, you will disable its ability to support import and export of text.

If you must replace a default `TransferHandler` &#8212; for example, one that handles text &#8212; you will need to re-implement the text import and export ability. This does not need to be as extensive as what Swing provides &#8212; it could be as simple as supporting the `StringFlavor` data flavor, depending on your application's needs.

Next we show what `TransferHandler` methods are required to implement data export.
