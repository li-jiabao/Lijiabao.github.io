
# Demo - BasicDnD

Now we will look at a simple demo, called `BasicDnD`, that shows you what you get for free. As you see from the screen shot, BasicDnD contains a table, a list, a tree, a color chooser, a text area, and a text field.

All of these components are standard out-of-the-box components **except** for the list. This list has been customized to bring up a dialog showing where the drop would occur, if it accepted drops.

The following areas accept drops:

- Text field
- Text area
- The color chooser accepts drops of type color, but in order to try this, you need to run two copies of the demo (or another demo that contains a color chooser)

By default, none of the objects has default drag and drop enabled. At startup, you can check the "Turn on Drag and Drop" check box to see what drag and drop behavior you get for free.

<li>Click the Launch button to run BasicDnD using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/dnd/index.html#BasicDnD).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the BasicDnD example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/BasicDnDProject/BasicDnD.jnlp)<br /></li>
1. Select an item in the list and, while holding down the mouse button, begin to drag. Nothing happens because the drag has not yet been enabled on the list.
1. Select the "Turn on Drag and Drop" check box.
1. Press the selected item in the list and begin to drag. Drop the text back onto the list. A dialog shows where the text would appear if the list actually accepted drops. (The default behavior for a list would be to show a "does not accept data" cursor.)
1. Drag the selected text over a text area. The insertion point for the text is indicated by a blinking caret. Also, the cursor changes to indicate that the text area will accept the text as a copy.
1. Release the mouse and watch the text appear in the text area.
1. Select some text in one of the text areas.
1. Press the mouse button while the cursor is over the selected text and begin to drag.
1. Note that this time, the cursor for a drag action appears. Successfully dropping this text into another component will cause the text to be removed from the original component.
1. Hold the Control key down and press again on the selected text. Begin dragging and the copy cursor now appears. Move the cursor over the text area and drop. The text appears in the new location but is not removed from the original location. The Control key can be used to change any Move to a Copy.
1. Select a color from the color chooser. The selected color appears in the Preview panel. Press and hold the mouse button over the color in the Preview panel and drag it over the other components. Note that none of these components accepts color.
<li>Try dropping text, color, and even files, onto the list. A dialog will report the attempted action. The actual drop can be implemented with an additional six lines of code that have been commented out in the 
[`BasicDnD.java`](../examples/dnd/BasicDnDProject/src/dnd/BasicDnD.java ) source file.</li>

Next we will look at the `TransferHandler` class, the workhorse of the drag and drop mechanism.
