
# How to Write a Focus Listener

Focus events are fired whenever a component gains or loses the keyboard focus. This is true whether the change in focus occurs through the mouse, the keyboard, or programmatically. To get familiar with basic focus concepts or to obtain detailed information about focus, see 
[How to Use the Focus Subsystem](../misc/focus.html).

This section explains how to get focus events for a particular component by registering a `FocusListener` instance on it. To get focus for a window only, implement a [`WindowFocusListener`](windowlistener.html) instance instead. To obtain the focus status of many components, consider implementing a `PropertyChangeListener` instance on the `KeyboardFocusManager` class, as described in 
[Tracking Focus Changes to Multiple Components](../misc/focus.html#trackingFocus) in 
[How to Use the Focus Subsystem](../misc/focus.html).

The following example demonstrates focus events. The window displays a variety of components. A focus listener, registered on each component, reports every focus-gained and focus-lost event. For each event, the other component involved in the focus change, the **opposite component**, is reported. For example, when the focus goes from a button to a text field, a focus-lost event is fired by the button (with the text field as the opposite component) and then a focus-gained event is fired by the text field (with the button as the opposite component). Focus-lost as well as focus-gained events can be temporary. For example, a temporary focus-lost event occurs when the window loses the focus. A temporary focus-gained event occurs on popup menus.

## Running the Example

<li>Click the Launch button to run FocusEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#FocusEventDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the FocusEventDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FocusEventDemoProject/FocusEventDemo.jnlp)<br /></li>
1. You will see a "Focus gained: JTextField" message in the text area &#226;&#128;&#148; its "opposite component" is null, since it is the first component to have the focus.
1. Click the label. Nothing happens because the label, by default, cannot get the focus.
<li>Click the combo box. A focus-lost event is fired by the text field and a focus-gained event by the combo box. The combo box now shows that it has the focus, perhaps with a dotted line around the text &#226;&#128;&#148; exactly how this is represented is look and feel dependent.<br />
Notice that when the focus changes from one component to another, the first component fires a focus-lost event before the second component fires a focus-gained event.</li>
1. Select a choice from the combo box's menu. Click the combo box again. Notice that no focus event is reported. As long as the user manipulates the same component, the focus stays on that component.
1. Click the text area where the focus events are printed. Nothing happens because the text area has been rendered unclickable with `setRequestFocusEnabled(false)`.
1. Click the text field to return the focus to the initial component.
1. Press Tab on the keyboard. The focus moves to the combo box and skips over the label.
1. Press Tab again. The focus moves to the button.
1. Click another window so that the FocusEventDemo window loses the focus. A temporary focus-lost event is generated for the button.
1. Click the top of the FocusEventDemo window. A focus-gained event is fired by the button.
1. Press Tab on the keyboard. The focus moves to the list.
<li>Press Tab again. The focus moves to the text area.<br />
Notice that even though you are not allowed to click on the text area, you can tab to it. This is so users who use 
[assistive technologies](../misc/access.html) can determine that a component is there and what it contains. The demo disables click-to-focus for the text area, while retaining its tab-to-focus capability, by invoking `setRequestFocusEnabled(false)` on the text area. The demo could use `setFocusable(false)` to truly remove the text area from the focus cycle, but that would have the unfortunate effect of making the component unavailable to those who use assistive technologies.</li>
<li>Press Tab again. The focus moves from the list back to the text field. You have just completed a **focus cycle**. See the 
[introduction](../misc/focus.html#intro) in 
[How to Use the Focus Subsystem](../misc/focus.html) for a discussion of focus terminology and concepts.</li>

The complete code for this demo is in the 
[`FocusEventDemo.java`](../examples/events/FocusEventDemoProject/src/events/FocusEventDemo.java) file. The following code snippet represents the focus-event handling mechanism:

```

public class FocusEventDemo ... implements FocusListener ... {
    public FocusEventDemo() {
        ...
        JTextField textField = new JTextField("A TextField");
        textField.addFocusListener(this);
        ...
        JLabel label = new JLabel("A Label");
        label.addFocusListener(this);
        ...
        JComboBox comboBox = new JComboBox(vector);
        comboBox.addFocusListener(this);
        ...
        JButton button = new JButton("A Button");
        button.addFocusListener(this);
        ...
        JList list = new JList(listVector);
        list.setSelectedIndex(1); //It's easier to see the focus change
                                  //if an item is selected.
        list.addFocusListener(this);
        JScrollPane listScrollPane = new JScrollPane(list);
        
        ...

        //Set up the area that reports focus-gained and focus-lost events.
        display = new JTextArea();
        display.setEditable(false);
        //The method setRequestFocusEnabled prevents a
        //component from being clickable, but it can still
        //get the focus through the keyboard - this ensures
        //user accessibility.
        display.setRequestFocusEnabled(false);
        display.addFocusListener(this);
        JScrollPane displayScrollPane = new JScrollPane(display);

        ...
    }
    ...
    public void focusGained(FocusEvent e) {
        displayMessage("Focus gained", e);
    }

    public void focusLost(FocusEvent e) {
        displayMessage("Focus lost", e);
    }

    void displayMessage(String prefix, FocusEvent e) {
        display.append(prefix
                       + (e.isTemporary() ? " (temporary):" : ":")
                       +  e.getComponent().getClass().getName()
                       + "; Opposite component: " 
                       + (e.getOppositeComponent() != null ?
                          e.getOppositeComponent().getClass().getName() : "null")
                       + newline); 
    }
    ...
}

```

## <a name="api" id="api">The Focus Listener API</a>

<a name="focuslistener" id="focuslistener">The FocusListener Interface</a>

<em>The corresponding adapter class is 
[`FocusAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/FocusAdapter.html).</em>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[focusGained(FocusEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/FocusListener.html#focusGained-java.awt.event.FocusEvent-)</td><td headers="h2">Called just after the listened-to component gets the focus.</td>
<td headers="h1">[focusLost(FocusEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/FocusListener.html#focusLost-java.awt.event.FocusEvent-)</td><td headers="h2">Called just after the listened-to component loses the focus.</td>

<a name="focusevent" id="focusevent">The FocusEvent API</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[boolean isTemporary()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/FocusEvent.html#isTemporary--)</td><td headers="h102">Returns the true value if a focus-lost or focus-gained event is temporary.</td>
<td headers="h101">[Component getComponent()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentEvent.html#getComponent--)<br />(**in `java.awt.event.ComponentEvent`**)</td><td headers="h102">Returns the component that fired the focus event.</td>
<td headers="h101">[Component getOppositeComponent()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/FocusEvent.html#getOppositeComponent--)</td><td headers="h102">Returns the other component involved in the focus change. For a `FOCUS_GAINED` event, this is the component that lost the focus. For a `FOCUS_LOST` event, this is the component that gained the focus. If the focus change involves a native application, a Java application in a different VM or context, or no other component, then `null` is returned.</td>

## <a name="eg" id="eg">Examples that Use Focus Listeners</a>

The following table lists the examples that use focus listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
