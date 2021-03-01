
# How to Write a List Selection Listener

List selection events occur when the selection in a 
[list](../components/list.html) or 
[table](../components/table.html) is either changing or has just changed. List selection events are fired from an object that implements the 
[`ListSelectionModel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ListSelectionModel.html) interface. To get a table's list selection model object, you can use either `getSelectionModel` method or getColumnModel().getSelectionModel().

To detect list selection events, you register a listener on the appropriate list selection model object. The `JList` class also gives you the option of registering a listener on the list itself, rather than directly on the list selection model.

This section looks at two examples that show how to listen to list selection events on a selection model. [Examples that Use List Selection Listeners](#eg) lists examples that listen on the list directly.

In these two examples, you can dynamically change the selection mode to any of the three supported modes:

- single selection mode
- single interval selection mode
- multiple interval selection mode

Here is a picture of ListSelectionDemo example running in a List :

<li>Click the Launch button to run ListSelectionDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#ListSelectionDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ListSelectionDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ListSelectionDemoProject/ListSelectionDemo.jnlp)<br /></li>
1. Select and deselect items in the list. The mouse and keyboard commands required to select items depends on the look and feel. For the Java look and feel, click the left mouse button to begin a selection, use the shift key to extend a selection contiguously, and use the control key to extend a selection discontiguously. Note that there are two types of selections: Lead and Anchor. Lead is the focused item and Anchor is the highlighted item. When you press ctrl key and move up and down, the lead selection causes the events being fired even though the actual selection has not changed. Dragging the mouse moves or extends the selection, depending on the list selection mode.

Here is a picture of TableListSelectionDemo example running in a Table:

<li>Click the Launch button to run TableListSelectionDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#TableListSelectionDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the TableListSelectionDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TableListSelectionDemoProject/TableListSelectionDemo.jnlp)<br /></li>
1. Select and deselect items in the table. The mouse and keyboard commands required to select items depends on the look and feel. For the Java look and feel, click the left mouse button to begin a selection, use the shift key to extend a selection contiguously, and use the control key to extend a selection discontiguously. Note that there are two types of selections: Lead and Anchor. Lead is the focused item and Anchor is the highlighted item. When you press ctrl key and move up or down, the lead selection causes the events being fired even though the actual selection has not changed. Dragging the mouse moves or extends the selection, depending on the list selection mode.

You can find the entire program of ListSelectionDemo in 
[`<code>ListSelectionDemo.java`</code>](../examples/events/ListSelectionDemoProject/src/events/ListSelectionDemo.java) and the entire program of TableListSelectionDemo in 
[`<code>TableListSelectionDemo.java`</code>](../examples/events/TableListSelectionDemoProject/src/events/TableListSelectionDemo.java).

Here is the code from `ListSelectionDemo` that sets up the selection model and adds a listener to it:

```

**...//where the member variables are defined**
JList list;
    **...//in the init method:**
    listSelectionModel = list.getSelectionModel();
    listSelectionModel.addListSelectionListener(
                            new SharedListSelectionHandler());
    ...

```

And here is the code for the listener, which works for all the possible selection modes:

```

class SharedListSelectionHandler implements ListSelectionListener {
    public void valueChanged(ListSelectionEvent e) {
        ListSelectionModel lsm = (ListSelectionModel)e.getSource();

        int firstIndex = e.getFirstIndex();
        int lastIndex = e.getLastIndex();
        boolean isAdjusting = e.getValueIsAdjusting();
        output.append("Event for indexes "
                      + firstIndex + " - " + lastIndex
                      + "; isAdjusting is " + isAdjusting
                      + "; selected indexes:");

        if (lsm.isSelectionEmpty()) {
            output.append(" &lt;none&gt;");
        } else {
            // Find out which indexes are selected.
            int minIndex = lsm.getMinSelectionIndex();
            int maxIndex = lsm.getMaxSelectionIndex();
            for (int i = minIndex; i &lt;= maxIndex; i++) {
                if (lsm.isSelectedIndex(i)) {
                    output.append(" " + i);
                }
            }
        }
        output.append(newline);
    }
}


```

This `valueChanged` method displays the first and last indices reported by the event, the value of the event's `isAdjusting` flag, and the indices currently selected.

Note that the first and last indices reported by the event indicate the inclusive range of items for which the selection has changed. If the selection mode is multiple interval selection some items within the range might not have changed. The `isAdjusting` flag is `true` if the user is still manipulating the selection, and `false` if the user has finished changing the selection.

The `ListSelectionEvent` object passed into `valueChanged` indicates only that the selection has changed. The event contains no information about the current selection. So, this method queries the selection model to figure out the current selection.

## <a name="api" id="api">The List Selection Listener API</a>

<a name="listselectionlistener" id="listselectionlistener">The ListSelectionListener Interface</a>

**Because `ListSelectionListener` has only one method, it has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[valueChanged(ListSelectionEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListSelectionListener.html#valueChanged-javax.swing.ListSelectionEvent-)</td><td headers="h2">Called in response to selection changes.</td>

<a name="listselectionevent" id="listselectionevent">The ListSelectionEvent API</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Return the object that fired the event. If you register a list selection listener on a list directly, then the source for each event is the list. Otherwise, the source is the selection model.</td>
<td headers="h101">[int getFirstIndex()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListSelectionEvent.html#getFirstIndex--)</td><td headers="h102">Return the index of the first item whose selection value has changed. Note that for multiple interval selection, the first and last items are guaranteed to have changed but items between them might not have. However, when you press ctrl key and move up or down, the lead selection causes the events being fired even though the actual selection has not changed.</td>
<td headers="h101">[int getLastIndex()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListSelectionEvent.html#getLastIndex--)</td><td headers="h102">Return the index of the last item whose selection value has changed. Note that for multiple interval selection, the first and last items are guaranteed to have changed but items between them might not have. However, when you press ctrl key and move up and down, the lead selection causes the events being fired even though the actual selection has not changed.</td>
<td headers="h101">[boolean getValueIsAdjusting()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListSelectionEvent.html#getValueIsAdjusting--)</td><td headers="h102">Return `true` if the selection is still changing. Many list selection listeners are interested only in the final state of the selection and can ignore list selection events when this method returns `true`.</td>

## <a name="eg" id="eg">Examples that Use List Selection Listeners</a>

The following table lists the examples that use list selection listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
