
# How to Make Dialogs

A Dialog window is an independent subwindow meant to carry temporary notice apart from the main Swing Application Window. Most Dialogs present an error message or warning to a user, but Dialogs can present images, directory trees, or just about anything compatible with the main Swing Application that manages them.

For convenience, several Swing component classes can directly instantiate and display **dialogs**. To create simple, standard dialogs, you use the 
[`JOptionPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html) class. The [`ProgressMonitor`](progress.html) class can put up a dialog that shows the progress of an operation. Two other classes, [`JColorChooser`](colorchooser.html) and [`JFileChooser`](filechooser.html), also supply standard dialogs. To bring up a print dialog, you can use the 
[Printing](../../2d/printing/index.html) API. To create a custom dialog, use the 
[`JDialog`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html) class directly.

The code for simple dialogs can be minimal. For example, here is an informational dialog:

Here is the code that creates and shows it:

```

JOptionPane.showMessageDialog(frame, "Eggs are not supposed to be green.");

```

The rest of this section covers the following topics:

- [An Overview of Dialogs](#overview)
- [The DialogDemo Example](#dialogdemo)
- [JOptionPane Features](#features)
- [Creating and Showing Simple Dialogs](#create)
- [Customizing Button Text](#button)
- [Getting the User's Input from a Dialog](#input)
- [Stopping Automatic Dialog Closing](#stayup)
- [The Dialog API](#api)
- [Examples that Use Dialogs](#eg)

## <a name="overview" id="overview">An Overview of Dialogs</a>

Every dialog is dependent on a Frame component. When that Frame is destroyed, so are its dependent Dialogs. When the frame is iconified, its dependent Dialogs also disappear from the screen. When the frame is deiconified, its dependent Dialogs return to the screen. A swing JDialog class inherits this behavior from the AWT `Dialog` class.

A Dialog can be **modal**. When a modal Dialog is visible, it blocks user input to all other windows in the program. JOptionPane creates `JDialog`s that are modal. To create a non-modal Dialog, you must use the `JDialog` class directly.

Starting with JDK 7, you can modify dialog window modality behavior using the new Modality API. See [The New Modality API](http://www.oracle.com/technetwork/articles/javase/modality-137604.html) for details.

The `JDialog` class is a subclass of the AWT 
[`java.awt.Dialog`](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.html) class. It adds a [root pane](rootpane.html) container and support for a default close operation to the `Dialog` object . These are the same features that `JFrame` has, and using `JDialog` directly is very similar to using `JFrame`. If you're going to use `JDialog` directly, then you should understand the material in [Using Top-Level Containers](toplevel.html) and [How to Make Frames](frame.html), especially [Responding to Window-Closing Events](frame.html#windowevents).

Even when you use `JOptionPane` to implement a dialog, you're still using a `JDialog` behind the scenes. The reason is that `JOptionPane` is simply a container that can automatically create a `JDialog` and add itself to the `JDialog`'s content pane.

## <a name="dialogdemo" id="dialogdemo">The DialogDemo Example</a>

Here is a picture of an application that displays dialogs.

<li>Click the Launch button to run the Dialog Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#DialogDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the DialogDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/DialogDemoProject/DialogDemo.jnlp)<br />
<p><!--  ******* end boilerplate stuff  *******  -->
 <!--

<hr />**Try this:**&nbsp;<ol>
<li> [Run DialogDemo](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/DialogDemoProject/DialogDemo.jnlp) (
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).    Or, to compile and run the example yourself,
     consult the
     [example index](../examples/components/index.html#DialogDemo).
--></p>
</li>
<li>Click the Show it! button.<br />
A modal dialog will appear. Until you close it, the application will be unresponsive, although it will repaint itself if necessary. You can close the dialog either by clicking a button in the dialog or explicitly, such as by using the dialog window decorations.</li>
<li>In the More Dialogs pane, click the bottom radio button and then the Show it! button.<br />
A non-modal dialog will appear. Note that the DialogDemo window remains fully functional while the non-modal dialog is up.</li>
<li>While the non-modal dialog is showing, iconify the DialogDemo window.<br />
The dialog will disappear from the screen until you deiconify the DialogDemo window.</li>

## <a name="features" id="features">JOptionPane Features</a>

Using `JOptionPane`, you can quickly create and customize several different kinds of dialogs. `JOptionPane` provides support for laying out standard dialogs, providing icons, specifying the dialog title and text, and customizing the button text. Other features allow you to customize the components the dialog displays and specify where the dialog should appear onscreen. You can even specify that an option pane put itself into an [internal frame](internalframe.html) (`JInternalFrame`) instead of a `JDialog`.

When you create a `JOptionPane`, look-and-feel-specific code adds components to the `JOptionPane` and determines the layout of those components.

`JOptionPane`'s icon support lets you easily specify which icon the dialog displays. You can use a custom icon, no icon at all, or any one of four standard `JOptionPane` icons (question, information, warning, and error). Each look and feel has its own versions of the four standard icons. The following figure shows the icons used in the Java (and Windows) look and feel.
<th id="h1">Icon description</th><th id="h2">Java look and feel</th><th id="h3">Windows look and feel</th>
<td headers="h1">question</td><td headers="h2" align="center" width="75"><img src="../../figures/uiswing/components/metal-question.png" width="32" height="32" alt="The Java look and feel icon for dialogs that ask questions" /></td><td headers="h3" align="center" width="75"><img src="../../figures/uiswing/components/windows-question.png" width="32" height="32" alt="The Windows look and feel icon for dialogs that ask questions" /></td>
<td headers="h1">information</td><td headers="h2" align="center" width="75"><img src="../../figures/uiswing/components/metal-info.png" width="32" height="32" alt="The Java look and feel icon for informational dialogs" /></td><td headers="h3" align="center" width="75"><img src="../../figures/uiswing/components/windows-info.png" width="32" height="32" alt="The Windows look and feel icon for informational dialogs" /></td>
<td headers="h1">warning</td><td headers="h2" align="center" width="75"><img src="../../figures/uiswing/components/metal-warning.png" width="32" height="32" alt="The Java look and feel icon for warning dialogs" /></td><td headers="h3" align="center" width="75"><img src="../../figures/uiswing/components/windows-warning.png" width="32" height="32" alt="The Windows look and feel icon for warning dialogs" /></td>
<td headers="h1">error</td><td headers="h2" align="center" width="75"><img src="../../figures/uiswing/components/metal-error.png" width="32" height="32" alt="The Java look and feel icon for error dialogs" /></td><td headers="h3" align="center" width="75"><img src="../../figures/uiswing/components/windows-error.png" width="32" height="32" alt="The Windows look and feel icon for error dialogs" /></td>

## <a name="create" id="create">Creating and Showing Simple Dialogs</a>

For most simple modal dialogs, you create and show the dialog using one of `JOptionPane`'s `show**Xxx**Dialog` methods. If your dialog should be an [internal frame](internalframe.html), then add `Internal` after `show` &#151; for example, `showMessageDialog` changes to `showInternalMessageDialog`. If you need to control the dialog window-closing behavior or if you do not want the dialog to be modal, then you should directly instantiate `JOptionPane` and add it to a `JDialog` instance. Then invoke `setVisible(true)` on the `JDialog` to make it appear.

The two most useful `show**Xxx**Dialog` methods are `showMessageDialog` and `showOptionDialog`. The `showMessageDialog` method displays a simple, one-button dialog. The `showOptionDialog` method displays a customized dialog &#151; it can display a variety of buttons with customized button text, and can contain a standard text message or a collection of components.

The other two `show**Xxx**Dialog` methods are used less often. The `showConfirmDialog` method asks the user to confirm something, but presents standard button text (Yes/No or the localized equivalent, for example) rather than button text customized to the user situation (Start/Cancel, for example). A fourth method, `showInputDialog`, is designed to display a modal dialog that gets a string from the user, using either a text field, an uneditable combo box or a list.

Here are some examples, taken from 
[`DialogDemo.java`](../examples/components/DialogDemoProject/src/components/DialogDemo.java), of using `showMessageDialog`, `showOptionDialog`, and the `JOptionPane` constructor. For more example code, see 
[`DialogDemo.java`](../examples/components/DialogDemoProject/src/components/DialogDemo.java) and the other programs listed in [Examples that Use Dialogs](#eg).
<td valign="top"><img src="../../figures/uiswing/components/InformationalDialogMetal.png" width="268" height="122" alt="Informational dialog with default title and icon" /></td><td valign="top"><pre>`//default title and iconJOptionPane.showMessageDialog(frame,    "Eggs are not supposed to be green.");`</pre></td>
<td valign="top"><img src="../../figures/uiswing/components/DialogIcon2Metal.png" width="268" height="122" alt="Informational dialog with custom title, warning icon" /></td><td valign="top"><pre>`//custom title, warning iconJOptionPane.showMessageDialog(frame,    "Eggs are not supposed to be green.",    "Inane warning",    JOptionPane.WARNING_MESSAGE);`</pre></td>
<td valign="top"><img src="../../figures/uiswing/components/DialogIcon3Metal.png" width="268" height="122" alt="Informational dialog with custom title, error icon" /></td><td valign="top"><pre>`//custom title, error iconJOptionPane.showMessageDialog(frame,    "Eggs are not supposed to be green.",    "Inane error",    JOptionPane.ERROR_MESSAGE);`</pre></td>
<td valign="top"><img src="../../figures/uiswing/components/DialogIcon4Metal.png" width="268" height="122" alt="Informational dialog with custom title, no icon" /></td><td valign="top"><pre>`//custom title, no iconJOptionPane.showMessageDialog(frame,    "Eggs are not supposed to be green.",    "A plain message",    JOptionPane.PLAIN_MESSAGE);`</pre></td>
<td valign="top"><img src="../../figures/uiswing/components/DialogIcon5Metal.png" width="268" height="122" alt="Informational dialog with custom title, custom icon" /></td><td valign="top"><pre>`//custom title, custom iconJOptionPane.showMessageDialog(frame,    "Eggs are not supposed to be green.",    "Inane custom dialog",    JOptionPane.INFORMATION_MESSAGE,    icon);`</pre></td>
<td valign="top"><img src="../../figures/uiswing/components/OptionDialogMetal.png" width="374" height="122" alt="Yes/No/Cancel (in different words); showOptionDialog" /></td>
<td valign="top"><pre>`//Custom button textObject[] options = {"Yes, please",                    "No, thanks",                    "No eggs, no ham!"};int n = JOptionPane.showOptionDialog(frame,    "Would you like some green eggs to go "    + "with that ham?",    "A Silly Question",    JOptionPane.YES_NO_CANCEL_OPTION,    JOptionPane.QUESTION_MESSAGE,    null,    options,    options[2]);`</pre></td>
<td valign="top"><img src="../../figures/uiswing/components/JOptionPaneMetal.png" width="280" height="145" alt="Explicitly used the JOptionPane constructor" /></td>
<td valign="top"><pre>`final JOptionPane optionPane = new JOptionPane(    "The only way to close this dialog is by\n"    + "pressing one of the following buttons.\n"    + "Do you understand?",    JOptionPane.QUESTION_MESSAGE,    JOptionPane.YES_NO_OPTION);`</pre></td>

The arguments to all of the `show**Xxx**Dialog` methods and `JOptionPane` constructors are standardized, though the number of arguments for each method and constructor varies. The following list describes each argument. To see the exact list of arguments for a particular method, see [The Dialog API](#api).

The `JOptionPane` constructors do not include this argument. Instead, you specify the parent frame when you create the `JDialog` that contains the `JOptionPane`, and you use the `JDialog` `setLocationRelativeTo` method to set the dialog position.

```

"Complete the sentence:\n \"Green eggs and...\""

```

You can either let the option pane display its default icon or specify the icon using the message type or icon argument. By default, an option pane created with `showMessageDialog` displays the information icon, one created with `showConfirmDialog` or `showInputDialog` displays the question icon, and one created with a `JOptionPane` constructor displays no icon. To specify that the dialog display a standard icon or no icon, specify the message type corresponding to the icon you desire. To specify a custom icon, use the icon argument. The icon argument takes precedence over the message type; as long as the icon argument has a non-null value, the dialog displays the specified icon.

## <a name="button" id="button">Customizing Button Text</a>

When you use `JOptionPane` to create a dialog, you can either use the standard button text (which might vary by look and feel and locale) or specify different text. By default, the option pane type determines how many buttons appear. For example, `YES_NO_OPTION` dialogs have two buttons, and `YES_NO_CANCEL_OPTION` dialogs have three buttons.

The following code, taken from 
[`DialogDemo.java`](../examples/components/DialogDemoProject/src/components/DialogDemo.java), creates two Yes/No dialogs. The first dialog is implemented with `showConfirmDialog`, which uses the look-and-feel wording for the two buttons. The second dialog uses `showOptionDialog` so it can customize the wording. With the exception of wording changes, the dialogs are identical.
<td valign="top"><img src="../../figures/uiswing/components/CustomizingButtonTextMetal.png" width="277" height="122" alt="A yes/no dialog, in those words [but perhaps translated]" /></td><td valign="top"><pre>`//default icon, custom titleint n = JOptionPane.showConfirmDialog(    frame,    "Would you like green eggs and ham?",    "An Inane Question",    JOptionPane.YES_NO_OPTION);`</pre></td>
<td valign="top"><img src="../../figures/uiswing/components/CustomizingButtonText2Metal.png" width="277" height="122" alt="A yes/no dialog -- in other words" /></td><td valign="top"><pre>`Object[] options = {"Yes, please",                    "No way!"};int n = JOptionPane.showOptionDialog(frame,    "Would you like green eggs and ham?",    "A Silly Question",    JOptionPane.YES_NO_OPTION,    JOptionPane.QUESTION_MESSAGE,    null,     //do not use a custom Icon    options,  //the titles of buttons    options[0]); //default button title`</pre></td>

As the previous code snippets showed, the `showMessageDialog`, `showConfirmDialog`, and `showOptionDialog` methods return an integer indicating the user's choice. The values for this integer are `YES_OPTION`, `NO_OPTION`, `CANCEL_OPTION`, `OK_OPTION`, and `CLOSED_OPTION`. Except for `CLOSED_OPTION`, each option corresponds to the button the user pressed. When `CLOSED_OPTION` is returned, it indicates that the user closed the dialog window explicitly, rather than by choosing a button inside the option pane.

Even if you change the strings that the standard dialog buttons display, the return value is still one of the pre-defined integers. For example, a `YES_NO_OPTION` dialog always returns one of the following values: `YES_OPTION`, `NO_OPTION`, or `CLOSED_OPTION`.

## <a name="input" id="input">Getting the User's Input from a Dialog</a>

The only form of `show**Xxx**Dialog` that does not return an integer is `showInputDialog`, which returns an `Object` instead. This `Object` is generally a `String` reflecting the user's choice. Here is an example of using `showInputDialog` to create a dialog that lets the user choose one of three strings:

```

Object[] possibilities = {"ham", "spam", "yam"};
String s = (String)JOptionPane.showInputDialog(
                    frame,
                    "Complete the sentence:\n"
                    + "\"Green eggs and...\"",
                    "Customized Dialog",
                    JOptionPane.PLAIN_MESSAGE,
                    icon,
                    possibilities,
                    "ham");

//If a string was returned, say so.
if ((s != null) &amp;&amp; (s.length() &gt; 0)) {
    setLabel("Green eggs and... " + s + "!");
    return;
}

//If you're here, the return value was null/empty.
setLabel("Come on, finish the sentence!");

```

If you do not care to limit the user's choices, you can either use a form of the `showInputDialog` method that takes fewer arguments or specify `null` for the array of objects. In the Java look and feel, substituting `null` for `possibilities` results in a dialog that has a text field and looks like this:

Because the user can type anything into the text field, you might want to check the returned value and ask the user to try again if it is invalid. Another approach is to create a custom dialog that validates the user-entered data before it returns. See 
[`CustomDialog.java`](../examples/components/DialogDemoProject/src/components/CustomDialog.java) for an example of validating data.

If you're designing a custom dialog, you need to design your dialog's API so that you can query the dialog about what the user chose. For example, `CustomDialog` has a `getValidatedText` method that returns the text the user entered.

## <a name="stayup" id="stayup">Stopping Automatic Dialog Closing</a>

By default, when the user clicks a `JOptionPane`-created button, the dialog closes. But what if you want to check the user's answer before closing the dialog? In this case, you must implement your own property change listener so that when the user clicks a button, the dialog does not automatically close.

`DialogDemo` contains two dialogs that implement a property change listener. One of these dialogs is a custom modal dialog, implemented in 
[`CustomDialog`](../examples/components/DialogDemoProject/src/components/CustomDialog.java), that uses `JOptionPane` both to get the standard icon and to get layout assistance. The other dialog, whose code is below, uses a standard Yes/No `JOptionPane`. Though this dialog is rather useless as written, its code is simple enough that you can use it as a template for more complex dialogs.

Besides setting the property change listener, the following code also calls the `JDialog`'s `setDefaultCloseOperation` method and implements a window listener that handles the window close attempt properly. If you do not care to be notified when the user closes the window explicitly, then ignore the bold code.

```

final JOptionPane optionPane = new JOptionPane(
                "The only way to close this dialog is by\n"
                + "pressing one of the following buttons.\n"
                + "Do you understand?",
                JOptionPane.QUESTION_MESSAGE,
                JOptionPane.YES_NO_OPTION);

final JDialog dialog = new JDialog(frame, 
                             "Click a button",
                             true);
dialog.setContentPane(optionPane);
<b>dialog.setDefaultCloseOperation(
    JDialog.DO_NOTHING_ON_CLOSE);
dialog.addWindowListener(new WindowAdapter() {
    public void windowClosing(WindowEvent we) {
        setLabel("Thwarted user attempt to close window.");
    }
});</b>
optionPane.addPropertyChangeListener(
    new PropertyChangeListener() {
        public void propertyChange(PropertyChangeEvent e) {
            String prop = e.getPropertyName();

            if (dialog.isVisible() 
             &amp;&amp; (e.getSource() == optionPane)
             &amp;&amp; (prop.equals(JOptionPane.VALUE_PROPERTY))) {
                //If you were going to check something
                //before closing the window, you'd do
                //it here.
                dialog.setVisible(false);
            }
        }
    });
dialog.pack();
dialog.setVisible(true);

int value = ((Integer)optionPane.getValue()).intValue();
if (value == JOptionPane.YES_OPTION) {
    setLabel("Good.");
} else if (value == JOptionPane.NO_OPTION) {
    setLabel("Try using the window decorations "
             + "to close the non-auto-closing dialog. "
             + "You can't!");
}

```

## <a name="api" id="api">The Dialog API</a>

The following tables list the commonly used `JOptionPane` and `JDialog` constructors and methods. Other methods you're likely to call are defined by the 
[`Dialog`](https://docs.oracle.com/javase/8/docs/api/java/awt/Dialog.html), 
[`Window`](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html) and 
[`Component`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html) classes and include `pack`, `setSize`, and `setVisible`.

The API is listed as follows:

- [Showing Standard Modal Dialogs (using `JOptionPane` Class Methods)](#showapi)
- [Methods for Using `JOptionPane`s Directly](#joptionpaneapi)
- [Frequently Used `JDialog` Constructors and Methods](#jdialogapi)
<th id="h501" scope="col" align="left">Method</th><th id="h502" scope="col" align="left">Purpose</th>
<td headers="h501" scope="row">[static void showMessageDialog(Component, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showMessageDialog-java.awt.Component-java.lang.Object-)<br />[static void showMessageDialog(Component, Object, String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showMessageDialog-java.awt.Component-java.lang.Object-java.lang.String-int-)<br />[static void showMessageDialog(Component, Object, String, int, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showMessageDialog-java.awt.Component-java.lang.Object-java.lang.String-int-javax.swing.Icon-)</td><td headers="h502">Show a one-button, modal dialog that gives the user some information. The arguments specify (in order) the parent component, message, title, message type, and icon for the dialog. See [Creating and Showing Simple Dialogs](#create) for a discussion of the arguments and their effects.</td>
<td headers="h501" scope="row">[static int showOptionDialog(Component, Object, String, int, int, Icon, Object[], Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showOptionDialog-java.awt.Component-java.lang.Object-java.lang.String-int-int-javax.swing.Icon-java.lang.Object:A-java.lang.Object-)</td><td headers="h502">Show a customized modal dialog. The arguments specify (in order) the parent component, message, title, option type, message type, icon, options, and initial value for the dialog. See [Creating and Showing Simple Dialogs](#create) for a discussion of the arguments and their effects.</td>
<td headers="h501" scope="row">[static int showConfirmDialog(Component, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showConfirmDialog-java.awt.Component-java.lang.Object-)<br />[static int showConfirmDialog(Component, Object, String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showConfirmDialog-java.awt.Component-java.lang.Object-java.lang.String-int-)<br />[static int showConfirmDialog(Component, Object, String, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showConfirmDialog-java.awt.Component-java.lang.Object-java.lang.String-int-int-)<br />[static int showConfirmDialog(Component, Object, String, int, int, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showConfirmDialog-java.awt.Component-java.lang.Object-java.lang.String-int-int-javax.swing.Icon-)</td><td headers="h502">Show a modal dialog that asks the user a question. The arguments specify (in order) the parent component, message, title, option type, message type, and icon for the dialog. See [Creating and Showing Simple Dialogs](#create) for a discussion of the arguments and their effects.</td>
<td headers="h501" scope="row">[static String showInputDialog(Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInputDialog-java.lang.Object-)<br />[static String showInputDialog(Component, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInputDialog-java.awt.Component-java.lang.Object-)<br />[static String showInputDialog(Component, Object, String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInputDialog-java.awt.Component-java.lang.Object-java.lang.String-int-)<br />[static String showInputDialog(Component, Object, String, int, Icon, Object[], Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInputDialog-java.awt.Component-java.lang.Object-java.lang.String-int-javax.swing.Icon-java.lang.Object:A-java.lang.Object-)</td><td headers="h502">Show a modal dialog that prompts the user for input. The single-argument version specifies just the message, with the parent component assumed to be null. The arguments for the other versions specify (in order) the parent component, message, title, message type, icon, options, and initial value for the dialog. See [Creating and Showing Simple Dialogs](#create) for a discussion of the arguments and their effects.</td>
<td headers="h501" scope="row">[static void showInternalMessageDialog(...)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInternalMessageDialog-java.awt.Component-java.lang.Object-)<br />[static void showInternalOptionDialog(...)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInternalOptionDialog-java.awt.Component-java.lang.Object-java.lang.String-int-int-javax.swing.Icon-java.lang.Object:A-java.lang.Object-)<br />[static void showInternalConfirmDialog(...)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInternalConfirmDialog-java.awt.Component-java.lang.Object-)<br />[static String showInternalInputDialog(...)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInternalInputDialog-java.awt.Component-java.lang.Object-)</td><td headers="h502">Implement a standard dialog as an internal frame. See the [`JOptionPane` API documentation](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html) for the exact list of arguments.</td>
<th id="h601" scope="col" align="left">Method or Constructor</th><th id="h602" scope="col" align="left">Purpose</th>
<td headers="h601" scope="row">[JOptionPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#JOptionPane--)<br />[JOptionPane(Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#JOptionPane-java.lang.Object-)<br />[JOptionPane(Object, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#JOptionPane-java.lang.Object-int-)<br />[JOptionPane(Object, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#JOptionPane-java.lang.Object-int-int-)<br />[JOptionPane(Object, int, int, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#JOptionPane-java.lang.Object-int-int-javax.swing.Icon-)<br />[JOptionPane(Object, int, int, Icon, Object[])](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#JOptionPane-java.lang.Object-int-int-javax.swing.Icon-java.lang.Object:A-)<br />[JOptionPane(Object, int, int, Icon, Object[], Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#JOptionPane-java.lang.Object-int-int-javax.swing.Icon-java.lang.Object:A-java.lang.Object-)</td><td headers="h602">Creates a `JOptionPane` instance. See [Creating and Showing Simple Dialogs](#create) for a discussion of the arguments and their effects.</td>
<td headers="h601" scope="row">[static Frame getFrameForComponent(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#getFrameForComponent-java.awt.Component-)<br />[static JDesktopPane getDesktopPaneForComponent(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#getDesktopPaneForComponent-java.awt.Component-)</td><td headers="h602">Handy `JOptionPane` class methods that find the [frame](frame.html) or [desktop pane](internalframe.html), respectively, that the specified component is in.</td>
<td headers="h601" scope="row">[int getMaxCharactersPerLineCount()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#getMaxCharactersPerLineCount--)</td><td headers="h602">Determines where line breaks will be automatically inserted in the option pane text. (The default is `Integer.MAX_VALUE`.) To use this method, you must create a `JOptionPane` subclass. For example, the following code results in an option pane with one word per line, due to the fact that each word in the string is 5 characters or less:<pre>`JOptionPane op = new JOptionPane("This is the text.") {    public int getMaxCharactersPerLineCount() {        return 5;    }};`</pre></td>



<th id="h701" scope="col" align="left">Method or Constructor</th><th id="h702" scope="col" align="left">Purpose</th>
<td headers="h701" scope="row">[JDialog()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog--)<br />[JDialog(Dialog)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-)<br />[JDialog(Dialog, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-boolean-)<br />[JDialog(Dialog, String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-)<br />[JDialog(Dialog, String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-boolean-)<br />[JDialog(Dialog, String, boolean, GraphicsConfiguration)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Dialog-java.lang.String-boolean-java.awt.GraphicsConfiguration-)<br />[JDialog(Frame)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Frame-)<br />[JDialog(Frame, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Frame-boolean-)<br />[JDialog(Frame, String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Frame-java.lang.String-)<br />[JDialog(Frame, String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Frame-java.lang.String-boolean-)<br />[JDialog(Frame, String, boolean, GraphicsConfiguration)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Frame-java.lang.String-boolean-java.awt.GraphicsConfiguration-)<br />[JDialog(Window owner)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Frame-java.lang.String-boolean-java.awt.GraphicsConfiguration-)<br />[JDialog(Window owner, Dialog.ModalityType modalityType)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Window-java.awt.Dialog.ModalityType-)<br />[JDialog(Window owner, String title)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Window-java.lang.String-)<br />[JDialog(Window owner, String title, Dialog.ModalityType modalityType)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Window-java.lang.String-java.awt.Dialog.ModalityType-)<br />[JDialog(Window owner, String title, Dialog.ModalityType modalityType, GraphicsConfiguration gc)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#JDialog-java.awt.Window-java.lang.String-java.awt.Dialog.ModalityType-java.awt.GraphicsConfiguration-)</td><td headers="h702">Creates a `JDialog` instance. The `Frame` argument, if any, is the frame (usually a `JFrame` object) that the dialog depends on. Make the boolean argument `true` to specify a modal dialog, `false` or absent to specify a non-modal dialog. You can also specify the title of the dialog, using a string argument.</td>
<td headers="h701" scope="row">[void setContentPane(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#setContentPane-java.awt.Container-)<br />[Container getContentPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#getContentPane--)</td><td headers="h702">Get and set the content pane, which is usually the container of all the dialog's components. See [Using Top-Level Containers](toplevel.html) for more information.</td>
<td headers="h701" scope="row">[void setDefaultCloseOperation(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#setDefaultCloseOperation-int-)<br />[int getDefaultCloseOperation()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#getDefaultCloseOperation--)</td><td headers="h702">Get and set what happens when the user tries to close the dialog. Possible values: `DISPOSE_ON_CLOSE`, `DO_NOTHING_ON_CLOSE`, `HIDE_ON_CLOSE` (the default). See [Responding to Window-Closing Events](frame.html#windowevents) for more information.</td>
<td headers="h701" scope="row">[void setLocationRelativeTo(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#setLocationRelativeTo-java.awt.Component-)</td><td headers="h702">Centers the dialog over the specified component.</td>
<td headers="h701" scope="row">[static void setDefaultLookAndFeelDecorated(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#setDefaultLookAndFeelDecorated-boolean-)<br />[static boolean isDefaultLookAndFeelDecorated()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDialog.html#isDefaultLookAndFeelDecorated--)</td><td headers="h702">Set or get a hint as to whether the dialog's window decorations (such as borders, or widgets to close the window) should be provided by the current look and feel. Otherwise the dialog's decorations will be provided by the current window manager. See [Specifying Window Decorations](frame.html#setDefaultLookAndFeelDecorated) for more information.</td>




## <a name="eg" id="eg">Examples that Use Dialogs</a>

This table lists examples that use `JOptionPane` or `JDialog`. To find other examples that use dialogs, see the example lists for [progress bars](progress.html#eg), [color choosers](colorchooser.html#eg), and [file choosers](filechooser.html#eg).
<th id="h801" scope="col" align="left">Example</th><th id="h802" scope="col" align="left">Where Described</th><th id="h803" scope="col" align="left">Notes</th>
<td headers="h801" scope="row">[`DialogDemo`](../examples/components/index.html#DialogDemo),<br />[`CustomDialog`](../examples/components/index.html#DialogDemo)</td><td headers="h802">This section</td><td headers="h803">Creates many kinds of dialogs, using `JOptionPane` and `JDialog`.</td>
<td headers="h801" scope="row">[`Framework`](../examples/components/index.html#Framework)</td><td headers="h802">&#151;</td><td headers="h803">Brings up a confirmation dialog when the user selects the Quit menu item.</td>
<td headers="h801" scope="row">[`ListDialog`](../examples/components/index.html#ListDialog)</td><td headers="h802">[How to Use BoxLayout](../layout/box.html)</td><td headers="h803">Implements a modal dialog containing a scrolling list and two buttons. Does not use `JOptionPane`, except for the utility method `getFrameForComponent`.</td>
