
# How to Use Text Fields

A text field is a basic text control that enables the user to type a small amount of text. When the user indicates that text entry is complete (usually by pressing Enter), the text field fires an 
[action event](../events/actionlistener.html). If you need to obtain more than one line of input from the user, use a [text area](textarea.html).

<a name="varieties" id="varieties">The Swing API provides several classes for components that are either varieties of text fields or that include text fields.</a>
<td width="30%">[`JTextField`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html)</td>|What this section covers: basic text fields.
|`JFormattedTextField`|A `JTextField` subclass that allows you to specify the legal set of characters that the user can enter. See [How to Use Formatted Text Fields](formattedtextfield.html).
|`JPasswordField`|A `JTextField` subclass that does not show the characters that the user types. See [How to Use Password Fields](passwordfield.html).
|`JComboBox`|Can be edited, and provides a menu of strings to choose from. See [How to Use Combo Boxes](combobox.html).
|`JSpinner`|Combines a formatted text field with a couple of small buttons that enables the user to choose the previous or next available value. See [How to Use Spinners](spinner.html).

The following example displays a basic text field and a text area. The text field is editable. The text area is not editable. When the user presses Enter in the text field, the program copies the text field's contents to the text area, and then selects all the text in the text field.

Click the Launch button to run TextDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#TextDemo).

You can find the entire code for this program in 
[`TextDemo.java`](../examples/components/TextDemoProject/src/components/TextDemo.java). The following code creates and sets up the text field:

```

textField = new JTextField(20);

```

The integer argument passed to the `JTextField` constructor, `20` in the example, indicates the number of columns in the field. This number is used along with metrics provided by the field's current font to calculate the field's preferred width. It does not limit the number of characters the user can enter. To do that, you can either use a [formatted text field](formattedtextfield.html) or a document listener, as described in [Text Component Features](generaltext.html).

We encourage you to specify the number of columns for each text field. If you do not specify the number of columns or a preferred size, then the field's preferred size changes whenever the text changes, which can result in unwanted layout updates.

The next line of code registers a `TextDemo` object as an action listener for the text field.

```

textField.addActionListener(this);

```

The `actionPerformed` method handles action events from the text field:

```

private final static String newline = "\n";
...
public void actionPerformed(ActionEvent evt) {
    String text = textField.getText();
    textArea.append(text + newline);
    textField.selectAll();
}

```

Notice the use of `JTextField`'s `getText` method to retrieve the text currently contained by the text field. The text returned by this method does **not** include a newline character for the Enter key that fired the action event.

