
# How to Use Editor Panes and Text Panes

Two Swing classes support styled text: 
[`JEditorPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html) and its subclass 
[`JTextPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html). The `JEditorPane` class is the foundation for Swing's styled text components and provides a mechanism through which you can add support for custom text formats. If you want unstyled text, use a [text area](textarea.html) instead.

You can see an editor pane and a text pane in use by running TextSamplerDemo. Here is a picture of the `TextSamplerDemo` example.

Click the Launch button to run TextSamplerDemo using 
[Java&#8482; Web Start ](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#TextSamplerDemo).

The `TextSamplerDemo` example barely begins to demonstrate the capabilities of editor panes and text panes. However, the top right editor pane illustrates a handy, easy-to-use feature: it displays uneditable help information loaded from a URL. The text pane at the lower right demonstrates that you can easily embed images and even components directly into text panes.

If you need a fully-fledged help system, take a look at the 
[javahelp](https://javaee.github.io/javahelp/) project.

The Swing text API is powerful and immense, and we could devote an entire book just to using editor panes and text panes. This section introduces their capabilities, offers hints on which one you might want to use, and points to other sources of information.

- [Using an Editor Pane to Display Text From a URL](#editorpane)
- [Editor Panes vs. Text Panes](#recap)
- [An Example of Using a Text Pane](#textpane)
- [The Editor Pane and Text Pane API](#api)
- [Examples That Use Editor Panes and Text Panes](#eg)

## <a name="editorpane" id="editorpane">Using an Editor Pane to Display Text From a URL</a>

One task that you can accomplish without knowing anything about the Swing text system is displaying text from a URL. Here is the code from 
[`TextSamplerDemo.java`](../examples/components/TextSamplerDemoProject/src/components/TextSamplerDemo.java) that creates an uneditable editor pane that displays text formatted with HTML tags.

```

JEditorPane editorPane = new JEditorPane();
editorPane.setEditable(false);
java.net.URL helpURL = TextSamplerDemo.class.getResource(
                                "TextSamplerDemoHelp.html");
if (helpURL != null) {
    try {
        editorPane.setPage(helpURL);
    } catch (IOException e) {
        System.err.println("Attempted to read a bad URL: " + helpURL);
    }
} else {
    System.err.println("Couldn't find file: TextSamplerDemoHelp.html");
}

//Put the editor pane in a scroll pane.
JScrollPane editorScrollPane = new JScrollPane(editorPane);
editorScrollPane.setVerticalScrollBarPolicy(
                JScrollPane.VERTICAL_SCROLLBAR_ALWAYS);
editorScrollPane.setPreferredSize(new Dimension(250, 145));
editorScrollPane.setMinimumSize(new Dimension(10, 10));

```

The code uses the default constructor to create the editor pane, then calls `setEditable(false)` so the user cannot edit the text. Next, the code creates the `URL` object, and calls the `setPage` method with it.

The `setPage` method opens the resource pointed to by the URL and figures out the format of the text (which is HTML in the example). If the text format is known, the editor pane initializes itself with the text found at the URL. A standard editor pane can understand plain text, HTML, and RTF. Note that the page might be loaded asynchronously, which keeps the GUI responsive but means that you should not count on the data being completely loaded after the call to `setPage` returns.

## <a name="recap" id="recap">Editor Panes vs. Text Panes</a>

In order to use editor panes and text panes, you need to understand the text system, which is described in [Text Component Features](generaltext.html). Several facts about editor panes and text panes are scattered throughout that section. Here we list the facts again and provide a bit more detail. The information here should help you understand the differences between editor panes and text panes, and when to use which.

<li>An editor pane or a text pane can easily be loaded with text from a URL using the `setPage` method. The `JEditorPane` class also provides constructors that let you initialize an editor pane from a URL. The `JTextPane` class has no such constructors. See [Using an Editor Pane to Display Text From a URL](#editorpane) for an example that uses this feature to load an uneditable editor pane with HTML-formatted text.
Be aware that the document and editor kit might change when using the `setPage` method. For example, if an editor pane contains plain text (the default), and you load it with HTML, the document will change to an `HTMLDocument` instance and the editor kit will change to an `HTMLEditorKit` instance. If your program uses the `setPage` method, make sure you adjust your code for possible changes to the pane's document and editor kit instances (re-register document listeners on the new document, and so on).
</li>
- Editor panes, by default, know how to read, write, and edit plain, HTML, and RTF text. Text panes inherit this capability but impose certain limitations. A text pane insists that its document implement the `StyledDocument` interface. `HTMLDocument` and `RTFDocument` are both `StyledDocuments` so HTML and RTF work as expected within a text pane. If you load a text pane with plain text though, the text pane's document is not a `PlainDocument` as you might expect, but a `DefaultStyledDocument`.
- To support a custom text format, implement an editor kit that can read, write, and edit text of that format. Then call the `registerEditorKitForContentType` method to register your kit with the `JEditorPane` class. By registering an editor kit in this way, all editor panes and text panes in your program will be able to read, write, and edit the new format. However, if the new editor kit is not a `StyledEditorKit`, text panes will not support the new format.
- As mentioned previously, a text pane requires its document to implement the `StyledDocument` interface. The Swing text package provides a default implementation of this interface, `DefaultStyledDocument`, which is the document that text panes use by default. A text pane also requires that its editor kit be an instance of a `StyledEditorKit` (or a subclass). Be aware that the `read` and `write` methods for `StyleEditorKit` work with plain text.
- Through their styled document and styled editor kit, text panes provide support for named styles and logical styles. The `JTextPane` class itself contains many methods for working with styles that simply call methods in its document or editor kit.
- Through the API provided in the `JTextPane` class, you can embed images and components in a text pane. You can embed images in an editor pane, too, but only by including the images in an HTML or RTF file.

## <a name="textpane" id="textpane">An Example of Using a Text Pane</a>

Here is the code from the `TextSamplerDemo` example that creates and initializes a text pane.

```

String[] initString =
        { /* ... ** fill array with initial text ** ... */ };

String[] initStyles =
        { /* ... ** fill array with names of styles ** ... */ };

JTextPane textPane = new JTextPane();
StyledDocument doc = textPane.getStyledDocument();
addStylesToDocument(doc);

//Load the text pane with styled text.
try {
    for (int i=0; i &lt; initString.length; i++) {
        doc.insertString(doc.getLength(), initString[i],
                         doc.getStyle(initStyles[i]));
    }
} catch (BadLocationException ble) {
    System.err.println("Couldn't insert initial text into text pane.");
}

```

Briefly, this code hard-codes the initial text into an array and creates and hard-codes several **styles** &#151; objects that represent different paragraph and character formats &#151; into another array. Next, the code loops over the arrays, inserts the text into the text pane, and specifies the style to use for the inserted text.

Although this is an interesting example that concisely demonstrates several features of `JTextPane`, "real-world" programs aren't likely to initialize a text pane this way. Instead, a program would use an editor pane to save a document which would then be used to initialize the text pane.

## <a name="api" id="api">The Editor Pane and Text Pane API</a>

This section lists some of the API related to text and editor panes. Many of the most useful methods for 
[JEditorPane](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html) and its subclass 
[JTextPane](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html) are inherited from the `JTextComponent` class. You can find the API tables for `JTextComponent` in [The Text Component API](textapi.html). Also see [The JComponent Class](jcomponent.html), which describes the API inherited from `JComponent`.
<th id="h1">Method or Constructor</th><th id="h2">Description</th>
<td headers="h1">[JEditorPane(URL)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#JEditorPane-java.net.URL-)<br />[JEditorPane(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#JEditorPane-java.lang.String-)</td><td headers="h2">Creates an editor pane loaded with the text at the specified URL.</td>
<td headers="h1">[setPage(URL)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#setPage-java.net.URL-)<br />[setPage(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#setPage-java.lang.String-)</td><td headers="h2">Loads an editor pane (or text pane) with the text at the specified URL.</td>
<td headers="h1">[URL getPage()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JEditorPane.html#getPage--)</td><td headers="h2">Gets the URL for the editor pane's (or text pane's) current page.</td>
<th id="h101">Method or Constructor</th><th id="h102">Description</th>
<td headers="h101">[JTextPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html#JTextPane--)<br />[JTextPane(StyledDocument)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html#JTextPane-javax.swing.text.StyledDocument-)</td><td headers="h102">Creates a text pane. The optional argument specifies the text pane's model.</td>
<td headers="h101">[StyledDocument getStyledDocument](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html#getStyledDocument--)<br />[setStyledDocument(StyledDocument)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextPane.html#setStyledDocument-javax.swing.text.StyledDocument-)</td><td headers="h102">Gets or sets the text pane's model.</td>

## <a name="eg" id="eg">Examples That Use Text Panes and Editor Panes</a>

To begin using text, you might want to run these programs and examine their code to find something similar to what you want to do.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
<td headers="h201" valign="top">[`TextSamplerDemo`](../examples/components/index.html#TextSamplerDemo)</td><td headers="h202" valign="top">[Using Text Components](text.html)</td><td headers="h203" valign="top">Uses each Swing text component.</td>
<td headers="h201" valign="top">[`TextComponentDemo`](../examples/components/index.html#TextComponentDemo)</td><td headers="h202" valign="top">[Text Component Features](generaltext.html)</td><td headers="h203" valign="top">Provides a customized text pane. Illustrates many text component features, such as undo and redo, document filters, document listeners, caret change listeners, and how to associate editing actions with menus and key strokes.</td>
<td headers="h201" valign="top">[`TreeDemo`](../examples/components/index.html#TreeDemo)</td><td headers="h202" valign="top">[How to Use Trees](tree.html)</td><td headers="h203" valign="top">Uses an editor pane to display help loaded from an HTML file.</td>


Learn to edit HTML text in JavaFX with the
[Using JavaFX UI Controls: HTML Editor](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/editor.htm) tutorial.
