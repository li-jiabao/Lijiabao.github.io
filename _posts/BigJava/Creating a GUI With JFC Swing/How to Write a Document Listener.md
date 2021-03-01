
# How to Write a Document Listener

A Swing text component uses a 
[`Document`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/Document.html) to represent its content. Document events occur when the content of a document changes in any way. You attach a document listener to a text component's document, rather than to the text component itself. See 
[Implementing a Document Filter](../components/generaltext.html#filter)for more information.

The following example demonstrates document events on two plain text components.

<li>Click the Launch button to run DocumentEventDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/events/index.html#Beeper).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the DocumentEventDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/DocumentEventDemoProject/DocumentEventDemo.jnlp)<br /></li>
<li>Type in the text field at the upper left of the window or the text area beneath the text field.<br />
One document event is fired for each character typed.</li>
<li>Delete text with the backspace key.<br />
One document event is fired for each backspace key typed.</li>
<li>Select text and then delete it by typing backspace or by using a keyboard command such as `CTRL-X` (cut).<br />
One document event is fired for the entire deletion.</li>
<li>Copy text from one text component into the other using keyboard commands such as `CTRL-C` (copy) and `CTRL-V` (paste).<br />
One document event is fired for the entire paste operation regardless of the length of the text pasted. If text is selected in the target text component before the paste command is issued, an additional document event is fired because the selected text is deleted first.</li>

You can find the demo's code in 
[`DocumentEventDemo.java`](../examples/events/DocumentEventDemoProject/src/events/DocumentEventDemo.java). Here is the demo's document event handling code:

```

public class DocumentEventDemo ... {
    **...//where initialization occurs:**
    textField = new JTextField(20);
    textField.addActionListener(new MyTextActionListener());
    textField.getDocument().addDocumentListener(new MyDocumentListener());
    textField.getDocument().putProperty("name", "Text Field");

    textArea = new JTextArea();
    textArea.getDocument().addDocumentListener(new MyDocumentListener());
    textArea.getDocument().putProperty("name", "Text Area");
    ...

class MyDocumentListener implements DocumentListener {
    String newline = "\n";
 
    public void insertUpdate(DocumentEvent e) {
        updateLog(e, "inserted into");
    }
    public void removeUpdate(DocumentEvent e) {
        updateLog(e, "removed from");
    }
    public void changedUpdate(DocumentEvent e) {
        //Plain text components do not fire these events
    }

    public void updateLog(DocumentEvent e, String action) {
        Document doc = (Document)e.getDocument();
        int changeLength = e.getLength();
        displayArea.append(
            changeLength + " character" +
            ((changeLength == 1) ? " " : "s ") +
            action + doc.getProperty("name") + "." + newline +
            "  Text length = " + doc.getLength() + newline);
    }
}

```

Document listeners should not modify the contents of the document; The change is already complete by the time the listener is notified of the change. Instead, write a custom document that overrides the `insertString` or `remove` methods, or both. See 
[Listening for Changes on a Document](../components/generaltext.html#doclisteners) for details.

## <a name="api" id="api">The Document Listener API</a>

<a name="documentlistener" id="documentlistener">The DocumentListener Interface</a>

**`DocumentListener` has no adapter class.**
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[changedUpdate(DocumentEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentListener.html#changedUpdate-javax.swing.event.DocumentEvent-)</td><td headers="h2">Called when the style of some of the text in the listened-to document changes. This sort of event is fired only from a `StyledDocument` &#8212; a `PlainDocument` does not fire these events.</td>
<td headers="h1">[insertUpdate(DocumentEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentListener.html#insertUpdate-javax.swing.event.DocumentEvent-)</td><td headers="h2">Called when text is inserted into the listened-to document.</td>
<td headers="h1">[removeUpdate(DocumentEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentListener.html#removeUpdate-javax.swing.event.DocumentEvent-)</td><td headers="h2">Called when text is removed from the listened-to document.</td>

<a name="documentevent" id="documentevent">The DocumentEvent Interface</a>

Each document event method is passed an object that implements the `DocumentEvent` interface. Typically, this is an instance of 
[`DefaultDocumentEvent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/AbstractDocument.DefaultDocumentEvent.html), defined in `AbstractDocument`.
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[Document getDocument()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentEvent.html#getDocument--)</td><td headers="h102">Returns the document that fired the event. Note that the `DocumentEvent` interface does not inherit from `EventObject`. Therefore, it does not inherit the `getSource` method.</td>
<td headers="h101">[int getLength()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentEvent.html#getLength--)</td><td headers="h102">Returns the length of the change.</td>
<td headers="h101">[int getOffset()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentEvent.html#getOffset--)</td><td headers="h102">Returns the location within the document of the first character changed.</td>
<td headers="h101">[ElementChange getChange(Element)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentEvent.html#getChange-javax.swing.text.Element-)</td><td headers="h102">Returns details about what elements in the document have changed and how. [`ElementChange`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentEvent.ElementChange.html) is an interface defined within the `DocumentEvent` interface.</td>
<td headers="h101">[EventType getType()](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentEvent.html#getType--)</td><td headers="h102">Returns the type of change that occurred. [`EventType`](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/DocumentEvent.EventType.html) is a class defined within the `DocumentEvent` interface that enumerates the possible changes that can occur on a document: insert text, remove text, and change text style.</td>

<a name="eg" id="eg"></a>

## Examples that Use Document Listeners

The following table lists the examples that use document listeners.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
