
# How to Use Key Bindings

The `JComponent` class supports key bindings as a way of responding to individual keys typed by a user. Here are some examples of when key bindings are appropriate:

<li>You're creating a custom component and want to support keyboard access to it.<br />
For example, you might want the component to react when it has the focus and the user presses the Space key.</li>
<li>You want to override the behavior of an existing key binding.<br />
For example, if your application normally reacts to presses of the F2 key in a particular way, you might want it to perform a different action or ignore the key press.</li>
<li>You want to provide a new key binding for an existing action.<br />
For example, you might feel strongly that Control-Shift-Insert should perform a paste operation.</li>

You often don't need to use key bindings directly. They're used behind the scenes by mnemonics (supported by all buttons and by tabbed panes as well as by 
[`JLabel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JLabel.html)) and accelerators (supported by menu items). You can find coverage of mnemonics and accelerators in the section 
[Enabling Keyboard Operation](../components/menu.html#mnemonic).

An alternative to key bindings is using 
[key listeners](../events/keylistener.html). Key listeners have their place as a low-level interface to keyboard input, but for responding to individual keys key bindings are more appropriate and tend to result in more easily maintained code. Key listeners are also difficult if the key binding is to be active when the component doesn't have focus. Some of the advantages of key bindings are they're somewhat self documenting, take the containment hierarchy into account, encourage reusable chunks of code (`Action` objects), and allow actions to be easily removed, customized, or shared. Also, they make it easy to change the key to which an action is bound. Another advantage of Actions is that they have an enabled state which provides an easy way to disable the action without having to track which component it is attached to.

The rest of this section gives you the details you need to use key bindings:

- [How Key Bindings Work](#maps)
- [How to Make and Remove Key Bindings](#howto)
- [The Key Binding API](#api)
- [Examples that Use Key Bindings](#eg)

## <a name="maps" id="maps">How Key Bindings Work</a>

The key binding support provided by `JComponent` relies on the 
[`InputMap`](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputMap.html) and 
[`ActionMap`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ActionMap.html) classes. An input map binds key strokes to action names, and an action map specifies the 
[action](action.html) corresponding to each action name. Technically, you don't need to use action names in the maps; you can use any object as the "key" into the maps. By convention, however, you use a string that names an action.

Each `InputMap`/`ActionMap` has a parent that typically comes from the UI. Any time the look and feel is changed, the parent is reset. In this way, any bindings specified by the developer are never lost on look and feel changes.

Each `JComponent` has one action map and three input maps. The input maps correspond to the following focus situations:

When the user types a key, the `JComponent` key event processing code searches through one or more input maps to find a valid binding for the key. When it finds a binding, it looks up the corresponding action in the action map. If the action is enabled, the binding is valid and the action is executed. If it's disabled, the search for a valid binding continues.

If more than one binding exists for the key, only the first valid one found is used. Input maps are checked in this order:

1. The focused component's `WHEN_FOCUSED` input map.
1. The focused component's `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT` input map.
1. The `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT` input maps of the focused component's parent, and then its parent's parent, and so on, continuing up the containment hierarchy. Note: Input maps for disabled components are skipped.
1. The `WHEN_IN_FOCUSED_WINDOW` input maps of all the enabled components in the focused window are searched. Because the order of searching the components is unpredictable, **avoid duplicate `WHEN_IN_FOCUSED_WINDOW` bindings!**

Let's consider what happens in two typical key binding cases: a button reacting to the Space key, and a frame with a default button reacting to the Enter key.

In the first case, assume the user presses the Space key while a `JButton` has the keyboard focus. First, the button's key listeners are notified of the event. Assuming none of the key listeners **consumes** the event (by invoking the 
[`consume`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/InputEvent.html#consume--) method on the `KeyEvent`) the button's `WHEN_FOCUSED` input map is consulted. A binding is found because `JButton` uses that input map to bind Space to an action name. The action name is looked up in the button's action map, and the `actionPerformed` method of the action is invoked. The `KeyEvent` is consumed, and processing stops.

In the second case, assume the Enter key is pressed while the focus is anywhere inside a frame that has a default button (set using the `JRootPane` 
[`setDefaultButton`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRootPane.html#setDefaultButton-javax.swing.JButton-) method). Whatever the focused component is, its key listeners are first notified. Assuming none of them consumes the key event the focused component's `WHEN_FOCUSED` input map is consulted. If it has no binding for the key or the Action bound to the key is disabled, the focused component's `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT` input map is consulted and then (if no binding is found or the Action bound to the key is disabled) the `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT` input maps of each of the component's ancestors in the containment hierarchy. Eventually, the root pane's `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT` input map is searched. Since that input map has a valid binding for Enter, the action is executed, causing the default button to be clicked.

## <a name="howto" id="howto">How to Make and Remove Key Bindings</a>

Here is an example of specifying that a component should react to the F2 key:

```

component.getInputMap().put(KeyStroke.getKeyStroke("F2"),
                            "doSomething");
component.getActionMap().put("doSomething",
                             anAction);
**//where anAction is a javax.swing.Action**

```

As the preceding code shows, to get a component's action map you use the `getActionMap` method (inherited from `JComponent`). To get an input map, you can use the `getInputMap(int)` method, where the integer is one of the `JComponent.WHEN_*FOCUSED*` constants shown in the preceding list. Or, in the usual case where the constant is `JComponent.WHEN_FOCUSED`, you can just use `getInputMap` with no arguments.

To add an entry to one of the maps, use the `put` method. You specify a key using a `KeyStroke` object, which you can get using the 
[`KeyStroke.getKeyStroke(String)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/KeyStroke.html#getKeyStroke-java.lang.String-) method. You can find examples of creating an `Action` (to put in an action map) in 
[How to Use Actions](../misc/action.html).

Here's a slightly more complex example that specifies that a component should react to the Space key as if the user clicked the mouse.

```

component.getInputMap().put(KeyStroke.getKeyStroke("SPACE"),
                            "pressed");
component.getInputMap().put(KeyStroke.getKeyStroke("released SPACE"),
                            "released");
component.getActionMap().put("pressed",
                             pressedAction);
component.getActionMap().put("released",
                             releasedAction);
**//where pressedAction and releasedAction are javax.swing.Action objects**

```

To make a component ignore a key that it normally responds to, you can use the special action name "none". For example, the following code makes a component ignore the F2 key.

```

component.getInputMap().put(KeyStroke.getKeyStroke("F2"),
                            "none");

```

The preceding code doesn't prevent the relevant `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT` and `WHEN_IN_FOCUSED_WINDOW` input maps from being searched for an F2 key binding. To prevent this search, you must use a valid action instead of "none". For example:

```

Action doNothing = new AbstractAction() {
    public void actionPerformed(ActionEvent e) {
        //do nothing
    }
};
component.getInputMap().put(KeyStroke.getKeyStroke("F2"),
                            "doNothing");
component.getActionMap().put("doNothing",
                             doNothing);

```

## <a name="api" id="api">The Key Binding API</a>

The following tables list the commonly used API for key bindings. Also see the API table 
[Creating and Using an Action](action.html#actionapi), in the section 
[How to Use Actions](action.html).

- [Creating and Using InputMaps](#inputmap)
- [Creating and Using ActionMaps](#actionmap)
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[InputMap getInputMap()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getInputMap--)<br />[InputMap getInputMap(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getInputMap-int-)<br />**(in `JComponent`)**</td><td headers="h2">Get one of the input maps for the component. The arguments can be one of these `JComponent` constants: `WHEN_FOCUSED`, `WHEN_IN_FOCUSED_WINDOW`, or `WHEN_ANCESTOR_OF_FOCUSED_COMPONENT`. The no-argument method gets the `WHEN_FOCUSED` input map.</td>
<td headers="h1">[void put(KeyStroke, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputMap.html#put-javax.swing.KeyStroke-java.lang.Object-)<br />**(in `InputMap`)**</td><td headers="h2">Set the action name associated with the specified key stroke. If the second argument is `null`, this method removes the binding for the key stroke. To make the key stroke be ignored, use `"none"` as the second argument.</td>
<td headers="h1">[static KeyStroke getKeyStroke(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/KeyStroke.html#getKeyStroke-java.lang.String-)<br />**(in `KeyStroke`)**</td><td headers="h2">Get the object specifying a particular user keyboard activity. Typical arguments are "alt shift X", "INSERT", and "typed a". See the [`KeyStroke` API documentation](https://docs.oracle.com/javase/8/docs/api/javax/swing/KeyStroke.html) for full details and for other forms of the `getKeyStroke` method.</td>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[ActionMap getActionMap()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getActionMap--)<br />**(in `JComponent`)**</td><td headers="h102">Get the object that maps names into actions for the component.</td>
<td headers="h101">[void put(Object, Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/ActionMap.html#put-java.lang.Object-javax.swing.Action-)<br />**(in `ActionMap`)**</td><td headers="h102">Set the action associated with the specified name. If the second argument is `null`, this method removes the binding for the name.</td>

## <a name="eg" id="eg">Examples that Use Key Bindings</a>

The following table lists examples that use key bindings:
