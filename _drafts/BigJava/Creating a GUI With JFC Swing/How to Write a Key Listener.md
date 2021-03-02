
# How to Write a Key Listener

Key events indicate when the user is typing at the keyboard. Specifically, key events are fired by the component with the keyboard focus when the user presses or releases keyboard keys. For detailed information about focus, see 
[How to Use the Focus Subsystem](../misc/focus.html).

To define special reactions to particular keys, use key bindings instead of a key listener. For further information, see 
[How to Use Key Bindings](../misc/keybinding.html).

Notifications are sent about two basic kinds of key events:

- The typing of a Unicode character
- The pressing or releasing of a key on the keyboard

The first kind of event is called a **key-typed** event. The second kind is either a **key-pressed** or **key-released** event.

In general, you react to only key-typed events unless you need to know when the user presses keys that do not correspond to characters. For example, to know when the user types a Unicode character &#226;&#128;&#148; whether by pressing one key such as 'a' or by pressing several keys in sequence &#226;&#128;&#148; you handle key-typed events. On the other hand, to know when the user presses the F1 key, or whether the user pressed the '3' key on the number pad, you handle key-pressed events.

To fire keyboard events, a component **must** have the keyboard focus.

To make a component get the keyboard focus, follow these steps:

1. Make sure the component's `isFocusable` method returns `true`. This state allows the component to receive the focus. For example, you can enable keyboard focus for a `JLabel` component by calling the `setFocusable(true)` method on the label.
1. Make sure the component requests the focus when appropriate. For custom components, implement a mouse listener that calls the `requestFocusInWindow` method when the component is clicked.

The focus subsystem consumes focus traversal keys, such as Tab and Shift Tab. If you need to prevent the focus traversal keys from being consumed, you can call

```

component.setFocusTraversalKeysEnabled(false)

```

