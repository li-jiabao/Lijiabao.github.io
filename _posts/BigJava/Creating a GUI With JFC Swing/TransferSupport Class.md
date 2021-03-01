
# TransferSupport Class

The 
[`TransferSupport`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html) class serves two functions. As the name suggests, its first function is to support the transfer process and for that purpose it provides several utility methods used to access the details of the data transfer. The following list shows the methods that can be used to obtain information from the `TransferHandler`. Several of these methods are related to drop actions, which will be discussed in 
[Setting the Drop Mode](dropmodes.html).

<li>
[`Component getComponent()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getComponent--)&#8212; This method returns the target component of the transfer.</li>
<li>
[`int getDropAction()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getDropAction--) &#8212; This method returns the chosen action (`COPY`, `MOVE` or `LINK`) when the transfer is a drop. If the transfer is not a drop, this method throws an exception.</li>
<li>
[`int getUserDropAction()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getUserDropAction--) &#8212; This method returns the user's chosen drop action. For example, if the user simultaneously holds Control and Shift during the drag gesture, this indicates an `ACTION_LINK` action. For more information on user drop actions, see the API for 
[`DropTargetDragEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/dnd/DropTargetDragEvent.html). If the transfer is not a drop, this method throws an exception.</li>
<li>
[`int getSourceDropActions()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getSourceDropActions--) &#8212; This method returns the set of actions supported by the source component. If the transfer is not a drop, this method throws an exception.</li>
<li>
[`DataFlavor[] getDataFlavors()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getDataFlavors--) &#8212; This method returns all the data flavors supported by this component. For example, a component might support files and text, or text and color. If the transfer is not a drop, this method throws an exception.</li>
<li>
[`boolean isDataFlavorSupported(DataFlavor)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#isDataFlavorSupported-java.awt.datatransfer.DataFlavor-) &#8212; This method returns true if the specified `DataFlavor` is supported. The 
[`DataFlavor`](https://docs.oracle.com/javase/8/docs/api/java/awt/datatransfer/DataFlavor.html) indicates the type of data represented, such as an image (`imageFlavor`), a string (`stringFlavor`), a list of files (`javaFileListFlavor`), and so on.</li>
<li>
[`Transferable getTransferable()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getTransferable--) &#8212; This method returns the `Transferable` data for this transfer. It is more efficient to use one of these methods to query information about the transfer than to fetch the transferable and query it, so this method is not recommended unless you cannot get the information another way.</li>
<li>
[`DropLocation getDropLocation()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getDropLocation--) &#8212; This method returns the drop location in the component. Components with built-in drop support, such as list, table and tree, override this method to return more useful data. For example, the version of this method for the `JList` component returns the index in the list where the drop occurred. If the transfer is not a drop, this method throws an exception.</li>

## Sample Import Methods

Now that you are familiar with the `TransferSupport` utility methods, let us look at sample `canImport` and `importData` methods:

```

public boolean canImport(TransferSupport supp) {
    // Check for String flavor
    if (!supp.isDataFlavorSupported(stringFlavor)) {
        return false;
    }

    // Fetch the drop location
    DropLocation loc = supp.getDropLocation();

    // Return whether we accept the location
    return shouldAcceptDropLocation(loc);
}

public boolean importData(TransferSupport supp) {
    if (!canImport(sup)) {
        return false;
    }

    // Fetch the Transferable and its data
    Transferable t = supp.getTransferable();
    String data = t.getTransferData(stringFlavor);

    // Fetch the drop location
    DropLocation loc = supp.getDropLocation();

    // Insert the data at this location
    insertAt(loc, data);

    return true;
}

```

Next we look at how you can set the drop mode for selected components.
