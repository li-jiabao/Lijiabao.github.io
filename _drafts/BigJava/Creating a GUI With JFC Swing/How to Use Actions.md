
# How to Use Actions

An 
[`Action`](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html) can be used to separate functionality and state from a component. For example, if you have two or more components that perform the same function, consider using an `Action` object to implement the function. An `Action` object is an 
[action listener](../events/actionlistener.html) that provides not only action-event handling, but also centralized handling of the state of action-event-firing components such as 
[tool bar buttons](../components/toolbar.html), 
[menu items](../components/menu.html), 
[common buttons](../components/button.html), and 
[text fields](../components/textfield.html). The state that an action can handle includes text, icon, mnemonic, enabled, and selected status.

You typically attach an action to a component using the `setAction` method. Here's what happens when `setAction` is invoked on a component:

- The component's state is updated to match the state of the `Action`. For example, if the `Action`'s text and icon values were set, the component's text and icon are set to those values.
- The `Action` object is registered as an action listener on the component.
- If the state of the `Action` changes, the component's state is updated to match the `Action`. For example, if you change the enabled status of the action, all components it's attached to change their enabled states to match the action.

Here's an example of creating a tool-bar button and menu item that perform the same function:

```

Action leftAction = new LeftAction(); **//LeftAction code is shown later**
...
button = new JButton(leftAction)
...
menuItem = new JMenuItem(leftAction);

```

To create an `Action` object, you generally create a subclass of 
[`AbstractAction`](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractAction.html) and then instantiate it. In your subclass, you must implement the `actionPerformed` method to react appropriately when the action event occurs. Here's an example of creating and instantiating an `AbstractAction` subclass:

```

leftAction = new LeftAction("Go left", anIcon,
             "This is the left button.",
             new Integer(KeyEvent.VK_L));
...
class LeftAction extends AbstractAction {
    public LeftAction(String text, ImageIcon icon,
                      String desc, Integer mnemonic) {
        super(text, icon);
        putValue(SHORT_DESCRIPTION, desc);
        putValue(MNEMONIC_KEY, mnemonic);
    }
    public void actionPerformed(ActionEvent e) {
        displayResult("Action for first button/menu item", e);
    }
}

```

When the action created by the preceding code is attached to a button and a menu item, the button and menu item display the text and icon associated with the action. The `L` character is used for mnemonics on the button and menu item, and their tool-tip text is set to the `SHORT_DESCRIPTION` string followed by a representation of the mnemonic key.

For example, we have provided a simple example, 
[`ActionDemo.java`](../examples/misc/ActionDemoProject/src/misc/ActionDemo.java), which defines three actions. Each action is attached to a button and a menu item. Thanks to the mnemonic values set for each button's action, the key sequence `Alt-L` activates the left button, `Alt-M` the middle button, and `Alt-R` the right button. The tool tip for the left button displays **This is the left button. Alt-L.** All of this configuration occurs automatically, without the program making explicit calls to set the mnemonic or tool-tip text. As we'll show later, the program **does** make calls to set the button text, but only to avoid using the values already set by the actions.

<li>
<p>Click the Launch button to run ActionDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Or, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#ActionDemo).</p>
[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ActionDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ActionDemoProject/ActionDemo.jnlp)<br /></li>
<li>
<p>Choose the top item from the left menu (**Menu &gt; Go left**).<br />
The text area displays some text identifying both the event source and the action listener that received the event.</p>
</li>
<li>
<p>Click the leftmost button in the tool bar.<br />
The text area again displays information about the event. Note that although the source of the events is different, both events were detected by the same action listener: the `Action` object attached to the components.</p>
</li>
<li>
<p>Choose the top item from the **Action State** menu.<br />
This disables the "Go left" `Action` object, which in turn disables its associated menu item and button.</p>
</li>

Here is what the user sees when the "Go left" action is disabled:
|<center><img src="../../figures/uiswing/misc/ActionDemo-a.png" width="458" height="207" align="bottom" alt="A snapshot of ActionDemo when " go="" left="" is="" disabled.="" /></center>|<center><img src="../../figures/uiswing/misc/ActionDemo-b.png" width="458" height="207" align="bottom" alt="A snapshot of ActionDemo when " go="" left="" is="" disabled.="" /></center>

Here's the code that disables the "Go left" action:

```

boolean selected = ...//**true if the action should be enabled;**
                      //**false, otherwise**
leftAction.setEnabled(selected);

```

After you create components using an `Action`, you might well need to customize them. For example, you might want to customize the appearance of one of the components by adding or deleting the icon or text. For example, 
[`ActionDemo.java`](../examples/misc/ActionDemoProject/src/misc/ActionDemo.java) has no icons in its menus, and no text in its buttons. Here's the code that accomplishes this:

```

menuItem = new JMenuItem();
menuItem.setAction(leftAction);
menuItem.setIcon(null); //arbitrarily chose not to use icon in menu
...
button = new JButton();
button.setAction(leftAction);
button.setText(""); //an icon-only button

```

We chose to create an icon-only button and a text-only menu item from the same action by setting the icon property to `null` and the text to an empty string. However, if a property of the `Action` changes, the widget may try to reset the icon and text from the `Action` again.

## <a name="api" id="api">The Action API</a>

The following tables list the commonly used `Action` constructors and methods. The API for using `Action` objects falls into three categories:

