
# How to Use Text Areas

The 
[`JTextArea`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html) class provides a component that displays multiple lines of text and optionally allows the user to edit the text. If you need to obtain only one line of input from the user, you should use a [text field](textfield.html). If you want the text area to display its text using multiple fonts or other styles, you should use an [editor pane or text pane](editorpane.html). If the displayed text has a limited length and is never edited by the user, use a [label](label.html).

Many of the Tutorial's examples use uneditable text areas to display program output. Here is a picture of an example called `TextDemo` that enables you to type text using a text field (at the top) and then appends the typed text to a text area (underneath).

Click the Launch button to run TextDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#TextDemo).

You can find the entire code for this program in 
[`TextDemo.java`](../examples/components/TextDemoProject/src/components/TextDemo.java). The following code creates and initializes the text area:

```

textArea = new JTextArea(5, 20);
JScrollPane scrollPane = new JScrollPane(textArea); 
textArea.setEditable(false);

```

The two arguments to the `JTextArea` constructor are hints as to the number of rows and columns, respectively, that the text area should display. The scroll pane that contains the text area pays attention to these hints when determining how big the scroll pane should be.

Without the creation of the scroll pane, the text area would not automatically scroll. The `JScrollPane` constructor shown in the preceding snippet sets up the text area for viewing in a scroll pane, and specifies that the scroll pane's scroll bars should be visible when needed. See [How to Use Scroll Panes](scrollpane.html) if you want further information.

Text areas are editable by default. The code `setEditable(false)` makes the text area uneditable. It is still selectable and the user can copy data from it, but the user cannot change the text area's contents directly.

The following code adds text to the text area. Note that the text system uses the '\n' character internally to represent newlines; for details, see the API documentation for 
[`DefaultEditorKit`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.html).

```

private final static String newline = "\n";
...
textArea.append(text + newline);

```

Unless the user has moved the caret (insertion point) by clicking or dragging in the text area, the text area automatically scrolls so that the appended text is visible. You can force the text area to scroll to the bottom by moving the caret to the end of the text area after the call to `append`:

```

textArea.setCaretPosition(textArea.getDocument().getLength());

```

## <a name="custom" id="custom">Customizing Text Areas</a>

You can customize text areas in several ways. For example, although a given text area can display text in only one font and color, you can set which font and color it uses. This customization option can be performed on any component. You can also determine how the text area wraps lines and the number of characters per tab. Finally, you can use the methods that the `JTextArea` class inherits from the `JTextComponent` class to set properties such as the caret, support for dragging, or color selection.

The following code taken from 
[`TextSamplerDemo.java`](../examples/components/TextSamplerDemoProject/src/components/TextSamplerDemo.java) demonstrates initializing an editable text area. The text area uses the specified italic font, and wraps lines between words.

```

JTextArea textArea = new JTextArea(
    "This is an editable JTextArea. " +
    "A text area is a \"plain\" text component, " +
    "which means that although it can display text " +
    "in any font, all of the text is in the same font."
);
textArea.setFont(new Font("Serif", Font.ITALIC, 16));
textArea.setLineWrap(true);
textArea.setWrapStyleWord(true);

```

By default, a text area does not wrap lines that are too long for the display area. Instead, it uses one line for all the text between newline characters and &#151; if the text area is within a [scroll pane](scrollpane.html) &#151; allows itself to be scrolled horizontally. This example turns line wrapping on with a call to the `setLineWrap` method and then calls the `setWrapStyleWord` method to indicate that the text area should wrap lines at word boundaries rather than at character boundaries.

To provide scrolling capability, the example puts the text area in a scroll pane.

```

JScrollPane areaScrollPane = new JScrollPane(textArea);
areaScrollPane.setVerticalScrollBarPolicy(
                JScrollPane.VERTICAL_SCROLLBAR_ALWAYS);
areaScrollPane.setPreferredSize(new Dimension(250, 250));

```

You might have noticed that the `JTextArea` constructor used in this example does not specify the number of rows or columns. Instead, the code limits the size of the text area by setting the scroll pane's preferred size.

## <a name="textareademo" id="textareademo">Another Example: TextAreaDemo</a>

