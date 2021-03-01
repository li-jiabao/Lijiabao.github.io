
# CCP in a Text Component

If you are implementing cut, copy and paste using one of the Swing text components (text field, password field, formatted text field, or text area) your work is very straightforward. These text components utilize the 
[`DefaultEditorKit`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/DefaultEditorKit.html) which provides built-in actions for cut, copy and paste. The default editor kit also handles the work of remembering which component last had the focus. This means that if the user initiates one of these actions using the menu or a keyboard equivalent, the correct component receives the action &#8212; no additional code is required.

The following demo, `TextCutPaste`, contains three text fields. As you can see in the screen shot, you can cut, copy, and paste to or from any of these text fields. They also support drag and drop.

<li>Click the Launch button to run `TextCutPaste` using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/dnd/index.html#TextCutPaste).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the TextCutPaste example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TextCutPasteProject/TextCutPaste.jnlp)<br /></li>
1. Select text in one of the text fields. Use the Edit menu or the keyboard equivalent to cut or copy the text from the source.
1. Position the caret where you want the text to be pasted.
1. Paste the text using the menu or the keyboard equivalent.
1. Perform the same operation using drag and drop.

Here is the code that creates the Edit menu by hooking up the built-in cut, copy, and paste actions defined in `DefaultEditorKit` to the menu items. This works with any component that descends from `JComponent`:

```

    /**
     * Create an Edit menu to support cut/copy/paste.
     */
    public JMenuBar createMenuBar () {
        JMenuItem menuItem = null;
        JMenuBar menuBar = new JMenuBar();
        JMenu mainMenu = new JMenu("Edit");
        mainMenu.setMnemonic(KeyEvent.VK_E);

        menuItem = new JMenuItem(new DefaultEditorKit.CutAction());
        menuItem.setText("Cut");
        menuItem.setMnemonic(KeyEvent.VK_T);
        mainMenu.add(menuItem);

        menuItem = new JMenuItem(new DefaultEditorKit.CopyAction());
        menuItem.setText("Copy");
        menuItem.setMnemonic(KeyEvent.VK_C);
        mainMenu.add(menuItem);

        menuItem = new JMenuItem(new DefaultEditorKit.PasteAction());
        menuItem.setText("Paste");
        menuItem.setMnemonic(KeyEvent.VK_P);
        mainMenu.add(menuItem);

        menuBar.add(mainMenu);
        return menuBar;
    }

```

Next we will look at how to accomplish the same functionality using a component that does not have the built-in support of the `DefaultEditorKit`.
