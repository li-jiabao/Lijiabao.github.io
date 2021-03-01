
# Demo - LocationSensitiveDemo

The following demo, `LocationSensitiveDemo`, shows a `JTree` that has been configured to support drop on any node except for one called "names" (or its descendants). Use the text field at the top of the frame as the drag source (it will automatically increment the string number each time you drag from there).

A combo box below the tree allows you to toggle the behavior for showing the drop location. Swing's default behavior is to show the drop location only when the area can accept the drop. You can override this behavior to always show the drop location (even if the area cannot accept the drop) or to never show the drop location (even if the area can accept the drop).

<li>Click the Launch button to run `LocationSensitiveDemo` using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/dnd/index.html#LocationSensitiveDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ListDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/LocationSensitiveDemoProject/LocationSensitiveDemo.jnlp)<br /></li>
1. Initiate a drag by pressing on top of "String 0" in the text field and moving the mouse a short distance. Drag into the tree and move downwards. As you hover the mouse over most of the nodes, the drag acceptibility is indicated by both the mouse cursor and by the node becoming highlighted. Drop the text onto the "colors" node. The new item becomes a child of that node and a sibling to the colors listed.
1. Drag "String 1" from the textfield into the tree. Try to drop it on the "names" node. As you drag over that node or its children, Swing will not provide any drop location feedback and it will not accept the data.
1. Change the "Show drop location" combo box to "Always".
1. Repeat steps 1 and 2. The drop location now displays for the "names" node, but you cannot drop data into that area.
1. Change the "Show drop location" combo box to "Never".
1. Repeat steps 1 and 2. The drop location will not display for any part of the tree, though you can still drop data into the nodes, other than "names".

The `canImport` method for 
[`<code>LocationSensitiveDemo`</code>](../examples/dnd/LocationSensitiveDemoProject/src/dnd/LocationSensitiveDemo.java) looks like this:

```

public boolean canImport(TransferHandler.TransferSupport info) {
    // for the demo, we will only support drops (not clipboard paste)
    if (!info.isDrop()) {
        return false;
    }

    String item = (String)indicateCombo.getSelectedItem();
                
    <b>if (item.equals("Always")) {
        info.setShowDropLocation(true);
    } else if (item.equals("Never")) {
        info.setShowDropLocation(false);
    }</b>

    // we only import Strings
    if (!info.isDataFlavorSupported(DataFlavor.stringFlavor)) {
        return false;
    }

    // fetch the drop location
    JTree.DropLocation dl = (JTree.DropLocation)info.getDropLocation();

    TreePath path = dl.getPath();

    // we do not support invalid paths or descendants of the names folder
    <b>if (path == null || namesPath.isDescendant(path)) {
        return false;
    }</b>

    return true;
}

```

The first code snippet displayed in bold modifies the drop location feedback mechanism. If "Always", then the drop location is always shown. If "Never", the drop location is never shown. Otherwise, the default behavior applies.

The second code snippet displayed in bold contains the logic that determines whether the tree will accept the data. If the path is not a valid path or if it is not the names path (or its descendant) it will return false and the import will not be accepted.
