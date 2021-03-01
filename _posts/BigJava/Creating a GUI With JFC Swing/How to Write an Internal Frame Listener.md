
# How to Write an Internal Frame Listener

An `InternalFrameListener` is similar to a `WindowListener`. Like the window listener, the internal frame listener listens for events that occur when the "window" has been shown for the first time, disposed of, iconified, deiconified, activated, or deactivated. Before using an internal frame listener, please familiarize yourself with the `WindowListener` interface in [How to Write Window Listeners](windowlistener.html).

The application shown in the following figure demonstrates internal frame events. The application listens for internal frame events from the Event Generator frame, displaying a message that describes each event.

<li>Click the Launch button to run InternalFrameEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#InternalFrameEventDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the InternalFrameEventDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/InternalFrameEventDemoProject/InternalFrameEventDemo.jnlp)<br /></li>
<li>Bring up the Event Generator internal frame by clicking the **Show internal frame** button.<br />
You should see an "Internal frame opened" message in the display area.</li>
<li>Try various operations to see what happens. For example, click the Event Generator so that it gets activated. Click the Event Watcher so that the Event Generator gets deactivated. Click the Event Generator's decorations to iconify, maximize, minimize, and close the window.<br />
See [How to Write Window Listeners](windowlistener.html) for information on what kinds of events you will see.</li>

Here is the internal frame event handling code:

```

public class InternalFrameEventDemo ...
                     implements InternalFrameListener ... {
    ...

    public void internalFrameClosing(InternalFrameEvent e) {
        displayMessage("Internal frame closing", e);
    }

    public void internalFrameClosed(InternalFrameEvent e) {
        displayMessage("Internal frame closed", e);
        listenedToWindow = null;
    }

    public void internalFrameOpened(InternalFrameEvent e) {
        displayMessage("Internal frame opened", e);
    }

    public void internalFrameIconified(InternalFrameEvent e) {
        displayMessage("Internal frame iconified", e);
    }

    public void internalFrameDeiconified(InternalFrameEvent e) {
        displayMessage("Internal frame deiconified", e);
    }

    public void internalFrameActivated(InternalFrameEvent e) {
        displayMessage("Internal frame activated", e);
    }

    public void internalFrameDeactivated(InternalFrameEvent e) {
        displayMessage("Internal frame deactivated", e);
    }

    void displayMessage(String prefix, InternalFrameEvent e) {
        String s = prefix + ": " + e.getSource(); 
        display.append(s + newline);
    }

    public void actionPerformed(ActionEvent e) {
        if (SHOW.equals(e.getActionCommand())) {
            ...
            if (listenedToWindow == null) {
                listenedToWindow = new JInternalFrame("Event Generator",
                                                      true,  //resizable
                                                      true,  //closable
                                                      true,  //maximizable
                                                      true); //iconifiable
                //We want to reuse the internal frame, so we need to
                //make it hide (instead of being disposed of, which is
                //the default) when the user closes it.
                listenedToWindow.setDefaultCloseOperation(
                                        WindowConstants.HIDE_ON_CLOSE);

                listenedToWindow.addInternalFrameListener(this);
                ...
            }
        } 
        ...
    }
}

```

## <a name="api" id="api">The Internal Frame Listener API</a>

<a name="internalframelistener" id="internalframelistener">The InternalFrameListener Interface</a>

<em>The corresponding adapter class is 
[`InternalFrameAdapter`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameAdapter.html).</em>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[internalFrameOpened(InternalFrameEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameListener.html#internalFrameOpened-javax.swing.event.InternalFrameEvent-)</td><td headers="h2">Called just after the listened-to internal frame has been shown for the first time.</td>
<td headers="h1">[internalFrameClosing(InternalFrameEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameListener.html#internalFrameClosing-javax.swing.event.InternalFrameEvent-)</td><td headers="h2">Called in response to a user request that the listened-to internal frame be closed. By default, `JInternalFrame` hides the window when the user closes it. You can use the `JInternalFrame` `setDefaultCloseOperation` method to specify another option, which must be either `DISPOSE_ON_CLOSE` or `DO_NOTHING_ON_CLOSE` (both defined in `WindowConstants`, an interface that `JInternalFrame` implements). Or by implementing an `internalFrameClosing` method in the internal frame's listener, you can add custom behavior (such as bringing up dialogs or saving data) to internal frame closing.</td>
<td headers="h1">[internalFrameClosed(InternalFrameEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameListener.html#internalFrameClosed-javax.swing.event.InternalFrameEvent-)</td><td headers="h2">Called just after the listened-to internal frame has been disposed of.</td>
<td headers="h1">[internalFrameIconified(InternalFrameEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameEvent.html#internalFrameIconified-javax.swing.event.InternalFrameEvent-)<br />[internalFrameDeiconified(InternalFrameEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameEvent.html#internalFrameDeiconified-javax.swing.event.InternalFrameEvent-)</td><td headers="h2">Called just after the listened-to internal frame is iconified or deiconified, respectively.</td>
<td headers="h1">[internalFrameActivated(InternalFrameEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameListener.html#internalFrameActivated-javax.swing.event.InternalFrameEvent-)<br />[internalFrameDeactivated(InternalFrameEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameListener.html#internalFrameDeactivated-javax.swing.event.InternalFrameEvent-)</td><td headers="h2">Called just after the listened-to internal frame is activated or deactivated, respectively.</td>

Each internal frame event method has a single parameter: an 
[`InternalFrameEvent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/InternalFrameEvent.html) object. The `InternalFrameEvent` class defines no generally useful methods. To get the internal frame that fired the event, use the `getSource` method, which `InternalFrameEvent` inherits from `java.util.EventObject`.

## <a name="eg" id="eg">Examples that Use Internal Frame Listeners</a>

No other source files currently contain internal frame listeners. However, internal frame listeners are very similar to `WindowListener`s and several Swing programs have window listeners:
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
