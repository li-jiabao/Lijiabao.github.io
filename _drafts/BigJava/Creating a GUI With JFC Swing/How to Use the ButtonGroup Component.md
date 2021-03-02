
# How to Use the ButtonGroup Component

The `ButtonGroup` component manages the selected/unselected state for a set of buttons. For the group, the `ButtonGroup` instance guarantees that only one button can be selected at a time.

Initially, all buttons managed by a `ButtonGroup` instance are unselected.

## How to Use ButtonGroup Features

You can use `ButtonGroup` with any set of objects that inherit from `AbstractButton`. Typically a button group contains instances of `JRadioButton, JRadioButtonMenuItem`, or `JToggleButton`. It would not make sense to put an instance of `JButton` or `JMenuItem` in a button group because `JButton` and `JMenuItem` do not implement the select/deselect button state.

In general, you will typically follow these steps to write code that uses a `ButtonGroup` component.

1. Subclass `JFrame`
1. Call `ContextPane` together with a layout manager
1. Declare and configure a set of radio buttons or toggle buttons
1. Instantiate a `ButtonGroup` object
1. Call the `add` method on that buttongroup object in order to add each button to the group.

For details and a code example, see 
[How to Use Radio Buttons](button.html#radiobutton). It shows how to use a `ButtonGroup` component to group a set of RadioButtons set into a JPanel.

## The ButtonGroup API

&#160;
<th id="h1">Constructor or Method</th><th id="h2">Purpose</th>
<td headers="h1">[ButtonGroup()](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#ButtonGroup--)</td><td headers="h2">Create a `ButtonGroup` instance.</td>
<td headers="h1">[void add(AbstractButton)](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#add-javax.swing.AbstractButton-)<br />[void remove(AbstractButton)](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#remove-javax.swing.AbstractButton-)</td><td headers="h2">Add a button to the group, or remove a button from the group. </td>
<td headers="h1">[public ButtonGroup getGroup()](https://docs.oracle.com/javase/8/docs/api/javax/swing/DefaultButtonModel.html#getGroup--)<br />**(in `DefaultButtonModel`)**</td><td headers="h2">Get the `ButtonGroup`, if any, that controls a button. For example:<br />`ButtonGroup group = ((DefaultButtonModel)button.getModel()).getGroup();`</td>
<td headers="h1">[public ButtonGroup clearSelection()](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#ButtonGroup--)</td><td headers="h2">Clears the state of selected buttons in the ButtonGroup. None of the buttons in the ButtonGroup are selected .</td>

## ButtonGroup Examples

The following demonstration application uses the ButtonGroup component to group radio buttons displaying on a Window.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
<td headers="h101">[`RadioButtonDemo`](../examples/components/index.html#RadioButtonDemo)</td><td headers="h102">[How to Use Radio Buttons](button.html#radiobutton)</td><td headers="h103">Uses radio buttons to determine which of five images it should display.</td>
