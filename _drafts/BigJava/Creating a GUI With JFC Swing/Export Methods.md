
# Export Methods

The first set of methods we will examine are used for exporting data from a component. These methods are invoked for the drag gesture, or the cut/copy action, when the component in question is the source of the operation. The `TransferHandler` methods for exporting data are:

<li>
[`getSourceActions(JComponent)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#getSourceActions-javax.swing.JComponent-) &#8212; This method is used to query what actions are supported by the source component, such as `COPY`, `MOVE`, or `LINK`, in any combination. For example, a customer list might not support moving a customer name out of the list, but it would very likely support copying the customer name. Most of our examples support both `COPY` and `MOVE`.</li>
<li>
[`createTransferable(JComponent)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#createTransferable-javax.swing.JComponent-) &#8212; This method bundles up the data to be exported into a 
[`Transferable`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/Transferable.html) object in preparation for the transfer.</li>
<li>
[`exportDone(JComponent, Transferable, int)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#exportDone-javax.swing.JComponent-java.awt.datatransfer.Transferable-int-) &#8212; This method is invoked after the export is complete. When the action is a `MOVE`, the data needs to be removed from the source after the transfer is complete &#8212; this method is where any necessary cleanup occurs.</li>

## Sample Export Methods

Here are some sample implementations of the export methods:

```

int getSourceActions(JComponent c) {
    return COPY_OR_MOVE;
}

Transferable createTransferable(JComponent c) {
    return new StringSelection(c.getSelection());
}

void exportDone(JComponent c, Transferable t, int action) {
    if (action == MOVE) {
        c.removeSelection();
    }
}

```

Next we will look at the `TransferHandler` methods required for data import.
