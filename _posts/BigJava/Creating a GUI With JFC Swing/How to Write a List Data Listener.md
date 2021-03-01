
# How to Write a List Data Listener

List data events occur when the contents of a mutable 
[list](../components/list.html) change. Since the model &#151; not the component &#151; fires these events, you have to register a list data listener with the list model. If you have not explicitly created a list with a mutable list model, then your list is immutable, and its model will not fire these events.


[Combo box](../components/combobox.html) models also fire list data events. However, you normally do not need to know about them unless you are 
[creating a custom combo box model](../components/combobox.html#datsun).

The following example demonstrates list data events on a mutable list:

<li>Click the Launch button to run ListDataEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#ListDataEventDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ListDataEventDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ListDataEventDemoProject/ListDataEventDemo.jnlp)<br /></li>
1. Type in the name of your favorite ski resort and click the **Add** button. An `intervalAdded` event was fired.
1. Select a few continguous items in the list and click the **Delete** button. An `intervalRemoved` event was fired.
1. Select one item and move it up or down in the list with the arrow buttons. Two `contentsChanged` events are fired &#226;&#128;&#148; one for the item that moved and one for the item that was displaced.

You can find the demo's code in 
[`ListDataEventDemo.java`](../examples/events/ListDataEventDemoProject/src/events/ListDataEventDemo.java). Here is the code that registers a list data listener on the list model and implements the listener:

```

//**...where member variables are declared...**
private DefaultListModel listModel;
...
//Create and populate the list model
listModel = new DefaultListModel();
...
listModel.addListDataListener(new MyListDataListener());

class MyListDataListener implements ListDataListener {
    public void contentsChanged(ListDataEvent e) {
        log.append("contentsChanged: " + e.getIndex0() +
                   ", " + e.getIndex1() + newline);
    }
    public void intervalAdded(ListDataEvent e) {
        log.append("intervalAdded: " + e.getIndex0() +
                   ", " + e.getIndex1() + newline);
    }
    public void intervalRemoved(ListDataEvent e) {
        log.append("intervalRemoved: " + e.getIndex0() +
                   ", " + e.getIndex1() + newline);
    }
} 

```

## <a name="api" id="api">The List Data Listener API</a>

<a name="listdatalistener" id="listdatalistener">The ListDataListener Interface</a>

**`ListDataListener` has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[intervalAdded(ListDataEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListDataListener.html#intervalAdded-javax.swing.event.ListDataEvent-)</td><td headers="h2">Called when one or more items have been added to the list.</td>
<td headers="h1">[intervalRemoved(ListDataEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListDataListener.html#intervalRemoved-javax.swing.event.ListDataEvent-)</td><td headers="h2">Called when one or more items have been removed from the list.</td>
<td headers="h1">[contentsChanged(ListDataEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListDataListener.html#contentsChanged-javax.swing.event.ListDataEvent-)</td><td headers="h2">Called when the contents of one or more items in the list have changed.</td>

<a name="listdataevent" id="listdataevent">The ListDataEvent API</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Return the object that fired the event.</td>
<td headers="h101">[int getIndex0()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListDataEvent.html#getIndex0--)</td><td headers="h102">Return the index of the first item whose value has changed.</td>
<td headers="h101">[int getIndex1()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListDataEvent.html#getIndex1--)</td><td headers="h102">Return the index of the last item whose value has changed.</td>
<td headers="h101">[int getType()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ListDataEvent.html#getType--)</td><td headers="h102">Return the event type. The possible values are: `CONTENTS_CHANGED`, `INTERVAL_ADDED`, or `INTERVAL_REMOVED`.</td>

## <a name="eg" id="eg">Examples that Use List Data Listeners</a>

The following table lists the examples that use list data listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
