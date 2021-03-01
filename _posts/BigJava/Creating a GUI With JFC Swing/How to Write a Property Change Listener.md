
# How to Write a Property Change Listener

Property-change events occur whenever the value of a **bound property** changes for a **bean** &#226;&#128;&#148; a component that conforms to the JavaBeans&#8482; specification. You can find out more about beans from the 
[JavaBeans](../../javabeans/) trail of the Java Tutorial. All Swing components are also beans.

A JavaBeans property is accessed through its **get** and **set** methods. For example, `JComponent` has the property **font** which is accessible through the `getFont` and `setFont` methods.

Besides the **get** and **set** methods, a bound property fires a property-change event when its value changes. For more information, see the 
[Bound Properties](../../javabeans/writing/properties.html#bound) page in the JavaBeans trail.

Some scenarios that commonly require property-change listeners include:

<li>You have implemented a formatted text field and need a way to detect when the user has entered a new value. You can register a property-change listener on the formatted text field to listen to changes on the **value** property. See `FormattedTextFieldDemo` in 
[How to Use Formatted Text Fields](../components/formattedtextfield.html#value) for an example of this.</li>
<li>You have implemented a dialog and need to know when a user has clicked one of the dialog's buttons or changed a selection in the dialog. See `DialogDemo` in 
[How to Make Dialogs](../components/dialog.html#stayup) for an example of registering a property-change listener on an option pane to listen to changes to the **value** property. You can also check out `FileChooserDemo2` in 
[How to Use File Choosers](../components/filechooser.html#advancedexample) for an example of how to register a property-change listener to listen to changes to the **directoryChanged** and **selectedFileChanged** properties.</li>
<li>You need to be notified when the component that has the focus changes. You can register a property-change listener on the keyboard focus manager to listen to changes to the **focusOwner** property. See `TrackFocusDemo` and `DragPictureDemo` in 
[How to Use the Focus Subsystem](../misc/focus.html#trackingFocus) for examples of this.</li>

Although these are some of the more common uses for property-change listeners, you can register a property-change listener on the bound property of any component that conforms to the JavaBeans specification.

You can register a property change listener in two ways. The first uses the method `addPropertyChangeListener(PropertyChangeListener)`. When you register a listener this way, you are notified of every change to every bound property for that object. In the `propertyChange` method, you can get the name of the property that has changed using the `PropertyChangeEvent` `getPropertyName` method, as in the following code snippet:

```

KeyboardFocusManager focusManager =
   KeyboardFocusManager.getCurrentKeyboardFocusManager();
focusManager.addPropertyChangeListener(new FocusManagerListener());
...
public FocusManagerListener implements PropertyChangeListener {
    public void propertyChange(PropertyChangeEvent e) {
        String propertyName = e.getPropertyName();
        if ("focusOwner".equals(propertyName) {
            ...
        } else if ("focusedWindow".equals(propertyName) {
            ...
        }
    }
    ...
}

```

The second way to register a property change listener uses the method `addPropertyChangeListener(String, PropertyChangeListener)`. The `String` argument is the name of a property. Using this method means that you only receive notification when a change occurs to that particular property. So, for example, if you registered a property change listener like this:

```

aComponent.addPropertyChangeListener("font",
                                     new FontListener());

```

`FontListener` only receives notification when the value of the component's **font** property changes. It does **not** receive notification when the value changes for **transferHandler**, **opaque**, **border**, or any other property.

The following example shows how to register a property change listener on the **value** property of a formatted text field using the two-argument version of `addPropertyChangeListener`:

```

**//...where initialization occurs:**
double amount;
JFormattedTextField amountField;
...
amountField.addPropertyChangeListener("value",
                                      new FormattedTextFieldListener());
...
class FormattedTextFieldListener implements PropertyChangeListener {
    public void propertyChanged(PropertyChangeEvent e) {
        Object source = e.getSource();
        if (source == amountField) {
            amount = ((Number)amountField.getValue()).doubleValue();
            ...
        }
        **...//re-compute payment and update field...**
    }
}

```

## <a name="api" id="api">The Property Change Listener API</a>

<a name="addpropertychangelistener" id="addpropertychangelistener">Registering a PropertyChangeListener</a>
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[addPropertyChangeListener(PropertyChangeListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addPropertyChangeListener-java.beans.PropertyChangeListener-)</td><td headers="h2">Add a property-change listener to the listener list.</td>
<td headers="h1">[addPropertyChangeListener(String, PropertyChangeListener)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#addPropertyChangeListener-java.lang.String-java.beans.PropertyChangeListener-)</td><td headers="h2">Add a property-change listener for a specific property. The listener is called only when there is a change to the specified property.</td>

<a name="propertychangelistener" id="propertychangelistener">The PropertyChangeListener Interface</a>

**Because `PropertyChangeListener` has only one method, it has no corresponding adapter class.**
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[propertyChange(PropertyChangeEvent)](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeListener.html#propertyChange-java.beans.PropertyChangeEvent-)</td><td headers="h102">Called when the listened-to bean changes a bound property.</td>

<a name="propertychangeevent" id="propertychangeevent">The PropertyChangeEvent Class</a>



<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[Object getNewValue()](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeEvent.html#getNewValue--)<br />[Object getOldValue()](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeEvent.html#getOldValue--)</td><td headers="h202">Return the new, or old, value of the property, respectively.</td>
<td headers="h201">[String getPropertyName()](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeEvent.html#getPropertyName--)</td><td headers="h202">Return the name of the property that was changed.</td>
<td headers="h201">[void setPropagationId()](https://docs.oracle.com/javase/8/docs/api/java/beans/PropertyChangeEvent.html#setPropagationId--)</td><td headers="h202">Get or set the propagation ID value. Reserved for future use.</td>

## <a name="eg" id="eg">Examples that Use Property Change Listeners</a>

The following table lists the examples that use property-change listeners.
<th id="h301" align="left">Example</th><th id="h302" align="left">Where Described</th><th id="h303" align="left">Notes</th>
