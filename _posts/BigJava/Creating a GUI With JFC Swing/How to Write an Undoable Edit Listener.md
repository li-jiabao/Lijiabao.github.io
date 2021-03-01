
# How to Write an Undoable Edit Listener

Undoable edit events occur when an operation that can be undone occurs on a component. Currently, only text components fire undoable edit events, and then only indirectly. The text component's document fires the events. For text components, undoable operations include inserting characters, deleting characters, and modifying the style of text. Programs typically listen to undoable edit events to assist in the implementation of undo and redo commands.

Here is the undoable edit event handling code from an application called `TextComponentDemo`.

```

...
**//where initialization occurs**
**document**.addUndoableEditListener(new MyUndoableEditListener());
...
protected class MyUndoableEditListener implements UndoableEditListener {
    public void undoableEditHappened(UndoableEditEvent e) {
        //Remember the edit and update the menus
        undo.addEdit(e.getEdit());
        undoAction.updateUndoState();
        redoAction.updateRedoState();
    }
}  

```

You can find a link to the source file for `TextComponentDemo` in the 
[example index for Using Swing Components](../examples/components/index.html#TextComponentDemo). For a discussion about the undoable edit listener aspect of the program see 
[Implementing Undo and Redo](../components/generaltext.html#undo)

## <a name="api" id="api">The Undoable Edit Listener API</a>

<a name="undoableeditlistener" id="undoableeditlistener">The UndoableEditListener Interface</a>

**Because `UndoableEditListener` has only one method, it has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[undoableEditHappened(UndoableEditEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/UndoableEditListener.html#undoableEditHappened-javax.swing.event.UndoableEditEvent-)</td><td headers="h2">Called when an undoable event occurs on the listened-to component.</td>

<a name="undoableeditevent" id="undoableeditevent">The UndoableEditEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Return the object that fired the event.</td>
<td headers="h101">[UndoableEdit getEdit()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/UndoableEditEvent.html#getEdit--)</td><td headers="h102">Returns an [`UndoableEdit`](https://docs.oracle.com/javase/8/docs/api/javax/swing/undo/UndoableEdit.html) object that represents the edit that occurred and contains information about and commands for undoing or redoing the edit.</td>

## <a name="eg" id="eg">Examples that Use Undoable Edit Listeners</a>

The following table lists the examples that use undoable edit listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