The `TextAreaDemo` example introduces an editable text area with a special feature &#151; a word completion function. As the user types in words, the program suggests hints to complete the word whenever the program's vocabulary contains a word that starts with what has been typed. Here is a picture of the `TextAreaDemo` application.

Click the Launch button to run TextAreaDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#TextAreaDemo).

You can find the entire code for this program in 
[`TextAreaDemo.java`](../examples/components/TextAreaDemoProject/src/components/TextAreaDemo.java).

This example provides a scrolling capacity for the text area with the default scroll bar policy. By default, the vertical scroll bar only appears when the display area is entirely filled with text and there is no room to append new words. You can provide a scroll pane of this type with the following code:

```

  textArea.setWrapStyleWord(true);
  jScrollPane1 = new JScrollPane(textArea);

```

As mentioned above, the text area is editable. You can play with the text area by typing and pasting text, or by deleting some parts of text or the entire content. Also try using standard key bindings for editing text within the text area.

Now explore how the word completion function is implemented. Type in a word like "Swing" or "special". As soon as you have typed "sw" the program shows a possible completion "ing" highlighted in light-blue. Press Enter to accept the completion or continue typing.

The following code adds a document listener to the text area's document:

```

  textArea.getDocument().addDocumentListener(this);

```

When you started typing a word, the `insertUpdate` method checks whether the program's vocabulary contains the typed prefix. Once a completion for the prefix is found, a call to the `invokeLater` method submits a task for changing the document later. It is important to remember that you cannot modify the document from within the document event notification, otherwise you will get an exception. Examine the following code below.

```

String prefix = content.substring(w + 1).toLowerCase();
int n = Collections.binarySearch(words, prefix);
if (n &lt; 0 &amp;&amp; -n &lt;= words.size()) {
    String match = words.get(-n - 1);
    if (match.startsWith(prefix)) {
        // A completion is found
        String completion = match.substring(pos - w);
        // We cannot modify Document from within notification,
        // so we submit a task that does the change later
        SwingUtilities.invokeLater(
            new CompletionTask(completion, pos + 1));
    }
} else {
    // Nothing found
    mode = Mode.INSERT;
}

```

The code shown in bold illustrates how the selection is created. The caret is first set to the end of the complete word, then moved back to a position after the last character typed. The `moveCaretPosition` method not only moves the caret to a new position but also selects the text between the two positions. The completion task is implemented with the following code:

```

  private class CompletionTask implements Runnable {
        String completion;
        int position;
        
        CompletionTask(String completion, int position) {
            this.completion = completion;
            this.position = position;
        }
        
        public void run() {
            textArea.insert(completion, position);
            <b>textArea.setCaretPosition(position + completion.length());
            textArea.moveCaretPosition(position);</b>
            mode = Mode.COMPLETION;
        }
    }

```

## <a name="api" id="api">The Text Area API</a>

The following tables list the commonly used `JTextArea` constructors and methods. Other methods you are likely to call are defined in `JTextComponent`, and listed in [The Text Component API](textapi.html).

You might also invoke methods on a text area that it inherits from its other ancestors, such as `setPreferredSize`, `setForeground`, `setBackground`, `setFont`, and so on. See [The JComponent Class](jcomponent.html) for tables of commonly used inherited methods.

The API for using text areas includes the following categories:

