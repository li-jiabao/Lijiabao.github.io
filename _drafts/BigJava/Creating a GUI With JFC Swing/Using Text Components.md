
# Using Text Components

This section provides background information you might need when using Swing text components. If you intend to use an unstyled text component &#151; a [text field](textfield.html), [password field](passwordfield.html), [formatted text field](formattedtextfield.html), or [text area](textarea.html) &#151; go to its how-to page and return here only if necessary. If you intend to use a styled text component, see [How to Use Editor Panes and Text Panes](editorpane.html), and read this section as well. If you do not know which component you need, read on.

Swing text components display text and optionally allow the user to edit the text. Programs need text components for tasks ranging from the straightforward (enter a word and press Enter) to the complex (display and edit styled text with embedded images in an Asian language).

Swing provides six text components, along with supporting classes and interfaces that meet even the most complex text requirements. In spite of their different uses and capabilities, all Swing text components inherit from the same superclass, 
[`JTextComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html), which provides a highly-configurable and powerful foundation for text manipulation.

The following figure shows the `JTextComponent` hierarchy.

The following picture shows an application called `TextSamplerDemo` that uses each Swing text component.

<li>Click the Launch button to run TextSamplerDemo using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#TextSamplerDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the TextSamplerDemo Application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TextSamplerDemoProject/TextSamplerDemo.jnlp)<br /></li>
1. Type some text in the text field and press Enter. Do the same in the password field. The label beneath the fields is updated when you press Enter.
1. Try entering valid and invalid dates into the formatted text field. Note that when you press Enter the label beneath the fields is updated only if the date is valid.
1. Select and edit text in the text area and the text pane. Use keyboard bindings, Ctrl-X, Ctrl-C, and Ctrl-V, to cut, copy, and paste text, respectively.
1. Try to edit the text in the editor pane, which has been made uneditable with a call to `setEditable`.
1. Look in the text pane to find an example of an embedded component and an embedded icon.

The `TextSamplerDemo` example uses the text components in very basic ways. The following table tells you more about what you can do with each kind of text component.
<th id="h1">Group</th><th id="h2">Description</th><th id="h3">Swing Classes</th>
<th id="h4">Text Controls</th><td headers="h1">Also known simply as text fields, text controls can display only one line of editable text. Like buttons, they generate action events. Use them to get a small amount of textual information from the user and perform an action after the text entry is complete.</td><td headers="h2">[`JTextField`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html) and its subclasses [`JPasswordField`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPasswordField.html) and [`JFormattedTextField`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFormattedTextField.html)</td>
<th id="h5">Plain Text Areas</th><td headers="h1">`JTextArea` can display multiple lines of editable text. Although a text area can display text in any font, all of the text is in the same font. Use a text area to allow the user to enter unformatted text of any length or to display unformatted help information.</td><td headers="h2">[`JTextArea`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html)</td>
<th id="h6">Styled Text Areas</th><td headers="h1">A styled text component can display editable text using more than one font. Some styled text components allow embedded images and even embedded components. Styled text components are powerful and multi-faceted components suitable for high-end needs, and offer more avenues for customization than the other text components.Because they are so powerful and flexible, styled text components typically require more initial programming to set up and use. One exception is that editor panes can be easily loaded with formatted text from a URL, which makes them useful for displaying uneditable help information.</td><td headers="h2">[`JEditorPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html)<br />and its subclass<br />[`JTextPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html)</td>

This Tutorial provides information about the foundation laid by the `JTextComponent` class and tells you how to accomplish some common text-related tasks.

To learn more about text components in JavaFX, see the
[Using Text and Text Effects in JavaFX](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/text.htm) and
[Using JavaFX UI Controls: Text Field](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/text-field.htm) tutorials.
