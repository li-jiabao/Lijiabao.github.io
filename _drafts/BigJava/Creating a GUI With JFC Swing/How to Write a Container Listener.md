
# How to Write a Container Listener

Container events are fired by a `Container` just after a component is added to or removed from the container. These events are for notification only &#8212; no container listener need be present for components to be successfully added or removed.

The following example demonstrates container events. By clicking **Add a button** or **Remove a button**, you can add buttons to or remove them from a panel at the bottom of the window. Each time a button is added to or removed from the panel, the panel fires a container event, and the panel's container listener is notified. The listener displays descriptive messages in the text area at the top of the window.

<li>Click the Launch button to run ContainerEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#ContainerEventDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ContainerEventDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ContainerEventDemoProject/ContainerEventDemo.jnlp)<br /></li>
<li>Click the button labeled **Add a button**.<br />
You will see a button appear near the bottom of the window. The container listener reacts to the resulting component-added event by displaying "JButton #1 was added to javax.swing.JPanel" at the top of the window.</li>
<li>Click the button labeled **Remove a button**.<br />
This removes the most recently added button from the panel, causing the container listener to receive a component-removed event.</li>

You can find the demo's code in 
[`ContainerEventDemo.java`](../examples/events/ContainerEventDemoProject/src/events/ContainerEventDemo.java). Here is the demo's container event handling code:

```

public class ContainerEventDemo ... implements ContainerListener ... {
    **...//where initialization occurs:**
        buttonPanel = new JPanel(new GridLayout(1,1));
        buttonPanel.addContainerListener(this);
    ...
    public void componentAdded(ContainerEvent e) {
        displayMessage(" added to ", e);
    }

    public void componentRemoved(ContainerEvent e) {
        displayMessage(" removed from ", e);
    }

    void displayMessage(String action, ContainerEvent e) {
        display.append(((JButton)e.getChild()).getText()
                       + " was"
                       + action
                       + e.getContainer().getClass().getName()
                       + newline);
    }
    ...
}

```

## <a name="api" id="api">The Container Listener API</a>

<a name="containerlistener" id="containerlistener">The ContainerListener Interface</a>

<em>The corresponding adapter class is 
[`ContainerAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ContainerAdapter.html).</em>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[componentAdded(ContainerEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ContainerListener.html#componentAdded-java.awt.event.ContainerEvent-)</td><td headers="h2">Called just after a component is added to the listened-to container.</td>
<td headers="h1">[componentRemoved(ContainerEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ContainerListener.html#componentRemoved-java.awt.event.ContainerEvent-)</td><td headers="h2">Called just after a component is removed from the listened-to container.</td>

<a name="containerevent" id="containerevent">The ContainerEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Component getChild()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ContainerEvent.html#getChild--)</td><td headers="h102">Returns the component whose addition or removal triggered this event.</td>
<td headers="h101">[Container getContainer()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ContainerEvent.html#getContainer--)</td><td headers="h102">Returns the container that fired this event. You can use this instead of the `getSource` method.</td>

<a name="eg" id="eg"></a>

## Examples that Use Container Listeners

The following table lists the examples that use container listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
