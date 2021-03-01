
# How to Write a Mouse-Wheel Listener

Mouse-wheel events notify when the wheel on the mouse rotates. For information on listening to other mouse events, such as clicks, see [How to Write a Mouse Listener](mouselistener.html). For information on listening to mouse-dragged events, see [How to Write a Mouse-Motion Listener](mousemotionlistener.html). Not all mice have wheels and, in that case, mouse-wheel events are never generated. There is no way to programmatically detect whether the mouse is equipped with a mouse wheel.

Alternatively, use the corresponding 
[`MouseAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseAdapter.html) AWT class, which implements the `MouseWheelListener` interface, to create a `MouseWheelEvent` and override the methods for the specific events.

Usually it is not necessary to implement a mouse-wheel listener because the mouse wheel is used primarily for scrolling. Scroll panes automatically register mouse-wheel listeners that react to the mouse wheel appropriately.

However, to create a custom component to be used inside a scroll pane you may need to customize its scrolling behavior &#226;&#128;&#148; specifically you might need to set the unit and block increments. For a text area, for example, scrolling one unit means scrolling by one line of text. A block increment typically scrolls an entire "page", or the size of the viewport. For more information, see 
[Implementing a Scrolling-Savvy Client](../components/scrollpane.html#scrollable) in the 
[How to Use Scroll Panes](../components/scrollpane.html) page.

To generate mouse-wheel events, the cursor must be **over** the component registered to listen for mouse-wheel events. The type of scrolling that occurs, either `WHEEL_UNIT_SCROLL` or `WHEEL_BLOCK_SCROLL`, is platform dependent. The amount that the mouse wheel scrolls is also platform dependent. Both the type and amount of scrolling can be set via the mouse control panel for the platform.

The following example demonstrates mouse-wheel events.

<li>Click the Launch button to run MouseWheelEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#MouseWheelEventDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the MouseWheelEventDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/MouseWheelEventDemoProject/MouseWheelEventDemo.jnlp)<br /></li>
1. Move the cursor over the text area.
1. Rotate the mouse wheel away from you. You will see one or more mouse-wheel events in the **up** direction.
1. Rotate the mouse wheel in the opposite direction. You will see mouse-wheel events in the **down** direction.
1. Try changing your mouse wheel's scrolling behavior your system's mouse control panel to see how the output changes. You should not need to restart the demo to see the changes take effect.

The output from MouseWheelEventDemo for a system that uses unit increments for its mouse wheel might look as follows:

```

javax.swing.JTextArea: Mouse wheel moved UP 1 notch(es)
    Scroll type: WHEEL_UNIT_SCROLL
    Scroll amount: 3 unit increments per notch
    Units to scroll: -3 unit increments
    Vertical unit increment: 16 pixels

```

The scroll amount, returned by the `getScrollAmount` method, indicates how many units will be scrolled and always presents a positive number. The units to scroll, returned by the `getUnitsToScroll` method, are positive when scrolling down and negative when scrolling up. The number of pixels for the vertical unit is obtained from the vertical scroll bar using the `getUnitIncrement` method. In the preceding example, rolling the mouse wheel one notch upward should result in the text area scrolling upward 48 pixels (3x16).

For a system that uses block increments for mouse-wheel scrolling, for the same movement of the mouse wheel the output might look as follows:

```

javax.swing.JTextArea: Mouse wheel moved UP 1 notch(es)
    Scroll type: WHEEL_BLOCK_SCROLL
    Vertical block increment: 307 pixels

```

The vertical block increment is obtained from the vertical scroll bar using the `getBlockIncrement` method. In this case, rolling the mouse wheel upward one notch means that the text area should scroll upward 307 pixels.

Find the demo's code in the 
[`MouseWheelEventDemo.java`](../examples/events/MouseWheelEventDemoProject/src/events/MouseWheelEventDemo.java) file. The following code snippet is related to the mouse-wheel event handling:

```

public class MouseWheelEventDemo ... implements MouseWheelListener ... {
    public MouseWheelEventDemo() {
        **//where initialization occurs:**
        //Register for mouse-wheel events on the text area.
        textArea.addMouseWheelListener(this);
        ...
    }

    public void mouseWheelMoved(MouseWheelEvent e) {
       String message;
       int notches = e.getWheelRotation();
       if (notches &lt; 0) {
           message = "Mouse wheel moved UP "
                        + -notches + " notch(es)" + newline;
       } else {
           message = "Mouse wheel moved DOWN "
                        + notches + " notch(es)" + newline;
       }
       if (e.getScrollType() == MouseWheelEvent.WHEEL_UNIT_SCROLL) {
           message += "    Scroll type: WHEEL_UNIT_SCROLL" + newline;
           message += "    Scroll amount: " + e.getScrollAmount()
                   + " unit increments per notch" + newline;
           message += "    Units to scroll: " + e.getUnitsToScroll()
                   + " unit increments" + newline;
           message += "    Vertical unit increment: "
               + scrollPane.getVerticalScrollBar().getUnitIncrement(1)
               + " pixels" + newline;
       } else { //scroll type == MouseWheelEvent.WHEEL_BLOCK_SCROLL
           message += "    Scroll type: WHEEL_BLOCK_SCROLL" + newline;
           message += "    Vertical block increment: "
               + scrollPane.getVerticalScrollBar().getBlockIncrement(1)
               + " pixels" + newline;
       }
       saySomething(message, e);
    }
    ...
}

```

## <a name="api" id="api">The Mouse Wheel Listener API</a>

<a name="mousewheellistener" id="mousewheellistener">The MouseWheelListener Interface</a>

**Although `MouseWheelListener` has only one method, it has the corresponding adapter class &#8212; `MouseAdapter`. This capability enables an application to have only one adapter class instance for the component to manage all types of events from the mouse pointer.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[mouseWheelMoved(MouseWheelEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseWheelListener.html#mouseWheelMoved-java.awt.event.MouseWheelEvent-)</td><td headers="h2">Called when the mouse wheel is rotated.</td>

<a name="mousewheelevent" id="mousewheelevent">The MouseWheelEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[int getScrollType()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseWheelEvent.html#getScrollType--)</td><td headers="h102">Returns the type of scrolling to be used. Possible values are `WHEEL_BLOCK_SCROLL` and `WHEEL_UNIT_SCROLL`, and are determined by the native platform.</td>
<td headers="h101">[int getWheelRotation()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseWheelEvent.html#getWheelRotation--)</td><td headers="h102">Returns the number of notches the mouse wheel was rotated. If the mouse wheel rotated towards the user (down) the value is positive. If the mouse wheel rotated away from the user (up) the value is negative.</td>
<td headers="h101">[int getScrollAmount()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseWheelEvent.html#getScrollAmount--)</td><td headers="h102">Returns the number of units that should be scrolled per notch. This is always a positive number and is only valid if the scroll type is `MouseWheelEvent.WHEEL_UNIT_SCROLL`.</td>
<td headers="h101">[int getUnitsToScroll()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/MouseWheelEvent.html#getUnitsToScroll--)</td><td headers="h102">Returns the positive or negative units to scroll for the current event. This is only valid when the scroll type is `MouseWheelEvent.WHEEL_UNIT_SCROLL`.</td>

## <a name="eg" id="eg">Examples That Use Mouse Wheel Listeners</a>

The following table lists the examples that use mouse-wheel listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
