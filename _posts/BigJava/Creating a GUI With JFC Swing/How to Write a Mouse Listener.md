
# How to Write a Mouse Listener

Mouse events notify when the user uses the mouse (or similar input device) to interact with a component. Mouse events occur when the cursor enters or exits a component's onscreen area and when the user presses or releases one of the mouse buttons.

Tracking the cursor's motion involves significantly more system overhead than tracking other mouse events. That is why mouse-motion events are separated into Mouse Motion listener type (see [How to Write a Mouse Motion Listener](mousemotionlistener.html)).

To track mouse wheel events, you can register a mouse-wheel listener. See [How to Write a Mouse Wheel Listener](mousewheellistener.html) for more information.

If an application requires the detection of both mouse events and mouse-motion events, use the 
[`MouseInputAdapter`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MouseInputAdapter.html) class. This class implements the 
[`MouseInputListener`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MouseInputListener.html), a convenient interface that implements the `MouseListener` and `MouseMotionListener` interfaces. However, the `MouseInputListener` interface does not implement the `MouseWheelListener` interface.

Alternatively, use the corresponding AWT 
[`MouseAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseAdapter.html) class, which implements the `MouseListener`, `MouseMotionListener`, and `MouseWheelListener` interfaces.

The following example shows a mouse listener. At the top of the window is a blank area (implemented by a class named `BlankArea`). The mouse listener listens for events both on the `BlankArea` and on its container, an instance of `MouseEventDemo`. Each time a mouse event occurs, a descriptive message is displayed under the blank area. By moving the cursor on top of the blank area and occasionally pressing mouse buttons, you can fire mouse events.

<li>Click the Launch button to run MouseEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#MouseEventDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the MouseEventDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/MouseEventDemoProject/MouseEventDemo.jnlp)<br /></li>
<li>Move the cursor into the yellow rectangle at the top of the window.<br />
You will see one or more mouse-entered events.</li>
<li>Press and hold the left mouse button without moving the mouse.<br />
You will see a mouse-pressed event. You might see some extra mouse events, such as mouse-exited and then mouse-entered.</li>
<li>Release the mouse button.<br />
You will see a mouse-released event. If you did not move the mouse, a mouse-clicked event will follow.</li>
<li>Press and hold the mouse button again, and then drag the mouse so that the cursor ends up outside the window. Release the mouse button.<br />
You will see a mouse-pressed event, followed by a mouse-exited event, followed by a mouse-released event. You are **not** notified of the cursor's motion. To get mouse-motion events, you need to implement a [mouse-motion listener.](mousemotionlistener.html)</li>

You can find the demo's code in 
[`MouseEventDemo.java`](../examples/events/MouseEventDemoProject/src/events/MouseEventDemo.java) and 
[`BlankArea.java`](../examples/events/MouseEventDemoProject/src/events/BlankArea.java). Here is the demo's mouse event handling code:

```

public class MouseEventDemo ... implements MouseListener {
        **//where initialization occurs:**
        //Register for mouse events on blankArea and the panel.
        blankArea.addMouseListener(this);
        addMouseListener(this);
    ...

    public void mousePressed(MouseEvent e) {
       saySomething("Mouse pressed; # of clicks: "
                    + e.getClickCount(), e);
    }

    public void mouseReleased(MouseEvent e) {
       saySomething("Mouse released; # of clicks: "
                    + e.getClickCount(), e);
    }

    public void mouseEntered(MouseEvent e) {
       saySomething("Mouse entered", e);
    }

    public void mouseExited(MouseEvent e) {
       saySomething("Mouse exited", e);
    }

    public void mouseClicked(MouseEvent e) {
       saySomething("Mouse clicked (# of clicks: "
                    + e.getClickCount() + ")", e);
    }

    void saySomething(String eventDescription, MouseEvent e) {
        textArea.append(eventDescription + " detected on "
                        + e.getComponent().getClass().getName()
                        + "." + newline);
    }
}

```

## <a name="api" id="api">The Mouse Listener API</a>

<a name="mouselistener" id="mouselistener">The MouseListener Interface</a>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[mouseClicked(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseListener.html#mouseClicked-java.awt.event.MouseEvent-)</td><td headers="h2">Called just after the user clicks the listened-to component.</td>
<td headers="h1">[mouseEntered(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseListener.html#mouseEntered-java.awt.event.MouseEvent-)</td><td headers="h2">Called just after the cursor enters the bounds of the listened-to component.</td>
<td headers="h1">[mouseExited(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseListener.html#mouseExited-java.awt.event.MouseEvent-)</td><td headers="h2">Called just after the cursor exits the bounds of the listened-to component.</td>
<td headers="h1">[mousePressed(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseListener.html#mousePressed-java.awt.event.MouseEvent-)</td><td headers="h2">Called just after the user presses a mouse button while the cursor is over the listened-to component.</td>
<td headers="h1">[mouseReleased(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseListener.html#mouseReleased-java.awt.event.MouseEvent-)</td><td headers="h2">Called just after the user releases a mouse button after a mouse press over the listened-to component.</td>

The 
[`MouseAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseAdapter.html) class (the AWT adapter class) is abstract. All its methods have an empty body. So a developer can define methods for events specific to the application. You can also use the 
[`MouseInputAdapter`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MouseInputAdapter.html) class, which has all the methods available from `MouseListener` and `MouseMotionListener`.

