
# CCP in a non-Text Component

If you are implementing cut, copy and paste using one of the Swing components that is *not* one of the text components you have to do some additional setup. First, you need to install the cut, copy, and paste actions in the action map. The following method shows how to do this:

```

    private void setMappings(JList list) { 
        ActionMap map = list.getActionMap();
        map.put(TransferHandler.getCutAction().getValue(Action.NAME),
                TransferHandler.getCutAction());
        map.put(TransferHandler.getCopyAction().getValue(Action.NAME),
                TransferHandler.getCopyAction());
        map.put(TransferHandler.getPasteAction().getValue(Action.NAME),
                TransferHandler.getPasteAction());

```

When you set up the Edit menu, you can also choose to add menu accelerators, so that the user can type Control-C to initiate a copy, for example. In the following code snippet, the bolded text shows how to set the menu accelerator for the cut action:

```

    menuItem = new JMenuItem("Cut");
    menuItem.setActionCommand((String)TransferHandler.getCutAction().
             getValue(Action.NAME));
    menuItem.addActionListener(actionListener);
    <b>menuItem.setAccelerator(
      KeyStroke.getKeyStroke(KeyEvent.VK_X, ActionEvent.CTRL_MASK));</b>
    menuItem.setMnemonic(KeyEvent.VK_T);
    mainMenu.add(menuItem);

```

If you have set the menu accelerators for the CCP actions, this next step is redundant. If you have not set the menu accelerators, you need to add the CCP bindings to the input map. The following code snippet shows how this is done:

```

    // only required if you have not set the menu accelerators
    InputMap imap = this.getInputMap();
    imap.put(KeyStroke.getKeyStroke("ctrl X"),
        TransferHandler.getCutAction().getValue(Action.NAME));
    imap.put(KeyStroke.getKeyStroke("ctrl C"),
        TransferHandler.getCopyAction().getValue(Action.NAME));
    imap.put(KeyStroke.getKeyStroke("ctrl V"),
        TransferHandler.getPasteAction().getValue(Action.NAME));

```

Once the bindings have been installed and the Edit menu has been set up, there is another issue to be addressed: When the user initiates a cut, copy or a paste, which component should receive the action? In the case of a text component, the `DefaultEditorKit` remembers which component last had the focus and forwards the action to that component. The following class, `TransferActionListener`, performs the same function for non-text Swing components. This class can be dropped into most any application:

```

public class TransferActionListener implements ActionListener,
                                              PropertyChangeListener {
    private JComponent focusOwner = null;

    public TransferActionListener() {
        KeyboardFocusManager manager = KeyboardFocusManager.
           getCurrentKeyboardFocusManager();
        manager.addPropertyChangeListener("permanentFocusOwner", this);
    }

    public void propertyChange(PropertyChangeEvent e) {
        Object o = e.getNewValue();
        if (o instanceof JComponent) {
            focusOwner = (JComponent)o;
        } else {
            focusOwner = null;
        }
    }

    public void actionPerformed(ActionEvent e) {
        if (focusOwner == null)
            return;
        String action = (String)e.getActionCommand();
        Action a = focusOwner.getActionMap().get(action);
        if (a != null) {
            a.actionPerformed(new ActionEvent(focusOwner,
                                              ActionEvent.ACTION_PERFORMED,
                                              null));
        }
    }
}

```

Finally, you have to decide how to handle the paste. In the case of a drag and drop, you insert the data at the drop location. In the case of a paste, you do not have the benefit of the user pointing to the desired paste location. You need to decide what makes sense for your application &#8212; inserting the data before or after the current selection might be the best solution.

The following demo, ListCutPaste, shows how to implement CCP in an instance of `JList`. As you can see in the screen shot there are three lists and you can cut, copy, and paste to or from any of these lists. They also support drag and drop. For this demo, the pasted data is inserted after the current selection. If there is no current selection, the data is appended to the end of the list.

<li>Click the Launch button to run ListCutPaste using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/dnd/index.html#ListCutPaste).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ListCutPaste example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ListCutPasteProject/ListCutPaste.jnlp)<br /></li>
1. Select an item in one of the lists. Use the Edit menu or the keyboard equivalent to cut or copy the list item from the source.
1. Select the list item where you want the item to be pasted.
1. Paste the text using the menu or the keyboard equivalent. The item is pasted after the current selection.
1. Perform the same operation using drag and drop.
