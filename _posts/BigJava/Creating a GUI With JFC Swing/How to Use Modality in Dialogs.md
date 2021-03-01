
# How to Use Modality in Dialogs

Java&#8482; SE 6 has resolved modality issues that arose in earlier versions of the platform. The new modality model enables the developer to scope, or limit, a dialog box's modality blocking.

Before proceding with the new modality model, review the following terms:

<li>Dialog box &#8212; A top-level pop-up window with a title and a border that typically takes some form of input from the user. A dialog box can be modal or modeless. For more information about dialog boxes, see 
[An Overview of Dialogs](../components/dialog.html#overview) in the 
[How to Make Dialogs](../components/dialog.html) page.</li>
- Modal dialog box &#8212; A dialog box that blocks input to some other top-level windows in the application, except for windows created with the dialog box as their owner. The modal dialog box captures the window focus until it is closed, usually in response to a button press.
- Modeless dialog box &#8212; A dialog box that enables you to operate with other windows while this dialog box is shown.

In Java SE 6 the behavior of both modal and modeless dialog boxes has been changed so that they always appear on top of both not only of their parent windows and of all blocked windows as well.

The following modality types are supported in Java SE 6:

- **Modeless type** &#8212; A modeless dialog box does not block any other window while it is visible.
- **Document-modal type** &#8212; A document-modal dialog box blocks all windows from the same document, except windows from its child hierarchy. In this context, a document is a hierarchy of windows that share a common ancestor, called the document root, which is the closest ancestor window without an owner.
- **Application-modal type** &#8212; An application-modal dialog box blocks all windows from the same application, except windows from its child hierarchy. If several applets are launched in a browser environment, the browser is allowed to treat them either as separate applications or as a single application. This behavior is implementation-dependent.
- **Toolkit-modal type** &#8212; A toolkit-modal dialog box blocks all windows that run in the same toolkit, except windows from its child hierarchy. If several applets are launched, all of them run with the same toolkit. Hence, a toolkit-modal dialog box shown from an applet may affect other applets and all windows of the browser instance that embeds the Java runtime environment for this toolkit.

Additionally, you can set up the modality exclusion mode:

- **Exclusion mode** &#8212; Any top-level window can be marked not to be blocked by modal dialogs. This property enables you to set up the **modal exclusion** mode. The `Dialog.ModalExclusionType` enum specifies the possible modal exclusion types.

The ModalityDemo example demonstrates the first three of the four modality types mentioned above.

<li>Click the Launch button to run ModalityDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#ModalityDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the ModalityDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ModalityDemoProject/ModalityDemo.jnlp)<br /></li>
<li>The following dialog boxes will appear:
<ul>
1. Book 1 (parent frame)
1. Book 2 (parent frame)
1. Feedback (parent frame)
1. Classics (excluded frame)
</ul>
</li>
1. Switch to the Book 1 frame and choose the Biography title for the book, then select OK.
1. The Biography title will be displayed in the title of the dialog box. Enter the name, for example - &#8220;John&#8221;, into the text field.
<li>Switch to the Book 1 frame and change the title to Funny Tale, then select OK. Since the dialog box for entering the name is **modeless**, you can easily switch to its **parent** frame.
<hr />**Note :**&#160;The modeless dialog box title has been changed to Funny Tale.
<hr />
</li>
1. Select OK in the modeless dialog box.
1. The Funny tale document-modal dialog box appears.
1. Type some text in the text field. Notice that it is signed by the name you entered in the modeless dialog box.
1. Switch to the modeless dialog box and try to change your name. You will not be able to do so, because the document-modal dialog box blocks all windows in its parent hierarchy.
1. Perform the same sequence of operations (steps 3 - 9) for the Book 2 parent frame.
1. Try switching to different dialog boxes. You will notice that you can switch either to the Classics frame or to the Feedback frame as well as to the dialog box of either the Book 1 frame or the Book 2 frame.
1. Switch to the Feedback parent frame. Select Rate Yourself.
1. The confirmation dialog box will appear. Try switching to different dialog boxes. You are only enabled to switch to the Classics dialog box because the standard confirmation dialog box is an application-modal dialog box and it blocks all windows from the same application. However, you will notice that you can select your favorite classical author in the Classics frame. This frame has been created by using the `APPLICATION_EXCLUDE` modality exclusion type, which prevents all top-level windows from being blocked by any application-modal dialog boxes.

The following code snippet shows how to create dialog boxes of different modality types:

```

//The Book 1 parent frame
f1 = new JFrame("Book 1 (parent frame)");

...

//The modeless dialog box
d2 = new JDialog(f1);

...
        
//The document-modal dialog box
d3 = new JDialog(d2, "", Dialog.ModalityType.DOCUMENT_MODAL);

...

        //The Book2 parent frame
f4 = new JFrame("Book 2 (parent frame)");

...

//The modeless dialog box
d5 = new JDialog(f4);

...

//The document-modality dialog box
d6 = new JDialog(d5, "", Dialog.ModalityType.DOCUMENT_MODAL);
        
...

//The excluded frame
f7 = new JFrame("Classics (excluded frame)");
f7.setModalityExclusionType(Dialog.ModalExclusionType.APPLICATION_EXCLUDED);
        
...

//The Feedback parent frame and Confirm Dialog box
f8 = new JFrame("Feedback (parent frame)");
...

JButton b8 = new JButton("Rate yourself");
b8.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        JOptionPane.showConfirmationDialog(null,
                                           "I really like my book",
                                           "Question (application-modal dialog)", 
                                           JOptionPane.Yes_NO_OPTION,
                                           JOptionPane.QUESTION_MESSAGE); 
    }
});


```

Find the demo's complete code in the 
[`ModalityDemo.java`](../examples/misc/ModalityDemoProject/src/misc/ModalityDemo.java) file.

In Java SE 6 you can create a document-modal dialog box without a parent. Because the `Dialog` class is a subclass of the `Window` class, a `Dialog` instance automatically becomes the root of the document if it has no owner. Thus, if such a dialog box is document-modal, its scope of blocking is empty, and it behaves as if it were a modeless dialog box.

## <a name="api" id="api">The Modality API</a>

The `JDialog` class constructors enable you to create dialog boxes of various modality types.
<th id="h1" align="left">Constructor</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[JDialog(Dialog owner)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-)</td><td headers="h2">Creates a modeless dialog box with the specified `Dialog` owner but without a title.</td>
<td headers="h1">[JDialog(Dialog owner, boolean modal)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-boolean-)</td><td headers="h2">Creates a dialog box with the specified `Dialog` owner and modality.</td>
<td headers="h1">[JDialog(Dialog owner, String title)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-)</td><td headers="h2">Creates a modeless dialog box with the specified `Dialog` owner and title.</td>
<td headers="h1">[JDialog(Dialog owner, String title, boolean modal)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-boolean-)</td><td headers="h2">Creates a dialog box with the specified `Dialog` owner, title, and modality.</td>
<td headers="h1">[JDialog(Dialog owner, String title, boolean modal, GraphicsConfiguration gc)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-boolean-java.awt.GraphicsConfiguration-)</td><td headers="h2">Creates a dialog box with the specified `Dialog` owner, title, modality, and graphics configuration.</td>
<td headers="h1">[JDialog(Frame owner)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Frame-)</td><td headers="h2">Creates a modeless dialog box without a title with the specified `Frame` owner. If the value for the owner is null, a shared, hidden frame will be set as the owner of the dialog box.</td>
<td headers="h1">[JDialog(Window owner, String title, Dialog.ModalityType modalityType)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Window-java.lang.String-java.awt.Dialog.ModalityType-)</td><td headers="h2">Creates a dialog box with the specified `Window` owner, title, and modality.</td>

The following table lists methods inherited from the 
[`java.awt.Dialog`](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.html) class.
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[getModalityType](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.html#getModalityType--)</td><td headers="h102">Returns the modality type for this dialog box.</td>
<td headers="h101">[setModalityType](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.html#setModalityType-java.awt.Dialog.ModalityType-)</td><td headers="h102">Sets the modality type for this dialog box. See [ModalityType](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.ModalityType.html) for possible modality types. If the given modality type is not supported, then the `MODELESS` type is used. To ensure that the modality type has been set, call the `getModalityType()` method after calling this method.</td>

## <a name="eg" id="eg">Examples That Use Modality API</a>

The following table lists the example that uses modality in dialogs.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
