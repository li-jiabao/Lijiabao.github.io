
# Choosing the Drop Action

Every drag source (Java based or otherwise) advertises the set of actions it supports when exporting data. If it supports data being copied, it advertises the `COPY` action; if it supports data being moved from it, then it advertises the `MOVE` action, and so on. For Swing components, the source actions are advertised through the 
[`getSourceActions`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.html#getSourceActions-javax.swing.JComponent-) method.

When a drag is initiated, the user has some control over which of the source actions is chosen for the transfer by way of keyboard modifiers used in conjunction with the drag gesture &#8212; this is called the **user action**. For example, the default (where no modifiers are used) generally indicates a move action, holding the Control key indicates a copy action, and holding both Shift and Control indicates a linking action. The user action is available via the 
[`getUserDropAction`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#getUserDropAction--) method.

The user action indicates a preference, but ultimately it is the target that decides the drop action. For example, consider a component that will only accept copied data. And consider a drag source that supports both copy and move. The `TransferHandler` for the copy-only target can be coded to only accept data from the source using the 
[`setDropAction`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#setDropAction-int-) method, even if the user has indicated a preference for a move action.

This work happens in the `canImport` method, where the target's `TransferHandler` decides whether to accept the incoming data. An implementation that explicitly chooses the `COPY` action, if it is supported by the source, might look like this:

```

public boolean canImport(TransferHandler.TransferSupport support) {
    // for the demo, we will only support drops (not clipboard paste)
    if (!support.isDrop()) {
        return false;
    }

    // we only import Strings
    if (!support.isDataFlavorSupported(DataFlavor.stringFlavor)) {
        return false;
    }

    // check if the source actions (a bitwise-OR of supported actions)
    // contains the COPY action
    <b>boolean copySupported = (COPY &amp; support.getSourceDropActions()) == COPY;
    if (copySupported) {
        support.setDropAction(COPY);
        return true;
    }</b>

    // COPY is not supported, so reject the transfer
    return false;
}

```

The code snippet displayed in bold shows where the source's supported drop actions are queried. If copy is supported, the `setDropAction` method is invoked to ensure that only a copy action will take place and the method returns true.

Next we will look at a demo that explicitly sets the drop action using `setDropAction`.
