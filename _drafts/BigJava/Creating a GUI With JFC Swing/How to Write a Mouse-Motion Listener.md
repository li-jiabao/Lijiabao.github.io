
# How to Write a Mouse-Motion Listener

Mouse-motion events notify when the user uses the mouse (or a similar input device) to move the onscreen cursor. For information on listening for other kinds of mouse events, such as clicks, see [How to Write a Mouse Listener](mouselistener.html). For information on listening for mouse-wheel events, see [How to Write a Mouse Wheel Listener](mousewheellistener.html).

If an application requires the detection of both mouse events and mouse-motion events, use the 
[`MouseInputAdapter`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MouseInputAdapter.html) class, which implements the 
[`MouseInputListener`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/MouseInputListener.html) a convenient interface that implements both the `MouseListener` and `MouseMotionListener` interfaces.

Alternatively, use the corresponding 
[`MouseAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseAdapter.html) AWT class, which implements the `MouseMotionListener` interface, to create a `MouseMotionEvent` and override the methods for the specific events.

The following demo code contains a mouse-motion listener. This demo is exactly the same as the demo described in the [How to Write a Mouse Listener](mouselistener.html) section, except for substituting the `MouseMotionListener` interface for the `MouseListener` interface. Additionally, MouseMotionEventDemo implements the `mouseDragged` and `mouseMoved` methods instead of the mouse listener methods, and displays coordinates instead of numbers of clicks.

<li>Click the Launch button to run MouseMotionEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#MouseMotionEventDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the MouseMotionEventDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/MouseMotionEventDemoProject/MouseMotionEventDemo.jnlp)<br /></li>
<li>Move the cursor into the yellow rectangle at the top of the window.<br />
You will see one or more mouse-moved events.</li>
<li>Press and hold the mouse button, and then move the mouse so that the cursor is outside the yellow rectangle.<br />
You will see mouse-dragged events.</li>

You can find the demo's code in 
[`MouseMotionEventDemo.java`](../examples/events/MouseMotionEventDemoProject/src/events/MouseMotionEventDemo.java) and 
[`BlankArea.java`](../examples/events/MouseMotionEventDemoProject/src/events/BlankArea.java). The following code snippet from `MouseMotionEventDemo` implements the mouse-motion event handling:

```

public class MouseMotionEventDemo extends JPanel 
                                  implements MouseMotionListener {
    **//...in initialization code:**
        //Register for mouse events on blankArea and panel.
        blankArea.addMouseMotionListener(this);
        addMouseMotionListener(this);
        ...
    }

    public void mouseMoved(MouseEvent e) {
       saySomething("Mouse moved", e);
    }

    public void mouseDragged(MouseEvent e) {
       saySomething("Mouse dragged", e);
    }

    void saySomething(String eventDescription, MouseEvent e) {
        textArea.append(eventDescription 
                        + " (" + e.getX() + "," + e.getY() + ")"
                        + " detected on "
                        + e.getComponent().getClass().getName()
                        + newline);
    }
}

```

The SelectionDemo example, draws a rectangle illustrating the user's current dragging. To draw the rectangle, the application must implement an event handler for three kinds of mouse events: mouse presses, mouse drags, and mouse releases. To be informed of all these events, the handler must implement both the `MouseListener` and `MouseMotionListener` interfaces, and be registered as both a mouse listener and a mouse-motion listener. To avoid having to define empty methods, the handler doesn't implement either listener interface directly. Instead, it extends `MouseInputAdapter`, as the following code snippet shows.

```

**...//where initialization occurs:**
    MyListener myListener = new MyListener();
    addMouseListener(myListener);
    addMouseMotionListener(myListener);
...
private class MyListener extends MouseInputAdapter {
    public void mousePressed(MouseEvent e) {
        int x = e.getX();
        int y = e.getY();
        currentRect = new Rectangle(x, y, 0, 0);
        updateDrawableRect(getWidth(), getHeight());
        repaint();
    }

    public void mouseDragged(MouseEvent e) {
        updateSize(e);
    }

    public void mouseReleased(MouseEvent e) {
        updateSize(e);
    }

    void updateSize(MouseEvent e) {
        int x = e.getX();
        int y = e.getY();
        currentRect.setSize(x - currentRect.x,
                            y - currentRect.y);
        updateDrawableRect(getWidth(), getHeight());
        Rectangle totalRepaint = rectToDraw.union(previouseRectDrawn); 
        repaint(totalRepaint.x, totalRepaint.y,
                totalRepaint.width, totalRepaint.height);
    }
}

```

## <a name="api" id="api">The Mouse-Motion Listener API</a>

<a name="mousemotionlistener" id="mousemotionlistener">The MouseMotionListener Interface</a>

<em>The corresponding adapter classes are 
[`MouseMotionAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseMotionAdapter.html) and 
[`MouseAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseAdapter.html ).</em>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[mouseDragged(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseMotionListener.html#mouseDragged-java.awt.event.MouseEvent-)</td><td headers="h2">Called in response to the user moving the mouse while holding a mouse button down. This event is fired by the component that fired the most recent mouse-pressed event, even if the cursor is no longer over that component.</td>
<td headers="h1">[mouseMoved(MouseEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseMotionListener.html#mouseMoved-java.awt.event.MouseEvent-)</td><td headers="h2">Called in response to the user moving the mouse with no mouse buttons pressed. This event is fired by the component that's currently under the cursor.</td>

Each mouse-motion event method has a single parameter &#226;&#128;&#148; and it's **not** called `MouseMotionEvent`! Instead, each mouse-motion event method uses a `MouseEvent` argument. See [The MouseEvent API](mouselistener.html#mouseevent) for information about using `MouseEvent` objects.

## <a name="eg" id="eg">Examples That Use Mouse-Motion Listeners</a>

The following table lists the examples that use mouse-motion listeners.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