- [Components that Support set/getAction](#actioncomponents)
- [Creating and Using an AbstractAction](#actionapi)
- [Action Properties](#properties)
<th id="h101" align="left">Class</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[AbstractButton](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setAction-javax.swing.Action-)<br />[JComboBox](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#setAction-javax.swing.Action-)<br />[JTextField](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#setAction-javax.swing.Action-)</td><td headers="h102">These components and their subclasses may have an action directly assigned to them via `setAction`. For further information about components that are often associated with actions, see the sections on [tool bar buttons](../components/toolbar.html), [menu items](../components/menu.html), [common buttons](../components/button.html), and [text fields](../components/textfield.html). For details on which properties each component takes from the `Action`, see the API documentation for the relevant class's [`configurePropertiesFromAction`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuItem.html#configurePropertiesFromAction-javax.swing.Action-) method. Also refer to the [`buttonActions`](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html#buttonActions) table.</td>
<th id="h201" align="left">Constructor or Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[AbstractAction()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractAction.html#AbstractAction--)<br />[AbstractAction(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractAction.html#AbstractAction-java.lang.String-)<br />[AbstractAction(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractAction.html#AbstractAction-java.lang.String-javax.swing.Icon-)</td><td headers="h202">Create an `Action` object. Through arguments, you can specify the text and icon to be used in the components that the action is attached to.</td>
<td headers="h201">[void setEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractAction.html#setEnabled-boolean-)<br />[boolean isEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractAction.html#isEnabled--)</td><td headers="h202">Set or get whether the components the action controls are enabled. Invoking `setEnabled(false)` disables all the components that the action controls. Similarly, invoking `setEnabled(true)` enables the action's components.</td>
<td headers="h201">[void putValue(String, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractAction.html#putValue-java.lang.String-java.lang.Object-)<br />[Object getValue(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractAction.html#getValue-java.lang.String-)</td><td headers="h202">Set or get an object associated with a specified key. Used for setting and getting properties associated with an action.</td>

<a name="properties" id="properties">Action Properties</a>

This table defines the properties that can be set on an action. The second column lists which components automatically use the properties (and what method is specifically called). For example, setting the `ACCELERATOR_KEY` on an action that is then attached to a menu item, means that `JMenuItem.setAccelerator(KeyStroke)` is called automatically. 

<th id="h301" align="left">Property</th><th id="h302" align="left">Auto-Applied to:<br />Class<br />**(Method Called)**</th><th id="h303" align="left">Purpose</th>
<td headers="h301">[ACCELERATOR_KEY](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html#ACCELERATOR_KEY)</td><td headers="h302">`JMenuItem`<br />(**setAccelerator**)</td><td headers="h303">The `KeyStroke` to be used as the accelerator for the action. For a discussion of accelerators versus mnemonics, see [Enabling Keyboard Operation.](../components/menu.html#mnemonic)</td>
<td headers="h301">[ACTION_COMMAND_KEY](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html#ACTION_COMMAND_KEY)</td><td headers="h302">`AbstractButton`, `JCheckBox`, `JRadioButton`<br />(**setActionCommand**)</td><td headers="h303">The command string associated with the `ActionEvent`.</td>
<td headers="h301">[LONG_DESCRIPTION](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html#LONG_DESCRIPTION)</td><td headers="h302">None</td><td headers="h303">The longer description for the action. Can be used for context-sensitive help.</td>
<td headers="h301">[MNEMONIC_KEY](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html#MNEMONIC_KEY)</td><td headers="h302">`AbstractButton`, `JMenuItem`, `JCheckBox`, `JRadioButton`<br />(**setMnemonic**)</td><td headers="h303">The mnemonic for the action. For a discussion of accelerators versus mnemonics, see [Enabling Keyboard Operation.](../components/menu.html#mnemonic)</td>
<td headers="h301">[NAME](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html#NAME)</td><td headers="h302">`AbstractButton`, `JMenuItem`, `JCheckBox`, `JRadioButton`<br />(**setText**)</td><td headers="h303">The name of the action. You can set this property when creating the action using the `AbstractAction(String)` or `AbstractAction(String, Icon)` constructors.</td>
<td headers="h301">[SHORT_DESCRIPTION](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html#SHORT_DESCRIPTION)</td><td headers="h302">`AbstractButton`, `JCheckBox`, `JRadioButton`<br />(**setToolTipText**)</td><td headers="h303">The short description of the action.</td>
<td headers="h301">[SMALL_ICON](https://docs.oracle.com/javase/8/docs/api/javax/swing/Action.html#SMALL_ICON)</td><td headers="h302">`AbstractButton`, `JMenuItem`<br />(**setIcon**)</td><td headers="h303">The icon for the action used in the tool bar or on a button. You can set this property when creating the action using the `AbstractAction(name, icon)` constructor.</td>

## <a name="eg" id="eg">Examples that Use Actions</a>

The following examples use `Action` objects.
<th id="h401" align="left">Example</th><th id="h402" align="left">Where Described</th><th id="h403" align="left">Notes</th>
<td headers="h401">[`ActionDemo`](../examples/misc/index.html#ActionDemo)</td><td headers="h402">This section</td><td headers="h403">Uses actions to bind buttons and menu items to the same function.</td>
<td headers="h401">[`TextComponentDemo`](../examples/components/index.html#TextComponentDemo)</td><td headers="h402">[Text Component Features](../components/generaltext.html)</td><td headers="h403">Uses text actions to create menu items for text editing commands, such as cut, copy, and paste, and to bind key strokes to caret movement. Also implements custom `AbstractAction` subclasses to implement undo and redo. The text action discussion begins in [Concepts: About Editor Kits](../components/generaltext.html#editorkits).</td>
