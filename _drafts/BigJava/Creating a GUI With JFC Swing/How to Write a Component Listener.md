
# How to Write a Component Listener

The Component listener is a listener interface for receiving component events. A component is an object having a graphical representation that can be displayed on the screen and that can interact with the user. Some of the examples of components are the buttons, checkboxes, and scrollbars of a typical graphical user interface.

The class that is interested in processing a component event either implements this interface and all the methods it contains, or extends the abstract ComponentAdapter class overriding only the methods of interest. The listener object created from that class is then registered with a component using the component's addComponentListener method. When the component's size, location, or visibility changes, the relevant method in the listener object is invoked, and the ComponentEvent is passed to it.

One or more component events are fired by a `Component` object just after the component is hidden, made visible, moved, or resized.

The component-hidden and component-shown events occur only as the result of calls to a `Component` 's `setVisible` method. For example, a window might be miniaturized into an icon (iconified) without a component-hidden event being fired.

To write a simple Component listener program, follow the steps mentioned below:

<li>Declare a class which implements Component listener. For example:
<pre><code>
public class ComponentEventDemo ... implements ComponentListener
</code></pre>
</li>
- Identify the components that you would like to catch the events for. For example: pane, label, checkbox, etc.
<li>Add the Component Listener to the identified components. For example:
<pre><code>
....
label.addComponentListener(this);
.....
checkbox.addComponentListener(this);
....
panel.addComponentListener(this);
...
frame.addComponentListener(this);
</code></pre>
</li>
<li>Finally, catch the different events of these components by using four methods of Component Listener as shown below:
<pre><code>
public void componentHidden(ComponentEvent e) {
        displayMessage(e.getComponent().getClass().getName() + " --- Hidden");
    }

    public void componentMoved(ComponentEvent e) {
        displayMessage(e.getComponent().getClass().getName() + " --- Moved");
    }

    public void componentResized(ComponentEvent e) {
        displayMessage(e.getComponent().getClass().getName() + " --- Resized ");            
    }

    public void componentShown(ComponentEvent e) {
        displayMessage(e.getComponent().getClass().getName() + " --- Shown");

    }
</code></pre>
</li>

The following example demonstrates component events.  The window contains a panel that has a label and a check box.  The check box controls whether the label is visible.  A text area displays a message every time the window, panel, label, or check box fires a component event.

<li> Click the Launch button to run ComponentEventDemo using
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).
Alternatively, to compile and run the example yourself, consult the
     [example index](../examples/events/index.html#Beeper).

<center> [<img src="../../images/jws-launch-button.png " width="88" height="23" align="bottom" alt="Launches the ComponentEventDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ComponentEventDemoProject/ComponentEventDemo.jnlp) </center></li>

<li> When the window appears, one or more component-shown
     events have been fired.</li>
<li> Click the check box to hide the label.
     <br />
     The label fires a component-hidden event.  The panel fires component-moved and component-resized events.  The check box fires a component-moved event.</li>

<li> Click the check box again to show the label.
     <br />
     The label fires a component-shown event.  The panel fires component-moved and component-resized events.  The check box fires a component-moved event.</li>

<li> Iconify and then deiconify the window.
     <br />
     You do **not** get component-hidden or -shown events.  If you want to be notified of iconification events, you should use a window listener or a window state listener.</li>

<li> Resize the window.
     <br />
     You will see component-resized (and possibly component-moved) events from all four components &#8212; label, check box, panel, and frame.  If the frame and panel's layout manager did not make every component as wide as possible, the panel, label, and check box would not have been resized.</li>

You can find the demo's code in
[`ComponentEventDemo.java`](../examples/events/ComponentEventDemoProject/src/events/ComponentEventDemo.java).  Here is just the code related to handling component events:

```

public class ComponentEventDemo ... implements ComponentListener {
    static JFrame frame;
    JLabel label;
    ...
    public ComponentEventDemo() {
        ...
        JPanel panel = new JPanel(new BorderLayout());
        label = new JLabel("This is a label", JLabel.CENTER);
        label.addComponentListener(this);
        panel.add(label, BorderLayout.CENTER);

        JCheckBox checkbox = new JCheckBox("Label visible", true);
        checkbox.addComponentListener(this);
        panel.add(checkbox, BorderLayout.PAGE_END);
        panel.addComponentListener(this);
        ...
        frame.addComponentListener(this);
    }
    ...
     public void componentHidden(ComponentEvent e) {
        displayMessage(e.getComponent().getClass().getName() + " --- Hidden");
    }

    public void componentMoved(ComponentEvent e) {
        displayMessage(e.getComponent().getClass().getName() + " --- Moved");
    }

    public void componentResized(ComponentEvent e) {
        displayMessage(e.getComponent().getClass().getName() + " --- Resized ");            
    }

    public void componentShown(ComponentEvent e) {
        displayMessage(e.getComponent().getClass().getName() + " --- Shown");

    }

    public static void main(String[] args) {
        ...
        //Create and set up the window.
        frame = new JFrame("ComponentEventDemo");
        ...
        JComponent newContentPane = new ComponentEventDemo();
        frame.setContentPane(newContentPane);
        ...
    }
}

```

## <a name="api" id="api">The Component Listener API</a>

<a name="componentlistener" id="componentlistener">The ComponentListener Interface</a>

<em>All of these methods are also in the adapter class, 
[`ComponentAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentAdapter.html).</em>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[componentHidden(ComponentEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentListener.html#componentHidden-java.awt.event.ComponentEvent-)</td><td headers="h2">Called after the listened-to component is hidden as the result of the `setVisible` method being called.</td>
<td headers="h1">[componentMoved(ComponentEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentListener.html#componentMoved-java.awt.event.ComponentEvent-)</td><td headers="h2">Called after the listened-to component moves, relative to its container. For example, if a window is moved, the window fires a component-moved event, but the components it contains do not.</td>
<td headers="h1">[componentResized(ComponentEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentListener.html#componentResized-java.awt.event.ComponentEvent-)</td><td headers="h2">Called after the listened-to component's size (rectangular bounds) changes.</td>
<td headers="h1">[componentShown(ComponentEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentListener.html#componentShown-java.awt.event.ComponentEvent-)</td><td headers="h2">Called after the listened-to component becomes visible as the result of the `setVisible` method being called.</td>

<a name="componentevent" id="componentevent">The ComponentEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Component getComponent()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentEvent.html#getComponent--)</td><td headers="h102">Returns the component that fired the event. You can use this instead of the `getSource` method.</td>

## Examples that Use Component Listeners
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