<a name="mouseevent" id="mouseevent">The MouseEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[int getClickCount()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getClickCount--)</td><td headers="h102">Returns the number of quick, consecutive clicks the user has made (including this event). For example, returns 2 for a double click.</td>
<td headers="h101">[int getX()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getX--)<br />[int getY()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getY--)<br />[Point getPoint()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getPoint--)</td><td headers="h102">Return the (x,y) position at which the event occurred, relative to the component that fired the event.</td>
<td headers="h101">[int getXOnScreen()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getXOnScreen--)<br />[int getYOnScreen()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getYOnScreen--)<br />[int getLocationOnScreen()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getLocationOnScreen--)</td><td headers="h102">Return the absolute (x,y) position of the event. These coordinates are relative to the virtual coordinate system for the multi-screen environment. Otherwise, these coordinates are relative to the coordinate system associated with the Component's Graphics Configuration.</td>
<td headers="h101">[int getButton()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getButton--)</td><td headers="h102">Returns which mouse button, if any, has a changed state. One of the following constants is returned: `NOBUTTON`, `BUTTON1`, `BUTTON2`, or `BUTTON3`.</td>
<td headers="h101">[boolean isPopupTrigger()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#isPopupTrigger--)</td><td headers="h102">Returns `true` if the mouse event should cause a popup menu to appear. Because popup triggers are platform dependent, if your program uses popup menus, you should call `isPopupTrigger` for all mouse-pressed and mouse-released events fired by components over which the popup can appear. See [Bringing Up a Popup Menu](../components/menu.html#popup) for more information about popup menus.</td>
<td headers="h101">[String getMouseModifiersText(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseEvent.html#getMouseModifiersText-int-)</td><td headers="h102">Returns a `String` describing the modifier keys and mouse buttons that were active during the event, such as "Shift", or "Ctrl+Shift". These strings can be localized using the awt.properties file.</td>

<a name="inputevent" id="inputevent">The InputEvent Class</a>

The `MouseEvent` class inherits many useful methods from 
[InputEvent](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html) and a couple handy methods from the 
[`ComponentEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentEvent.html) and 
[`AWTEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/AWTEvent.html) classes.
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[int getID()](https://docs.oracle.com/javase/8/docs/api/java/awt/AWTEvent.html#getID--)<br />(**in `java.awt.AWTEvent`**)</td><td headers="h202">Returns the event type, which defines the particular action. For example, the MouseEvent id reflects the state of the mouse buttons for every mouse event. The following states could be specified by the MouseEvent id: `MouseEvent.MOUSE_PRESSED`, `MouseEvent.MOUSE_RELEASED`, and `MouseEvent.MOUSE_CLICKED`.</td>
<td headers="h201">[Component getComponent()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentEvent.html#getComponent--)<br />(**in `ComponentEvent`**)</td><td headers="h202">Returns the component that fired the event. You can use this method instead of the `getSource` method.</td>
<td headers="h201">[int getWhen()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#getWhen--)</td><td headers="h202">Returns the timestamp of when this event occurred. The higher the timestamp, the more recently the event occurred.</td>
<td headers="h201">[boolean isAltDown()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#isAltDown--)<br />[boolean isControlDown()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#isControlDown--)<br />[boolean isMetaDown()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#isMetaDown--)<br />[boolean isShiftDown()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#isShiftDown--)<br /></td><td headers="h202">Return the state of individual modifier keys at the time the event was fired.</td>
<td headers="h201">[int getModifiers()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#getModifiers--)</td><td headers="h202">Returns the state of all the modifier keys and mouse buttons when the event was fired. You can use this method to determine which mouse button was pressed (or released) when a mouse event was fired. The `InputEvent` class defines these constants for use with the `getModifiers` method: `ALT_MASK`, `BUTTON1_MASK`, `BUTTON2_MASK`, `BUTTON3_MASK`, `CTRL_MASK`, `META_MASK`, and `SHIFT_MASK`. For example, the following expression is true if the right button was pressed:<pre>`(mouseEvent.getModifiers() &amp; InputEvent.BUTTON3_MASK)== InputEvent.BUTTON3_MASK`</pre></td>
<td headers="h201">[int getModifiersEx()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#getModifiersEx--)</td><td headers="h202">Returns the extended modifier mask for this event. Extended modifiers represent the state of the mouse button and all modal keys, such as ALT, CTRL, META, just after the event occurred. You can check the status of the modifiers using one of the following predefined bitmasks: `SHIFT_DOWN_MASK`, `CTRL_DOWN_MASK`, `META_DOWN_MASK`, `ALT_DOWN_MASK`, `BUTTON1_DOWN_MASK`, `BUTTON2_DOWN_MASK`, `BUTTON3_DOWN_MASK`, or `ALT_GRAPH_DOWN_MASK`. For example, to check that button 1 is down, but that buttons 2 and 3 are up, you would use the following code snippet:<pre>`if (event.getModifiersEx() &amp; (BUTTON1_DOWN_MASK |                              BUTTON2_DOWN_MASK |                              BUTTON3_DOWN_MASK)                               == BUTTON1_DOWN_MASK) {    ...}`</pre></td>
<td headers="h201">[int getModifiersExText(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#getModifiersExText-int-)</td><td headers="h202">Returns a string describing the extended modifier keys and mouse buttons, such as "Shift", "Button1", or "Ctrl+Shift". These strings can be localized by changing the awt.properties file.</td>

<a name="mouseinfo" id="mouseinfo">The MouseInfo Class</a>

The 
[`MouseInfo`](https://docs.oracle.com/javase/8/docs/api/java/awt/MouseInfo.html) class provides methods to obtain information about the mouse pointer location at any time while an application runs.
<th id="h301" align="left">Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[getPointerInfo()](https://docs.oracle.com/javase/8/docs/api/java/awt/MouseInfo.html#getPointerInfo--)</td><td headers="h302">Returns a `PointerInfo` instance that represents the current location of the mouse pointer.</td>
<td headers="h301">[getNumberOfButtons()](https://docs.oracle.com/javase/8/docs/api/java/awt/MouseInfo.html#getNumberOfButtons--)</td><td headers="h302">Returns the number of buttons on the mouse or `-1` , if a system does not support a mouse.</td>

## <a name="eg" id="eg">Examples That Use Mouse Listeners</a>

The following table lists the examples that use mouse listeners.
<th id="h401" align="left">Example</th><th id="h402" align="left">Where Described</th><th id="h403" align="left">Notes</th>