on the component that is firing the key events. Your program must then handle focus traversal on its own. Alternatively, you can use the 
[`KeyEventDispatcher`](https://docs.oracle.com/javase/8/docs/api/java/awt/KeyEventDispatcher.html) class to pre-listen to all key events. The 
[focus page](../misc/focus.html) has detailed information on the focus subsystem.

You can obtain detailed information about a particular key-pressed event. For example, you can query a key-pressed event to determine if it was fired from an action key. Examples of action keys include Copy, Paste, Page Up, Undo, and the arrow and function keys. You can also query a key-pressed or key-released event to determine the location of the key that fired the event. Most key events are fired from the standard keyboard, but the events for some keys, such as Shift, have information on whether the user pressed the Shift key on the left or the right side of the keyboard. Likewise, the number '2' can be typed from either the standard keyboard or from the number pad.

For key-typed events you can obtain the key character value as well as any modifiers used.

You should not rely on the key character value returned from `getKeyChar` unless it is involved in a key-typed event.

The following example demonstrates key events. It consists of a text field that you can type into, followed by a text area that displays a message every time the text field fires a key event. A button at the bottom of the window lets you clear both the text field and text area.

<li>Click the Launch button to run KeyEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#KeyEventDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the KeyEventDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/KeyEventDemoProject/KeyEventDemo.jnlp)<br /></li>
<li>Type a lowercase 'a' by pressing and releasing the A key on the keyboard.<br />
The text field fires three events: a key-pressed event, a key-typed event, and a key-released event. Note that the key-typed event doesn't have key code information, and key-pressed and key-released events don't have key character information. None of the events so far are from modifier or action keys and the key location, reported on the key-pressed and key-released events, is most likely standard.</li>
<li>Press the Clear button.<br />
You might want to do this after each of the following steps.</li>
<li>Press and release the Shift key.<br />
The text field fires two events: a key-pressed and a key-released. The text field doesn't fire a key-typed event because Shift, by itself, doesn't correspond to any character.</li>
<li>Type an uppercase 'A' by pressing the Shift and A keys.<br />
You'll see the following events, although perhaps not in this order: key-pressed (Shift), key-pressed (A), key typed ('A'), key-released (A), key-released (Shift). Note that Shift is listed as the modifier key for the key-typed and key-pressed events.</li>
<li>Type an uppercase 'A' by pressing and releasing the Caps Lock key, and then pressing the A key.<br />
You should see the following events: key-pressed (Caps Lock), key-pressed (A), key typed ('A'), key-released (A). Note that Caps Lock is **not** listed as a modifier key.</li>
1. Press the Tab key. No Tab key-pressed or key-released events are received by the key event listener. This is because the focus subsystem consumes focus traversal keys, such as Tab and Shift Tab. Press Tab twice more to return the focus to the text area.
1. Press a function key, such as F3. You'll see that the function key is an action key.
1. Press the left Shift key, followed by the right Shift key. The key-pressed and key-released events indicate which Shift key was typed.
<li>Press the Num Lock key if your keyboard has a number pad.<br />
As for Caps Lock, there is a key-pressed event, but no key-released event.</li>
1. Press the '2' key on the number pad. You see the key-pressed, key-typed, and key-released events for the number '2'.
1. Press the '2' key on the standard keyboard. Again, you see the three event messages. The key-typed events for both number 2 keys are identical. But the key-pressed and key-released events indicate different key codes and different key locations.
1. Press the Num Lock key again. A key-released event is fired.

You can find the example's code in 
[`KeyEventDemo.java`](../examples/events/KeyEventDemoProject/src/events/KeyEventDemo.java). Here is the demo's key event handling code:

```

public class KeyEventDemo ...  implements KeyListener ... {
    **...//where initialization occurs:**
        typingArea = new JTextField(20);
        typingArea.addKeyListener(this);

        //Uncomment this if you wish to turn off focus
        //traversal.  The focus subsystem consumes
        //focus traversal keys, such as Tab and Shift Tab.
        //If you uncomment the following line of code, this
        //disables focus traversal and the Tab events 
        //become available to the key event listener.
        //typingArea.setFocusTraversalKeysEnabled(false);
    ...
    /** Handle the key typed event from the text field. */
    public void keyTyped(KeyEvent e) {
        displayInfo(e, "KEY TYPED: ");
    }

    /** Handle the key-pressed event from the text field. */
    public void keyPressed(KeyEvent e) {
        displayInfo(e, "KEY PRESSED: ");
    }

    /** Handle the key-released event from the text field. */
    public void keyReleased(KeyEvent e) {
        displayInfo(e, "KEY RELEASED: ");
    }
    ...
    private void displayInfo(KeyEvent e, String keyStatus){
        
        //You should only rely on the key char if the event
        //is a key typed event.
        int id = e.getID();
        String keyString;
        if (id == KeyEvent.KEY_TYPED) {
            char c = e.getKeyChar();
            keyString = "key character = '" + c + "'";
        } else {
            int keyCode = e.getKeyCode();
            keyString = "key code = " + keyCode
                    + " ("
                    + KeyEvent.getKeyText(keyCode)
                    + ")";
        }
        
        int modifiersEx = e.getModifiersEx();
        String modString = "extended modifiers = " + modifiersEx;
        String tmpString = KeyEvent.getModifiersExText(modifiersEx);
        if (tmpString.length() &gt; 0) {
            modString += " (" + tmpString + ")";
        } else {
            modString += " (no extended modifiers)";
        }
        
        String actionString = "action key? ";
        if (e.isActionKey()) {
            actionString += "YES";
        } else {
            actionString += "NO";
        }
        
        String locationString = "key location: ";
        int location = e.getKeyLocation();
        if (location == KeyEvent.KEY_LOCATION_STANDARD) {
            locationString += "standard";
        } else if (location == KeyEvent.KEY_LOCATION_LEFT) {
            locationString += "left";
        } else if (location == KeyEvent.KEY_LOCATION_RIGHT) {
            locationString += "right";
        } else if (location == KeyEvent.KEY_LOCATION_NUMPAD) {
            locationString += "numpad";
        } else { // (location == KeyEvent.KEY_LOCATION_UNKNOWN)
            locationString += "unknown";
        }
        
        **...//Display information about the KeyEvent...**
    }
}

```

## <a name="api" id="api">The Key Listener API</a>

<a name="keylistener" id="keylistener">The KeyListener Interface</a>

<em>The corresponding adapter class is 
[`KeyAdapter`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyAdapter.html).</em>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[keyTyped(KeyEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyListener.html#keyTyped-java.awt.event.KeyEvent-)</td><td headers="h2">Called just after the user types a Unicode character into the listened-to component.</td>
<td headers="h1">[keyPressed(KeyEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyListener.html#keyPressed-java.awt.event.keyPressed-)</td><td headers="h2">Called just after the user presses a key while the listened-to component has the focus.</td>
<td headers="h1">[keyReleased(KeyEvent)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyListener.html#keyReleased-java.awt.event.KeyEvent-)</td><td headers="h2">Called just after the user releases a key while the listened-to component has the focus.</td>

<a name="keyevent" id="keyevent">The KeyEvent Class</a>

The `KeyEvent` class inherits many useful methods from the 
[`InputEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html) class, such as `getModifiersEx`, and a couple of useful methods from the 
[`ComponentEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ComponentEvent.html) and 
[`AWTEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/AWTEvent.html) classes. See the [InputEvent Class](mouselistener.html#inputevent) table in the [mouse listener](mouselistener.html) page for a complete list.
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[int getKeyChar()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyEvent.html#getKeyChar--)</td><td headers="h102">Obtains the Unicode character associated with this event. Only rely on this value for key-typed events.</td>
<td headers="h101">[int getKeyCode()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyEvent.html#getKeyCode--)</td><td headers="h102">Obtains the key code associated with this event. The key code identifies the particular key on the keyboard that the user pressed or released. The `KeyEvent` class defines many key code constants for commonly seen keys. For example, `VK_A` specifies the key labeled **A**, and `VK_ESCAPE` specifies the Escape key.</td>
<td headers="h101">[String getKeyText(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyEvent.html#getKeyText-int-)<br />[String getKeyModifiersText(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyEvent.html#getKeyModifiersText-int-)</td><td headers="h102">Return text descriptions of the event's key code and modifier keys, respectively.</td>
<td headers="h101">[int getModifiersEx() ](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#getModifiersEx--)<br />[String getModifiersExText(int modifiers)](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#getModifiersExText-int-)</td><td headers="h102">Return the extended modifiers mask for this event. There are methods inherited from the `InputEvent` class. Extended modifiers represent the state of all modal keys. The `getModifiersExText` method returns a string describing the extended modifier keys and mouse buttons. Since the `getModifiersEx` and `getModifiersExText` methods provide more information about key events, they are preferred over the `getKeyText` or `getKeyModifiersText` methods.</td>
<td headers="h101">[boolean isActionKey()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyEvent.html#isActionKey--)</td><td headers="h102">Returns true if the key firing the event is an action key. Examples of action keys include Cut, Copy, Paste, Page Up, Caps Lock, the arrow and function keys. This information is valid only for key-pressed and key-released events.</td>
<td headers="h101">[int getKeyLocation()](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyEvent.html#getKeyLocation--)</td><td headers="h102">Returns the location of the key that fired this event. This provides a way to distinguish keys that occur more than once on a keyboard, such as the two shift keys, for example. The possible values are `KEY_LOCATION_STANDARD`, `KEY_LOCATION_LEFT`, `KEY_LOCATION_RIGHT`, `KEY_LOCATION_NUMPAD`, or `KEY_LOCATION_UNKNOWN`. This method always returns `KEY_LOCATION_UNKNOWN` for key-typed events.</td>

## <a name="eg" id="eg">Examples that Use Key Listeners</a>

The following table lists the examples that use key listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
