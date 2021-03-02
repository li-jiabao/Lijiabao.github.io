
# How to Use Combo Boxes

A 
[`JComboBox`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html), which lets the user choose one of several choices, can have two very different forms. The default form is the uneditable combo box, which features a button and a drop-down list of values. The second form, called the editable combo box, features a text field with a small button abutting it. The user can type a value in the text field or click the button to display a drop-down list. Here's what the two forms of combo boxes look like in the Java look and feel:


|<center><img src="../../figures/uiswing/components/UneditableComboMenuMetal.png" width="177" height="25" align="bottom" alt="An uneditable combo box" /></center>|<center><img src="../../figures/uiswing/components/EditableComboBoxMenuMetal.png" width="211" height="25" align="bottom" alt="An editable combo box" /></center>
<td style="font-weight: bold">Uneditable combo box, before (top)<br />and after the button is clicked</td><td style="font-weight: bold">Editable combo box, before and after<br />the arrow button is clicked</td>

Combo boxes require little screen space, and their editable (text field) form is useful for letting the user quickly choose a value without limiting the user to the displayed values. Other components that can display one-of-many choices are groups of [radio buttons](button.html#radiobutton) and [lists](list.html). Groups of radio buttons are generally the easiest for users to understand, but combo boxes can be more appropriate when space is limited or more than a few choices are available. Lists are not terribly attractive, but they're more appropriate than combo boxes when the number of items is large (say, over 20) or when selecting multiple items might be valid.

Because editable and uneditable combo boxes are so different, this section treats them separately. This section covers these topics:

- [Using an Uneditable Combo Box](#uneditable)
- [Handling Events on a Combo Box](#listeners)
- [Using an Editable Combo Box](#editable)
- [Providing a Custom Renderer](#renderer)
- [The Combo Box API](#api)
- [Examples that Use Combo Boxes](#eg)

<a name="uneditable" id="uneditable"></a>

## <a name="uneditable__1" id="uneditable__1">Using an Uneditable Combo Box</a>

The application shown here uses an uneditable combo box for choosing a pet picture:

<a name="trythis" id="trythis"></a>
<!-- *******  boilerplate stuff ******* -->
<li>Click the Launch button to run the ComboBox Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ComboBoxDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ComboBoxDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ComboBoxDemoProject/ComboBoxDemo.jnlp)<br />
<p><!--  ******* end boilerplate stuff  *******  -->
 <!-- 

<li> [Run ComboBoxDemo](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ComboBoxDemoProject/ComboBoxDemo.jnlp) (
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).    Or, to compile and run the example yourself,
     consult the
     [example index](../examples/components/index.html#ComboBoxDemo).
--></p>
</li>
1. Choose an animal name from the combo box to view its picture.
<li>Compare the operation and GUI of this program to one that uses radio buttons: [run RadioButtonDemo](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/RadioButtonDemoProject/RadioButtonDemo.jnlp) (it requires release 6). You might want to compare the source code as well: 
[`ComboBoxDemo.java`](../examples/components/ComboBoxDemoProject/src/components/ComboBoxDemo.java) vs. 
[`RadioButtonDemo.java`](../examples/components/RadioButtonDemoProject/src/components/RadioButtonDemo.java ).</li>

The following code, taken from 
[`ComboBoxDemo.java`](../examples/components/ComboBoxDemoProject/src/components/ComboBoxDemo.java ), creates an uneditable combo box and sets it up:

```

String[] petStrings = { "Bird", "Cat", "Dog", "Rabbit", "Pig" };

//Create the combo box, select item at index 4.
//Indices start at 0, so 4 specifies the pig.
JComboBox petList = new JComboBox(petStrings);
petList.setSelectedIndex(4);
petList.addActionListener(this);

```

This combo box contains an array of strings, but you could just as easily use icons instead. To put anything else into a combo box or to customize how the items in a combo box look, you need to write a custom renderer. An editable combo box would also need a custom editor. Refer to [Providing a Custom Renderer](#renderer) for information and an example.

The preceding code registers an action listener on the combo box. To see the action listener implementation and learn about other types of listeners supported by combo box, refer to [Handling Events on a Combo Box](#listeners).

No matter which constructor you use, a combo box uses a combo box model to contain and manage the items in its menu. When you initialize a combo box with an array or a vector, the combo box creates a default model object for you. As with other Swing components, you can customize a combo box in part by implementing a custom model &#151; an object that implements the 
[`ComboBoxModel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ComboBoxModel.html) interface. <a name="datsun" id="datsun">&#160;</a>

Be careful when implementing a custom model for a combo box. The `JComboBox` methods that change the items in the combo box's menu, such as `insertItemAt`, work only if the data model implements the 
[`MutableComboBoxModel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/MutableComboBoxModel.html) interface (a subinterface of `ComboBoxModel`). Refer to the [API](#api) tables to see which methods are affected.

Something else to watch out for &#151; even for uneditable combo boxes &#151; is ensuring that your custom model fires 
[list data events](../events/listdatalistener.html) when the combo box's data or state changes. Even immutable combo box models, whose data never changes, must fire a list data event (a `CONTENTS_CHANGED` event) when the selection changes. One way to get the list data event firing code for free is to make your combo box model a subclass of 
[`AbstractListModel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractListModel.html).

## <a name="listeners" id="listeners">Handling Events on a Combo Box</a>

Here's the code from `ComboBoxDemo.java` that registers and implements an action listener on the combo box:

```

public class ComboBoxDemo ... implements ActionListener {
    . . .
        petList.addActionListener(this) {
    . . .
    public void actionPerformed(ActionEvent e) {
        JComboBox cb = (JComboBox)e.getSource();
        String petName = (String)cb.getSelectedItem();
        updateLabel(petName);
    }
    . . .
}

```

This action listener gets the newly selected item from the combo box, uses it to compute the name of an image file, and updates a label to display the image. The combo box fires an action event when the user selects an item from the combo box's menu. See 
[How to Write an Action Listener](../events/actionlistener.html), for general information about implementing action listeners.

Combo boxes also generate item events, which are fired when any of the items' selection state changes. Only one item at a time can be selected in a combo box, so when the user makes a new selection the previously selected item becomes unselected. Thus two item events are fired each time the user selects a different item from the menu. If the user chooses the same item, no item events are fired. Use `addItemListener` to register an item listener on a combo box. 
[How to Write an Item Listener](../events/itemlistener.html) gives general information about implementing item listeners.

Although `JComboBox` inherits methods to register listeners for low-level events &#151; focus, key, and mouse events, for example &#151; we recommend that you don't listen for low-level events on a combo box. Here's why: A combo box is a **compound component** &#151; it is comprised of two or more other components. The combo box itself fires high-level events such as action events. Its subcomponents fire low-level events such as mouse, key, and focus events. The low-level events and the subcomponent that fires them are look-and-feel-dependent. To avoid writing look-and-feel-dependent code, you should listen only for high-level events on a compound component such as a combo box. For information about events, including a discussion about high- and low-level events, refer to 
[Writing Event Listeners](../events/index.html).

## <a name="editable" id="editable">Using an Editable Combo Box</a>

Here's a picture of a demo application that uses an editable combo box to enter a pattern with which to format dates.
<!-- *******  boilerplate stuff ******* -->
<li>Click the Launch button to run the ComboBox2 Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ComboBoxDemo2).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ComboBoxDemo2 example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ComboBoxDemo2Project/ComboBoxDemo2.jnlp)<br />
<p><!--  ******* end boilerplate stuff  *******  -->
 <!-- 
<li> [Run ComboBoxDemo2](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ComboBoxDemo2Project/ComboBoxDemo2.jnlp) (
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).    Or, to compile and run the example yourself,
     consult the
     [example index](../examples/components/index.html#ComboBoxDemo2).
        
        --></p>
</li>
1. Enter a new pattern by choosing one from the combo box's menu. The program reformats the current date and time.
1. Enter a new pattern by typing one in and pressing Enter. Again the program reformats the current date and time.

The following code, taken from 
[`ComboBoxDemo2.java`](../examples/components/ComboBoxDemo2Project/src/components/ComboBoxDemo2.java), creates and sets up the combo box:

```

String[] patternExamples = {
         "dd MMMMM yyyy",
         "dd.MM.yy",
         "MM/dd/yy",
         "yyyy.MM.dd G 'at' hh:mm:ss z",
         "EEE, MMM d, ''yy",
         "h:mm a",
         "H:mm:ss:SSS",
         "K:mm a,z",
         "yyyy.MMMMM.dd GGG hh:mm aaa"
};
. . .
JComboBox patternList = new JComboBox(patternExamples);
**patternList.setEditable(true);**
patternList.addActionListener(this);

```

This code is very similar to the previous example, but warrants a few words of explanation. The bold line of code explicitly turns on editing to allow the user to type values in. This is necessary because, by default, a combo box is not editable. This particular example allows editing on the combo box because its menu does not provide all possible date formatting patterns, just shortcuts to frequently used patterns.

An editable combo box fires an action event when the user chooses an item from the menu and when the user types Enter. Note that the menu remains unchanged when the user enters a value into the combo box. If you want, you can easily write an action listener that adds a new item to the combo box's menu each time the user types in a unique value.

See 
[Internationalization](../../i18n/index.html) to learn more about formatting dates and other types of data.

## <a name="renderer" id="renderer">Providing a Custom Renderer</a>

<a name="renderer__1" id="renderer__1">A combo box uses a **renderer** to display each item in its menu. If the combo box is uneditable, it also uses the renderer to display the currently selected item. An editable combo box, on the other hand, uses an **editor** to display the selected item. A renderer for a combo box must implement the 
</a>[`ListCellRenderer`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ListCellRenderer.html) interface. A combo box's editor must implement 
[`ComboBoxEditor`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ComboBoxEditor.html). This section shows how to provide a custom renderer for an uneditable combo box.

The default renderer knows how to render strings and icons. If you put other objects in a combo box, the default renderer calls the `toString` method to provide a string to display. You can customize the way a combo box renders itself and its items by implementing your own `ListCellRenderer`.

Here's a picture of an application that uses a combo box with a custom renderer:

Click the Launch button to run the CustomComboBox Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#CustomComboBoxDemo).

The full source code for this example is in 
[`CustomComboBoxDemo.java`](../examples/components/CustomComboBoxDemoProject/src/components/CustomComboBoxDemo.java). To get the image files it requires, consult the [example index](../examples/components/index.html#CustomComboBoxDemo).

The following statements from the example create an instance of `ComboBoxRenderer` (a custom class) and set up the instance as the combo box's renderer:

```

JComboBox petList = new JComboBox(intArray);
. . .
ComboBoxRenderer renderer = new ComboBoxRenderer();
renderer.setPreferredSize(new Dimension(200, 130));
petList.setRenderer(renderer);
petList.setMaximumRowCount(3);

```

The last line sets the combo box's maximum row count, which determines the number of items visible when the menu is displayed. If the number of items in the combo box is larger than its maximum row count, the menu has a scroll bar. The icons are pretty big for a menu, so our code limits the number of rows to 3. Here's the implementation of `ComboBoxRenderer`, a renderer that puts an icon and text side-by-side:

```

class ComboBoxRenderer extends JLabel
                       implements ListCellRenderer {
    . . .
    public ComboBoxRenderer() {
        setOpaque(true);
        setHorizontalAlignment(CENTER);
        setVerticalAlignment(CENTER);
    }

    /*
     * This method finds the image and text corresponding
     * to the selected value and returns the label, set up
     * to display the text and image.
     */
    public Component getListCellRendererComponent(
                                       JList list,
                                       Object value,
                                       int index,
                                       boolean isSelected,
                                       boolean cellHasFocus) {
        //Get the selected index. (The index param isn't
        //always valid, so just use the value.)
        int selectedIndex = ((Integer)value).intValue();

        if (isSelected) {
            setBackground(list.getSelectionBackground());
            setForeground(list.getSelectionForeground());
        } else {
            setBackground(list.getBackground());
            setForeground(list.getForeground());
        }

        //Set the icon and text.  If icon was null, say so.
        ImageIcon icon = images[selectedIndex];
        String pet = petStrings[selectedIndex];
        setIcon(icon);
        if (icon != null) {
            setText(pet);
            setFont(list.getFont());
        } else {
            setUhOhText(pet + " (no image available)",
                        list.getFont());
        }

        return this;
    }
    . . .
}

```

As a `ListCellRenderer`, `ComboBoxRenderer` implements a method called `getListCellRendererComponent`, which returns a component whose `paintComponent` method is used to display the combo box and each of its items. The easiest way to display an image and an icon is to use a label. So `ComboBoxRenderer` is a subclass of label and returns itself. The implementation of `getListCellRendererComponent` configures the renderer to display the currently selected icon and its description.

These arguments are passed to `getListCellRendererComponent`:

- `JList list` &#151; a list object used behind the scenes to display the items. The example uses this object's colors to set up foreground and background colors.
- `Object value` &#151; the object to render. An `Integer` in this example.
- `int index` &#151; the index of the object to render.
- `boolean isSelected` &#151; indicates whether the object to render is selected. Used by the example to determine which colors to use.
- `boolean cellHasFocus` &#151; indicates whether the object to render has the focus.

Note that combo boxes and [lists](list.html) use the same type of renderer &#151; `ListCellRenderer`. You can save yourself some time by sharing renderers between combo boxes and lists, if it makes sense for your program.

## <a name="api" id="api">The Combo Box API</a>

The following tables list the commonly used `JComboBox` constructors and methods. Other methods you are most likely to invoke on a `JComboBox` object are those it inherits from its superclasses, such as `setPreferredSize`. See [The JComponent API](jcomponent.html#api) for tables of commonly used inherited methods.

The API for using combo boxes falls into two categories:

- [Setting or Getting the Items in the Combo Box's Menu](#list)
- [Customizing the Combo Box's Operation](#configuring)



<th id="h101">Method</th><th id="h102">Purpose</th>
<td headers="h101">[JComboBox()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#JComboBox--)<br />[JComboBox(ComboBoxModel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#JComboBox-javax.swing.ComboBoxModel-)<br />[JComboBox(Object[])](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#JComboBox-java.lang.Object:A-)<br />[JComboBox(Vector)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#JComboBox-java.util.Vector-)</td><td headers="h102">Create a combo box with the specified items in its menu. A combo box created with the default constructor has no items in the menu initially. Each of the other constructors initializes the menu from its argument: a model object, an array of objects, or a `Vector` of objects.</td>
<td headers="h101">[void addItem(Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#addItem-java.lang.Object-)<br />[void insertItemAt(Object, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#insertItemAt-java.lang.Object-int-)</td><td headers="h102">Add or insert the specified object into the combo box's menu. The insert method places the specified object **at** the specified index, thus inserting it before the object currently at that index. These methods require that the combo box's data model be an instance of `MutableComboBoxModel`.</td>
<td headers="h101">[Object getItemAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#getItemAt-int-)<br />[Object getSelectedItem()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#getSelectedItem--)</td><td headers="h102">Get an item from the combo box's menu.</td>
<td headers="h101">[void removeAllItems()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#removeAllItems--)<br />[void removeItemAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#removeItemAt-int-)<br />[void removeItem(Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#removeItem-java.lang.Object-)</td><td headers="h102">Remove one or more items from the combo box's menu. These methods require that the combo box's data model be an instance of `MutableComboBoxModel`.</td>
<td headers="h101">[int getItemCount()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#getItemCount--)</td><td headers="h102">Get the number of items in the combo box's menu.</td>
<td headers="h101">[void setModel(ComboBoxModel)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#setModel-javax.swing.ComboBoxModel-)<br />[ComboBoxModel getModel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#getModel--)</td><td headers="h102">Set or get the data model that provides the items in the combo box's menu.</td>
<td headers="h101">[void setAction(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#setAction-javax.swing.Action-)<br />[Action getAction()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#getAction--)</td><td headers="h102">Set or get the `Action` associated with the combo box. For further information, see [How to Use Actions](../misc/action.html).</td>
<th id="h201">Method or Constructor</th><th id="h202">Purpose</th>
<td headers="h201">[void addActionListener(ActionListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#addActionListener-java.awt.event.ActionListener-)</td><td headers="h202">Add an action listener to the combo box. The listener's `actionPerformed` method is called when the user selects an item from the combo box's menu or, in an editable combo box, when the user presses Enter.</td>
<td headers="h201">[void addItemListener(ItemListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#addItemListener-java.awt.event.ItemListener-)</td><td headers="h202">Add an item listener to the combo box. The listener's `itemStateChanged` method is called when the selection state of any of the combo box's items change.</td>
<td headers="h201">[void setEditable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#setEditable-boolean-)<br />[boolean isEditable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#isEditable--)</td><td headers="h202">Set or get whether the user can type in the combo box.</td>
<td headers="h201">[void setRenderer(ListCellRenderer)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#setRenderer-javax.swing.ListCellRenderer-)<br />[ListCellRenderer getRenderer()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#getRenderer--)</td><td headers="h202">Set or get the object responsible for painting the selected item in the combo box. The renderer is used only when the combo box is uneditable. If the combo box is editable, the editor is used to paint the selected item instead.</td>
<td headers="h201">[void setEditor(ComboBoxEditor)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#setEditor-javax.swing.ComboBoxEditor-)<br />[ComboBoxEditor getEditor()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComboBox.html#getEditor--)</td><td headers="h202">Set or get the object responsible for painting and editing the selected item in the combo box. The editor is used only when the combo box is editable. If the combo box is uneditable, the renderer is used to paint the selected item instead.</td>

## <a name="eg" id="eg">Examples that Use Combo Boxes</a>

This table shows the examples that use `JComboBox` and where those examples are described.
<th id="h301" align="left">Example</th><th id="h302" align="left">Where Described</th><th id="h303" align="left">Notes</th>
<td headers="h301">[`ComboBoxDemo`](../examples/components/index.html#ComboBoxDemo)</td><td headers="h302">This section</td><td headers="h303">Uses an uneditable combo box.</td>
<td headers="h301">[`ComboBoxDemo2`](../examples/components/index.html#ComboBoxDemo2)</td><td headers="h302">This section</td><td headers="h303">Uses an editable combo box.</td>
<td headers="h301">[`CustomComboBoxDemo`](../examples/components/index.html#CustomComboBoxDemo)</td><td headers="h302">This section</td><td headers="h303">Provides a custom renderer for a combo box.</td>
<td headers="h301">[`TableRenderDemo`](../examples/components/index.html#TableRenderDemo)</td><td headers="h302">How to Use Tables ([Using a Combo Box as an Editor](table.html#combobox))</td><td headers="h303">Shows how to use a combo box as a table cell editor.</td>