You have seen how a basic text field can be used. Because the `JTextField` class inherits from the `JTextComponent` class, text fields are very flexible and can be customized almost any way you like. For example, you can add a document listener or a document filter to be notified when the text changes, and in the filter case you can modify the text field accordingly. Information on text components can be found in [Text Component Features](generaltext.html). Before customizing a `JTextField`, however, make sure that one of the other [components based on text fields](#varieties) will not do the job for you.

Often text fields are paired with labels that describe the text fields. See [Examples That Use Text Fields](#eg) for pointers on creating these pairs.

## <a name="textfielddemo" id="textfielddemo">Another Example: TextFieldDemo</a>

The `TextFieldDemo` example introduces a text field and a text area. You can find the entire code for this program in 
[`TextFieldDemo.java`](../examples/components/TextFieldDemoProject/src/components/TextFieldDemo.java).

As you type characters in the text field the program searches for the typed text in the text area. If the entry is found it gets highlighted. If the program fails to find the entry then the text field's background becomes pink. A status bar below the text area displays a message whether text is found or not. The Escape key is used to start a new search or to finish the current one. Here is a picture of the `TextFieldDemo` application.

Click the Launch button ro run TextFieldDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#TextFieldDemo).

To highlight text, this example uses a highlighter and a painter. The code below creates and sets up the highlighter and the painter for the text area.

```

final Highlighter hilit;
final Highlighter.HighlightPainter painter;
...
hilit = new DefaultHighlighter();
painter = new DefaultHighlighter.DefaultHighlightPainter(HILIT_COLOR);
textArea.setHighlighter(hilit);

```

This code adds a document listener to the text field's document.

```

entry.getDocument().addDocumentListener(this);

```

Document listener's `insertUpdate` and `removeUpdate` methods call the `search` method, which not only performs a search in the text area but also handles highlighting. The following code highlights the found text, sets the caret to the end of the found match, sets the default background for the text field, and displays a message in the status bar.

```

hilit.addHighlight(index, end, painter);
textArea.setCaretPosition(end);
entry.setBackground(entryBg);
message("'" + s + "' found. Press ESC to end search");

```

The status bar is a `JLabel` object. The code below shows how the `message` method is implemented.

```

private JLabel status;
...
void message(String msg) {
    status.setText(msg);
}

```

If there is no match in the text area, the following code changes the text field's background to pink and displays a proper information message.

```

entry.setBackground(ERROR_COLOR);
message("'" + s + "' not found. Press ESC to start a new search");

```

The `CancelAction` class is responsible for handling the Escape key as follows.

```

   class CancelAction extends AbstractAction {
       public void actionPerformed(ActionEvent ev) {
               hilit.removeAllHighlights();
               entry.setText("");
               entry.setBackground(entryBg);
           }
   }

```

## <a name="api" id="api">The Text Field API</a>

The following tables list the commonly used `JTextField` constructors and methods. Other methods you are likely to call are defined in the `JTextComponent` class. Refer to [The Text Component API](textapi.html).

You might also invoke methods on a text field inherited from the text field's other ancestors, such as `setPreferredSize`, `setForeground`, `setBackground`, `setFont`, and so on. See [The JComponent Class](jcomponent.html) for tables of commonly used inherited methods.

The API for using text fields falls into these categories:

- [Setting or Obtaining the Field's Contents](#contents)
- [Fine Tuning the Field's Appearance](#looks)
- [Implementing the Field's Functionality](#function)
<th id="h101">Method or Constructor</th><th id="h102">Purpose</th>
<td headers="h101">[JTextField()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#JTextField--)<br />[JTextField(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#JTextField-java.lang.String-)<br />[JTextField(String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#JTextField-java.lang.String-int-)<br />[JTextField(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#JTextField-int-)</td><td headers="h102">Creates a text field. When present, the `int` argument specifies the desired width in columns. The `String` argument contains the field's initial text.</td>
<td headers="h101">[void setText(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setText-java.lang.String-)<br />[String getText()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#getText--)<br />**(defined in `JTextComponent`)**</td><td headers="h102">Sets or obtains the text displayed by the text field.</td>
<th id="h201">Method</th><th id="h202">Purpose</th>
<td headers="h201">[void setEditable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setEditable-boolean-)<br />[boolean isEditable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#isEditable--)<br />**(defined in `JTextComponent`)**</td><td headers="h202">Sets or indicates whether the user can edit the text in the text field.</td>
<td headers="h201">[void setColumns(int);](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#setColumns-int-)<br />[int getColumns()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#getColumns--)</td><td headers="h202">Sets or obtains the number of columns displayed by the text field. This is really just a hint for computing the field's preferred width.</td>
<td headers="h201">[void setHorizontalAlignment(int);](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#setHorizontalAlignment-int-)<br />[int getHorizontalAlignment()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#getHorizontalAlignment--)</td><td headers="h202">Sets or obtains how the text is aligned horizontally within its area. You can use `JTextField.LEADING`, `JTextField.CENTER`, and `JTextField.TRAILING` for arguments.</td>
<th id="h301">Method</th><th id="h302">Purpose</th>
<td headers="h301">[void addActionListener(ActionListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#addActionListener-java.awt.event.ActionListener-)<br />[void removeActionListener(ActionListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTextField.html#removeActionListener-java.awt.event.ActionListener-)</td><td headers="h302">Adds or removes an action listener.</td>
<td headers="h301">[void selectAll()](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#selectAll--)<br />**(defined in `JTextComponent`)**</td><td headers="h302">Selects all characters in the text field.</td>

## <a name="eg" id="eg">Examples That Use Text Fields</a>

This table shows a few of the examples that use text fields and points to where those examples are described. For examples of code that are similar among all varieties of text fields such as dealing with layout, look at the example lists for related components such as [formatted text fields](formattedtextfield.html#eg) and [spinners](spinner.html#eg).
<th id="h401" align="left">Example</th><th id="h402" align="left">Where Described</th><th id="h403" align="left">Notes</th>
<td headers="h401" valign="top">[TextDemo](../examples/components/index.html#TextDemo)</td><td headers="h402" valign="top">This section</td><td headers="h403" valign="top">An application that uses a basic text field with an action listener.</td>
<td headers="h401" valign="top">[TextFieldDemo](../examples/components/index.html#TextFieldDemo)</td><td headers="h402" valign="top">This section</td><td headers="h403" valign="top">An application that uses a text field and a text area. A search is made in the text area to find an entry from the text field.</td>
<td headers="h401" valign="top">[DialogDemo](../examples/components/index.html#DialogDemo)</td><td headers="h402" valign="top">[How to Make Dialogs](dialog.html)</td><td headers="h403" valign="top">`CustomDialog.java` includes a text field whose value is checked. You can bring up the dialog by clicking the More Dialogs tab, selecting the Input-validating dialog option, and then clicking the Show it! button.</td>
<td headers="h401" valign="top">[TextSamplerDemo](../examples/components/index.html#TextSamplerDemo)</td><td headers="h402" valign="top">[Using Text Components](text.html)</td><td headers="h403" valign="top">Lays out label-text field pairs using a `GridBagLayout` and a convenience method:<pre>`addLabelTextRows(JLabel[] **labels**,                 JTextField[] **textFields**,                 GridBagLayout **gridbag**,                 Container **container**)`</pre></td>
<td headers="h401" valign="top">[TextInputDemo](../examples/components/index.html#TextInputDemo)</td><td headers="h402" valign="top">[How to Use Formatted Text Fields](formattedtextfield.html)</td><td headers="h403" valign="top">Lays out label-text field pairs using a `SpringLayout` and a `SpringUtilities` convenience method:<pre>`makeCompactGrid(Container **parent**,                int **rows**, int **cols**,                int **initialX**, int **initialY**,                int **xPad**, int **yPad**)`</pre></td>

If you are programming in JavaFX, see
[Text Field](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/text-field.htm).
