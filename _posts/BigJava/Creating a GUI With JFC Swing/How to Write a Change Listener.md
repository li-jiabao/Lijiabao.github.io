
# How to Write a Change Listener

A change listener is similar to a [property change listener](propertychangelistener.html). A change listener is registered on an object &#8212; typically a component, but it could be another object, like a model &#8212; and the listener is notified when the object has changed. The big difference from a property change listener is that a change listener is not notified of **what** has changed, but simply that the source object **has** changed. Therefore, a change listener is most useful when it is only necessary to know when an object has changed in any way.

Several Swing components (including 
[JTabbedPane](../components/tabbedpane.html), JViewPort) rely on change events for basic functionality &#8212; sliders, color choosers and spinners. To learn when the value in a 
[slider](../components/slider.html) changes, you need to register a change listener. Similarly, you need to register a change listener on a 
[color chooser](../components/colorchooser.html) to be informed when the user chooses a new color. You register a change listener on a 
[spinner](../components/spinner.html), to be notified when the spinner's value changes.

Here is an example of change event handling code for a slider:

```

**//...where initialization occurs:**
framesPerSecond.addChangeListener(new SliderListener());
...
class SliderListener implements ChangeListener {
    public void stateChanged(ChangeEvent e) {
        JSlider source = (JSlider)e.getSource();
        if (!source.getValueIsAdjusting()) {
            int fps = (int)source.getValue();
            ...
        }    
    }
}

```

You can find the source file for `SliderDemo` in the 
[example index for Using Swing Components](../examples/components/index.html#SliderDemo).

## <a name="api" id="api">The Change Listener API</a>

<a name="changelistener" id="changelistener">The ChangeListener Interface</a>

**Because `ChangeListener` has only one method, it has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[stateChanged(ChangeEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ChangeListener.html#stateChanged-javax.swing.event.ChangeEvent-)</td><td headers="h2">Called when the listened-to component changes state.</td>

<a name="changeevent" id="changeevent">The ChangeEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Returns the object that fired the event.</td>

<a name="eg" id="eg"></a>

## Examples that Use Change Listeners

The following table lists the examples that use change listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
