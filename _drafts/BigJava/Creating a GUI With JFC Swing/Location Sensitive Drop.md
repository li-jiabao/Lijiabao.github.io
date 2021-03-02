
# Location Sensitive Drop

Sometimes you have a complex component and you want the user to be able to drop on some parts of it, but not on others. Perhaps it is a table that allows data to be dropped only in certain columns; or perhaps it is a tree that allows data to be dropped only on certain nodes. Obviously you want the cursor to provide accurate feedback &#8212; it should only show the drop location when it is over the specific part of the component that accepts drops.

This is simple to accomplish by installing the necessary logic in the 
[`canImport(TransferHandler.TransferSupport`)](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#canImport-javax.swing.TransferHandler.TransferSupport-) method of the `TransferHandler` class. It works only with this particular version of `canImport` because it is called continously while the drag gesture is over the bounds of the component. When this method returns true, Swing shows the drop cursor and the drop location is visually indicated; when this method returns false, Swing shows the "no-drag" cursor and the drop location is not displayed.

For example, imagine a table that allows drop, but not in the first column. The `canImport` method might look something like this:

```

public boolean canImport(TransferHandler.TransferSupport info) {
    // for the demo, we will only support drops (not clipboard paste)
    if (!info.isDrop()) {
        return false;
    }

    // we only import Strings
    if (!info.isDataFlavorSupported(DataFlavor.stringFlavor)) {
        return false;
    }

    // fetch the drop location
    JTable.DropLocation dl = (JTable.DropLocation)info.getDropLocation();

    int column = dl.getColumn();

    // we do not support invalid columns or the first column
    <b>if (column == -1 || column == 0) {
        return false;</b>
    }

    return true;
}

```

The code displayed in bold indicates the location-sensitive drop logic: When the user drops the data in such a way that the column cannot be calculated (and is therefore invalid) or when the user drops the text in the first column, the `canImport` method returns false &#8212; so Swing shows the "no-drag" mouse cursor. As soon as the user moves the mouse off the first column `canImport` returns true and Swing shows the drag cursor.

Next, we show a demo of a tree that has implemented location-sensitive drop.