- [Setting or Obtaining Contents](#contents)
- [Fine Tuning Appearance](#looks)
- [Implementing Functionality](#function)
<th id="h1">Method or Constructor</th><th id="h2">Purpose</th>
<td headers="h1">[JTextArea()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#JTextArea--)<br />[JTextArea(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#JTextArea-java.lang.String-)<br />[JTextArea(String, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#JTextArea-java.lang.String-int-int-)<br />[JTextArea(int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#JTextArea-int-int-)</td><td headers="h2">Creates a text area. When present, the `String` argument contains the initial text. The `int` arguments specify the desired width in columns and height in rows, respectively.</td>
<td headers="h1">[void setText(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setText-java.lang.String-)<br />[String getText()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getText--)<br />**(defined in `JTextComponent`)**</td><td headers="h2">Sets or obtains the text displayed by the text area.</td>
<th id="h101">Method</th><th id="h102">Purpose</th>
<td headers="h101">[void setEditable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setEditable-boolean-)<br />[boolean isEditable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#isEditable--)<br />**(defined in `JTextComponent`)**</td><td headers="h102">Sets or indicates whether the user can edit the text in the text area.</td>
<td headers="h101">[void setColumns(int);](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#setColumns-int-)<br />[int getColumns()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#getColumns--)</td><td headers="h102">Sets or obtains the number of columns displayed by the text area. This is really just a hint for computing the area's preferred width.</td>
<td headers="h101">[void setRows(int);](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#setRows-int-)<br />[int getRows()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#getRows--)</td><td headers="h102">Sets or obtains the number of rows displayed by the text area. This is a hint for computing the area's preferred height.</td>
<td headers="h101">[int setTabSize(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#setTabSize-int-)</td><td headers="h102">Sets the number of characters a tab is equivalent to.</td>
<td headers="h101">[int setLineWrap(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#setLineWrap-boolean-)</td><td headers="h102">Sets whether lines are wrapped if they are too long to fit within the allocated width. By default this property is false and lines are not wrapped.</td>
<td headers="h101">[int setWrapStyleWord(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#setWrapStyleWord-boolean-)</td><td headers="h102">Sets whether lines can be wrapped at white space (word boundaries) or at any character. By default this property is false, and lines can be wrapped (if line wrapping is turned on) at any character.</td>
<th id="h201">Method</th><th id="h202">Purpose</th>
<td headers="h201">[void selectAll()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#selectAll--)<br />**(defined in `JTextComponent`)**</td><td headers="h202">Selects all characters in the text area.</td>
<td headers="h201">[void append(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#append-java.lang.String-)</td><td headers="h202">Adds the specified text to the end of the text area.</td>
<td headers="h201">[void insert(String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#insert-java.lang.String-int-)</td><td headers="h202">Inserts the specified text at the specified position.</td>
<td headers="h201">[void replaceRange(String, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#replaceRange-java.lang.String-int-int-)</td><td headers="h202">Replaces the text between the indicated positions with the specified string.</td>
<td headers="h201">[int getLineCount()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#getLineCount--)<br />[int getLineOfOffset(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#getLineOfOffset-int-)<br />[int getLineStartOffset(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#getLineStartOffset-int-)<br />[int getLineEndOffset(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextArea.html#getLineEndOffset-int-)</td><td headers="h202">Utilities for finding a line number or the position of the beginning or end of the specified line.</td>

## <a name="eg" id="eg">Examples That Use Text Areas</a>

This table lists examples that use text areas and points to where those examples are described.
<th id="h301" align="left">Example</th><th id="h302" align="left">Where Described</th><th id="h303" align="left">Notes</th>
<td headers="h301" valign="top">[TextDemo](../examples/components/index.html#TextDemo)</td><td headers="h302" valign="top">This section</td><td headers="h303" valign="top">An application that appends user-entered text to a text area.</td>
<td headers="h301" valign="top">[TextAreaDemo](../examples/components/index.html#TextAreaDemo)</td><td headers="h302" valign="top">This section</td><td headers="h303" valign="top">An application that has a text area with a word completion function.</td>
<td headers="h301" valign="top">[TextSamplerDemo](../examples/components/index.html#TextSamplerDemo)</td><td headers="h302" valign="top">[Using Text Components](text.html)</td><td headers="h303" valign="top">Uses one of each Swing text components.</td>
<td headers="h301" valign="top">[HtmlDemo](../examples/components/index.html#HtmlDemo)</td><td headers="h302" valign="top">[How to Use HTML in Swing Components](html.html)</td><td headers="h303" valign="top">A text area that enables the user to type HTML code to be displayed in a label.</td>
<td headers="h301" valign="top">[BasicDnD](../examples/dnd/index.html#BasicDnD)</td><td headers="h302" valign="top">[Introduction to DnD](../dnd/intro.html)</td><td headers="h303" valign="top">Demonstrates built-in drag-and-drop functionality of several Swing components, including text areas.</td>
<td headers="h301" valign="top">[FocusConceptsDemo](../examples/misc/index.html#FocusConceptsDemo)</td><td headers="h302" valign="top">[How to Use the Focus Subsystem](../misc/focus.html)</td><td headers="h303" valign="top">Demonstrates how focus works using a few components that include a text area.</td>
