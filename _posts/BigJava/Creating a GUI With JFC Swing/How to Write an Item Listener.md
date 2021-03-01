
# How to Write an Item Listener

Item events are fired by components that implement the 
[`ItemSelectable`](https://docs.oracle.com/javase/8/docs/api/java/awt/ItemSelectable.html) interface. Generally, `ItemSelectable` components maintain on/off state for one or more items. The Swing components that fire item events include buttons like 
[check boxes](../components/button.html#checkbox), 
[check menu items](../components/menu.html), 
[toggle buttons](../components/button.html) etc...and 
[combo boxes](../components/combobox.html).

Here is some item-event handling code taken from 
[`ComponentEventDemo.java`](../examples/events/ComponentEventDemoProject/src/events/ComponentEventDemo.java):

```

**//where initialization occurs**
checkbox.addItemListener(this);
...
public void itemStateChanged(ItemEvent e) {
    if (e.getStateChange() == ItemEvent.SELECTED) {
        label.setVisible(true);
        ...
    } else {
        label.setVisible(false);
    }
}

```

## <a name="api" id="api">The Item Listener API</a>

<a name="itemlistener" id="itemlistener">The ItemListener Interface</a>

**Because `ItemListener` has only one method, it has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[itemStateChanged(ItemEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ItemListener.html#itemStateChanged-java.awt.event.ItemEvent-)</td><td headers="h2">Called just after a state change in the listened-to component.</td>

<a name="itemevent" id="itemevent">The ItemEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getItem()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ItemEvent.html#getItem--)</td><td headers="h102">Returns the component-specific object associated with the item whose state changed. Often this is a `String` containing the text on the selected item.</td>
<td headers="h101">[ItemSelectable getItemSelectable()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ItemEvent.html#getItemSelectable--)</td><td headers="h102">Returns the component that fired the item event. You can use this instead of the `getSource` method.</td>
<td headers="h101">[int getStateChange()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ItemEvent.html#getStateChange--)</td><td headers="h102">Returns the new state of the item. The `ItemEvent` class defines two states: `SELECTED` and `DESELECTED`.</td>

## <a name="eg" id="eg">Examples that Use Item Listeners</a>

The following table lists some examples that use item listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
