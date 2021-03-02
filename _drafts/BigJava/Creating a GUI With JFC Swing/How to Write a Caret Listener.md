
# How to Write a Caret Listener

Caret events occur when the **caret** &#8212; the cursor indicating the insertion point &#8212; in a text component moves or when the selection in a text component changes. The text component's document can initiate caret events when it inserts or removes text, for example. You can attach a caret listener to an instance of any `JTextComponent` subclass with the `addCaretListener` method.

Here is the caret event handling code from an application called `TextComponentDemo`:

```

...
**//where initialization occurs**
CaretListenerLabel caretListenerLabel =
    new CaretListenerLabel("Caret Status");
...
textPane.addCaretListener(caretListenerLabel);
...
protected class CaretListenerLabel extends JLabel
                                   implements CaretListener
{
    ...
    //Might not be invoked from the event dispatching thread.
    public void caretUpdate(CaretEvent e) {
        displaySelectionInfo(e.getDot(), e.getMark());
    }

    //This method can be invoked from any thread.  It 
    //invokes the setText and modelToView methods, which 
    //must run in the event dispatching thread. We use
    //invokeLater to schedule the code for execution
    //in the event dispatching thread.
    protected void displaySelectionInfo(final int dot,
                                        final int mark) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                if (dot == mark) {  // no selection
                    ...
                }
            });
        }
    }
}

```

You can find a link to the source file for `TextComponentDemo` in the 
[example index for using Swing Components](../examples/components/index.html#TextComponentDemo). For a discussion about the caret listener aspect of the program see 
[Listening for Caret and Selection Changes](../components/generaltext.html#caret) in 
[Text Component Features](../components/generaltext.html).

## <a name="api" id="api">The Caret Listener API</a>

<a name="caretlistener" id="caretlistener">The CaretListener Interface</a>

**Because `CaretListener` has only one method, it has no corresponding adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[caretUpdate(CaretEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/CaretListener.html#caretUpdate-javax.swing.event.CaretEvent-)</td><td headers="h2">Called when the caret in the listened-to component moves or when the selection in the listened-to component changes.</td>

<a name="caretevent" id="caretevent">The CaretEvent Class</a>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[int getDot()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/CaretEvent.html#getDot--)</td><td headers="h102">Returns the current location of the caret. If text is selected, the caret marks one end of the selection.</td>
<td headers="h101">[int getMark()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/CaretEvent.html#getMark--)</td><td headers="h102">Returns the other end of the selection. If nothing is selected, the value returned by this method is equal to the value returned by `getDot`. Note that the dot is not guaranteed to be less than the mark.</td>
<td headers="h101">[Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--)<br />(**in `java.util.EventObject`**)</td><td headers="h102">Returns the object that fired the event.</td>

<a name="eg" id="eg"></a>

## Examples that Use Caret Listeners

The following table lists the examples that use caret listeners.
