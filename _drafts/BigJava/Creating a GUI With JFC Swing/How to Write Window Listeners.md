
# How to Write Window Listeners

This section explains how to implement three kinds of window-related event handlers: `WindowListener`, `WindowFocusListener`, and `WindowStateListener`. All three listeners handle `WindowEvent` objects. The methods in all three event handlers are implemented by the abstract `WindowAdapter` class.

When the appropriate listener has been registered on a window (such as a 
[frame](../components/frame.html) or 
[dialog](../components/dialog.html)), window events are fired just after the window activity or state has occurred. A window is considered as a "focus owner", if this window receives keyboard input.

The following window activities or states can precede a window event:

- Opening a window &#8212; Showing a window for the first time.
- Closing a window &#8212; Removing the window from the screen.
- Iconifying a window &#8212; Reducing the window to an icon on the desktop.
- Deiconifying a window &#8212; Restoring the window to its original size.
- Focused window &#8212; The window which contains the "focus owner".
- Activated window (frame or dialog) &#8212; This window is either the focused window, or owns the focused window.
<li>Deactivated window &#8212; This window has lost the focus. For more information about focus, see the 
[AWT Focus Subsystem](https://docs.oracle.com/javase/6/docs/api/java/awt/doc-files/FocusSpec.html) specification.</li>
<li>Maximizing the window &#8212; Increasing a window's size to the maximum allowable size, either in the vertical direction, the horizontal direction, or both directions.
The `WindowListener` interface defines methods that handle most window events, such as the events for opening and closing the window, activation and deactivation of the window, and iconification and deiconification of the window.
The other two window listener interfaces are `WindowFocusListener` and `WindowStateListener`. `WindowFocusListener` contains methods to detect when the window becomes the focus owner or it loses the focus owner status. `WindowStateListener` has a single method to detect a change to the state of the window, such as when the window is iconified, deiconified, maximized, or restored to normal.
While you can use the `WindowListener` methods to detect some window states, such as iconification, there are two reasons why a `WindowStateListener` might be preferable: it has only one method for you to implement, and it provides support for maximization.
<hr />**Note:**&#160;Not all window managers/native platforms support all window states. The `java.awt.Toolkit` method 
[`isFrameStateSupported(int)`](https://docs.oracle.com/javase/8/docs/api/java/awt/Toolkit.html#isFrameStateSupported-int-) can be used to determine whether a particular window state is supported by a particular window manager. The WindowEventDemo example, described later in this section, shows how this method can be used.
<hr />
Window listeners are commonly used to implement custom window-closing behavior. For example, a window listener is used to save data before closing the window, or to exit the program when the last window closes.
<p>A user does not necessarily need to implement a window listener to specify what a window should do when the user closes it. By default, when the user closes a window the window becomes invisible. To specify different behavior, use the `setDefaultCloseOperation` method of the `JFrame` or `JDialog` classes. To implement a window-closing handler, use the `setDefaultCloseOperation(WindowConstants.DO_NOTHING_ON_CLOSE)` method to enable the window listener to provide all window-closing duties. See 
[Responding to Window-Closing Events](../components/frame.html#windowevents) for details on how to use `setDefaultCloseOperation`.</p>
When the last displayable window within the Java virtual machine (VM) is disposed of, the VM may terminate. Note, however, that there can be a delay before the program exits automatically, and that under some circumstances the program might keep running. It is quicker and safer to explicitly exit the program using `System.exit(int)`. See 
[AWT Threading Issues](https://docs.oracle.com/javase/8/docs/api/java/awt/doc-files/AWTThreadIssues.html#Autoshutdown) for more information.
Window listeners are also commonly used to stop threads and release resources when a window is iconified, and to start up again when the window is deiconified. This avoids unnecessarily using the processor or other resources. For example, when a window that contains animation is iconified, it should stop its animation thread and free any large buffers. When the window is deiconified, it can start the thread again and recreate the buffers.
<p>The following example demonstrates window events. A non-editable text area reports all window events that are fired by its window. This demo implements all methods in the `WindowListener`, `WindowFocusListener`, and `WindowStateListener` interfaces. You can find the demo's code in 
[`WindowEventDemo.java`](../examples/events/WindowEventDemoProject/src/events/WindowEventDemo.java).</p>
<center><img src="../../figures/uiswing/events/WindowEventDemo.png" width="508" height="484" align="bottom" alt="WindowEventDemo.html" /></center><hr />**Try this:**&#160;<ol>
<li>Click the Launch button to run WindowEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#WindowEventDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the WindowEventDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/WindowEventDemoProject/WindowEventDemo.jnlp)<br /></li>
- When the window appears, several messages are already displayed. One line reports whether your window manager supports MAXIMIZED_BOTH. If the window manager does not support other window states, this condition is also reported. Next, several lines are displayed, reporting that the window's window listener has received window-opened, activated, and gained-focus events. All the messages displayed in the window are also sent to standard output.
- Click another window. The "window lost focus" and "window deactivated" messages will be displayed. If this window is not a frame or a dialog, it will receive the activated or deactivated events.
- Click the WindowEventDemo window. You'll see "window activated" and "window gained focus" messages.
- Iconify the window, using the window controls. Two iconification messages are displayed, one from the window listener and the other from the window state listener. Unless you are looking at standard output, the messages will not display until the window is deiconified. Window-deactivation and window-lost-focus events are also reported.
- De-iconify the window. Two deiconification messages are displayed, one from the window listener and the other from the window state listener. The `windowStateChanged` method in `WindowStateListener` class gives the same information that you get using the `windowIconified` and `windowDeiconified` methods in `WindowListener` class. Window-activation and window-gained-focus events are also reported.
- Maximize the window, if your look and feel provides a way to do so. Note that some look and feels running on some window managers, such as the Java look and feel on dtwm, provide a way to maximize the window, but no events are reported. This is because dtwm mimics maximization by resizing the window, but it is not a true maximization event. Some look and feels provide a way to maximize the window in the vertical or horizontal direction **only**. Experiment with your window controls to see what options are available.
- Close the window, using the window controls. A window closing message is displayed. Once the window has closed, a window closed message is sent to standard output.
</ol>
<hr />
Here is the demo's window event handling code:
<pre><code>
public class WindowEventDemo extends JFrame implements WindowListener,
                                            WindowFocusListener,
                                            WindowStateListener {
    ...
    static WindowEventDemo frame = new WindowEventDemo("WindowEventDemo");
    JTextArea display;
    ...

    private void addComponentsToPane() {
        display = new JTextArea();
        display.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(display);
        scrollPane.setPreferredSize(new Dimension(500, 450));
        getContentPane().add(scrollPane, BorderLayout.CENTER);
        
        addWindowListener(this);
        addWindowFocusListener(this);
        addWindowStateListener(this);
        
        checkWM();
    }
    
    public WindowEventDemo(String name) {
        super(name);
    }

    //Some window managers don't support all window states.
  
    public void checkWM() {
        Toolkit tk = frame.getToolkit();
        if (!(tk.isFrameStateSupported(Frame.ICONIFIED))) {
            displayMessage(
                    "Your window manager doesn't support ICONIFIED.");
        }  else displayMessage(
                "Your window manager supports ICONIFIED.");
        if (!(tk.isFrameStateSupported(Frame.MAXIMIZED_VERT))) {
            displayMessage(
                    "Your window manager doesn't support MAXIMIZED_VERT.");
        }  else displayMessage(
                "Your window manager supports MAXIMIZED_VERT.");
        if (!(tk.isFrameStateSupported(Frame.MAXIMIZED_HORIZ))) {
            displayMessage(
                    "Your window manager doesn't support MAXIMIZED_HORIZ.");
        } else displayMessage(
                "Your window manager supports MAXIMIZED_HORIZ.");
        if (!(tk.isFrameStateSupported(Frame.MAXIMIZED_BOTH))) {
            displayMessage(
                    "Your window manager doesn't support MAXIMIZED_BOTH.");
        } else {
            displayMessage(
                    "Your window manager supports MAXIMIZED_BOTH.");
        }
    }

    public void windowClosing(WindowEvent e) {
        displayMessage("WindowListener method called: windowClosing.");
        //A pause so user can see the message before
        //the window actually closes.
        ActionListener task = new ActionListener() {
            boolean alreadyDisposed = false;
            public void actionPerformed(ActionEvent e) {
                if (frame.isDisplayable()) {
                    alreadyDisposed = true;
                    frame.dispose();
                }
            }
        };
        Timer timer = new Timer(500, task); //fire every half second
        timer.setInitialDelay(2000);        //first delay 2 seconds
        timer.setRepeats(false);
        timer.start();
    }

    public void windowClosed(WindowEvent e) {
        //This will only be seen on standard output.
        displayMessage("WindowListener method called: windowClosed.");
    }

    public void windowOpened(WindowEvent e) {
        displayMessage("WindowListener method called: windowOpened.");
    }

    public void windowIconified(WindowEvent e) {
        displayMessage("WindowListener method called: windowIconified.");
    }

    public void windowDeiconified(WindowEvent e) {
        displayMessage("WindowListener method called: windowDeiconified.");
    }

    public void windowActivated(WindowEvent e) {
        displayMessage("WindowListener method called: windowActivated.");
    }

    public void windowDeactivated(WindowEvent e) {
        displayMessage("WindowListener method called: windowDeactivated.");
    }

    public void windowGainedFocus(WindowEvent e) {
        displayMessage("WindowFocusListener method called: windowGainedFocus.");
    }

    public void windowLostFocus(WindowEvent e) {
        displayMessage("WindowFocusListener method called: windowLostFocus.");
    }

    public void windowStateChanged(WindowEvent e) {
        displayStateMessage(
          "WindowStateListener method called: windowStateChanged.", e);
    }

    void displayMessage(String msg) {
        display.append(msg + newline);
        System.out.println(msg);
    }

    void displayStateMessage(String prefix, WindowEvent e) {
        int state = e.getNewState();
        int oldState = e.getOldState();
        String msg = prefix
                   + newline + space
                   + "New state: "
                   + convertStateToString(state)
                   + newline + space
                   + "Old state: "
                   + convertStateToString(oldState);
        displayMessage(msg);
    }

    String convertStateToString(int state) {
        if (state == Frame.NORMAL) {
            return "NORMAL";
        }
        String strState = " ";
        if ((state &amp; Frame.ICONIFIED) != 0) {
            strState += "ICONIFIED";
        }
        //MAXIMIZED_BOTH is a concatenation of two bits, so
        //we need to test for an exact match.
        if ((state &amp; Frame.MAXIMIZED_BOTH) == Frame.MAXIMIZED_BOTH) {
            strState += "MAXIMIZED_BOTH";
        } else {
            if ((state &amp; Frame.MAXIMIZED_VERT) != 0) {
                strState += "MAXIMIZED_VERT";
            }
            if ((state &amp; Frame.MAXIMIZED_HORIZ) != 0) {
                strState += "MAXIMIZED_HORIZ";
            }
            if (" ".equals(strState)){
                strState = "UNKNOWN";
            }
        }
        return strState.trim();
    }
}
</code></pre>
<h2><a name="api" id="api">The Window Listener API</a></h2>
The window listener API consists of three window listener interfaces and the `WindowEvent` class. Their methods are listed in the following tables:
<ul>
- [The WindowListener Interface](#windowlistener)
- [The WindowFocusListener Interface](#windowfocuslistener)
- [The WindowStateListener Interface](#windowstatelistener)
- [The WindowEvent Class](#windowevent)

<a name="windowlistener" id="windowlistener">The WindowListener Interface</a>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[windowOpened(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowListener.html#windowOpened-java.awt.event.WindowEvent-)</td><td headers="h2">Called just after the listened-to window has been shown for the first time.</td>
<td headers="h1">[windowClosing(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowListener.html#windowClosing-java.awt.event.WindowEvent-)</td><td headers="h2">Called in response to a user request for the listened-to window to be closed. To actually close the window, the listener should invoke the window's `dispose` or `setVisible(false)` method.</td>
<td headers="h1">[windowClosed(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowListener.html#windowClosed-java.awt.event.WindowEvent-)</td><td headers="h2">Called just after the listened-to window has closed.</td>
<td headers="h1">[windowIconified(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowListener.html#windowIconified-java.awt.event.WindowEvent-)<br />[windowDeiconified(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowListener.html#windowDeiconified-java.awt.event.WindowEvent-)</td><td headers="h2">Called just after the listened-to window is iconified or deiconified, respectively.</td>
<td headers="h1">[windowActivated(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowListener.html#windowActivated-java.awt.event.WindowEvent-)<br />[windowDeactivated(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowListener.html#windowDeactivated-java.awt.event.WindowEvent-)</td><td headers="h2">Called just after the listened-to window is activated or deactivated, respectively. These methods are not sent to windows that are not frames or dialogs. For this reason, the `windowGainedFocus` and `windowLostFocus` methods to determine when a window gains or loses the focus are preferred.</td>

<a name="windowfocuslistener" id="windowfocuslistener">The WindowFocusListener Interface</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[windowGainedFocus(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowFocusListener.html#windowGainedFocus-java.awt.event.WindowEvent-)<br />[windowLostFocus(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowFocusListener.html#windowLostFocus-java.awt.event.WindowEvent-)</td><td headers="h102">Called just after the listened-to window gains or loses the focus, respectively.</td>

<a name="windowstatelistener" id="windowstatelistener">The WindowStateListener Interface</a>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[windowStateChanged(WindowEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowStateListener.html#windowStateChanged-java.awt.event.WindowEvent-)</td><td headers="h202">Called just after the listened-to window's state is changed by being iconified, deiconified, maximized, or returned to normal. The state is available through the `WindowEvent` as a bitwise mask. The possible values, defined in `java.awt.Frame`, are:- NORMAL. Indicates that no state bits are set.- ICONIFIED.- MAXIMIZED_HORIZ.- MAXIMIZED_VERT.- MAXIMIZED_BOTH. Concatenates `MAXIMIZED_HORIZ` and `MAXIMIZED_VERT`. A window manager may support `MAXIMIZED_BOTH`, while not supporting `MAXIMIZED_HORIZ` or `MAXIMIZED_VERT`. The [`java.awt.Toolkit`](https://docs.oracle.com/javase/8/docs/api/java/awt/Toolkit.html) method [`isFrameStateSupported(int)`](https://docs.oracle.com/javase/8/docs/api/java/awt/Toolkit.html#isFrameStateSupported-int-) can be used to determine what states are supported by the window manager.</td>

<a name="windowevent" id="windowevent">The WindowEvent Class</a>
<th id="h301" align="left">Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[Window getWindow()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowEvent.html#getWindow--)</td><td headers="h302">Returns the window that fired the event. You can use this instead of the `getSource` method.</td>
<td headers="h301">[Window getOppositeWindow()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowEvent.html#getOppositeWindow--)</td><td headers="h302">Returns the other window involved in this focus or activation change. For a `WINDOW_ACTIVATED` or `WINDOW_GAINED_FOCUS` event, this returns the window that lost activation or the focus. For a `WINDOW_DEACTIVATED` or `WINDOW_LOST_FOCUS` event, this returns the window that gained activation or the focus. For any other type of `WindowEvent` with a Java application in a different VM or context, or with no other window, `null` is returned.</td>
<td headers="h301">[int getOldState()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowEvent.html#getOldState--)<br />[int getNewState()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/WindowEvent.html#getNewState--)</td><td headers="h302">For `WINDOW_STATE_CHANGED` events, these methods return the previous or new state of the window as a bitwise mask.</td>

## <a name="eg" id="eg">Examples that Use Window Listeners</a>
