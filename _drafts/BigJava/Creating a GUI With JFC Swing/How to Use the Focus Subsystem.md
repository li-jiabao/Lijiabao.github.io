
# How to Use the Focus Subsystem

Many components &#150; even those primarily operated with the mouse, such as buttons &#150; can be operated with the keyboard. For a key press to affect a component, the component must have the keyboard focus.

From the user's point of view, the component with the keyboard focus is generally prominent &#150; with a dotted or black border, for example. The window containing the component is also more prominent than other windows onscreen. These visual cues let the user know to which component any typing will relate. Only one component at a time in the window system can have the keyboard focus.

Exactly how a window gains the focus depends on the windowing system. There is no foolproof way, across all platforms, to ensure that a window gains the focus. On some operating systems, such as Microsoft Windows, the front window usually becomes the focused window. In these cases, the 
[`Window.toFront`](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html#toFront--) method moves the window to the front, thereby giving it the focus. However, on other operating systems, such as Solaris&#8482; Operating System, the window manager may choose the focused window based on cursor position, and in these cases the behavior of the `Window.toFront` method is different.

A component generally gains the focus when the user clicks it, or when the user tabs between components, or otherwise interacts with a component. A component can also be given the focus programmatically, such as when its containing frame or dialog-box is made visible. This code snippet shows how to give a particular component the focus every time the window gains the focus:

```

//Make textField get the focus whenever frame is activated.
frame.addWindowFocusListener(new WindowAdapter() {
    public void windowGainedFocus(WindowEvent e) {
        textField.requestFocusInWindow();
    }
});

```

If you want to ensure that a particular component gains the focus the first time a window is activated, you can call the 
[`requestFocusInWindow`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#requestFocusInWindow--) method on the component after the component has been realized, but before the frame is displayed. The following sample code shows how this operation can be done:

```

    **//...Where initialization occurs...**
    JFrame frame = new JFrame("Test");
    JPanel panel = new JPanel(new BorderLayout());

    **//...Create a variety of components here...**

    //Create the component that will have the initial focus.
    JButton button = new JButton("I am first");
    panel.add(button);
    frame.getContentPane().add(panel);  //Add it to the panel

    frame.pack();  //Realize the components.
    <b>//This button will have the initial focus.
    button.requestFocusInWindow();</b> 
    frame.setVisible(true); //Display the window.

```

Alternatively, you can apply a custom `FocusTraversalPolicy` to the frame and call the `getDefaultComponent` method to determine which component will gain the focus.

The rest of this section covers the following topics:

- [Introduction to the Focus Subsystem](#intro)
- [Validating Input](#inputVerification)
- [Making a Custom Component Focusable](#focusable)
- [Customizing Focus Traversal](#customFocusTraversal)
- [Tracking Focus Changes to Multiple Components](#trackingFocus)
- [Timing Focus Transfers](#transferTiming)
- [The Focus API](#api)
- [Focus Examples](#eg)

## <a name="intro" id="intro">Introduction to the Focus Subsystem</a>

The focus subsystem is designed to do the right thing as invisibly as possible. In most cases it behaves in a reasonable manner, and if it does not you can tweak its behavior in various ways. Some common scenarios might include:

- The ordering is right but the first component with the focus is not set. As shown in a code snippet in the preceding section, you can use the `requestFocusInWindow` method to set the focus on a component when the window becomes visible.
- The ordering is wrong. To fix this issue, you can change the containment hierarchy, you can change the order that the components are added to their containers, or you can create a custom focus traversal policy. For more details see [Customizing Focus Traversal](#customFocusTraversal).
- A component must to be prevented from losing focus, or you need to check a value in a component before it loses focus. [Input verification](#inputVerification) is a solution to this problem.
- A custom component is not getting the focus. To fix this issue, you need to make sure that it satisfies all the requirements outlined in [Making a Custom Component Focusable](#focusable).

The `FocusConceptsDemo` example illustrates a few concepts.

<li>Click the Launch button to run FocusConceptsDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#FocusConceptsDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the FocusConceptsDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FocusConceptsDemoProject/FocusConceptsDemo.jnlp)<br /></li>
1. If necessary, click the window to give it the focus.
<li>Move the focus from component to component using the Tab key.<br />
You will notice that when the focus moves into the text area, it stays in the text area.</li>
1. Move the focus out of the text area using Control-Tab.
1. Move the focus in the opposite direction using Shift-Tab.
1. Move the focus out of the text area in the opposite direction using Control-Shift-Tab.

The `KeyboardFocusManager` is a critical element of the focus subsystem. It manages state and initiates changes. The keyboard manager tracks the *focus owner* &#226;&#128;&#148; the component that receives typing from the keyboard. The *focused window* is the window that contains the focus owner.

A *focus cycle* (or *focus traversal cycle*) is a set of components that share a common ancestor in the containment hierarchy. The *focus cycle root* is the container that is the root for a particular focus traversal cycle. By default, every `JWindow` and `JInternalFrame` component can be a focus cycle root. A focus cycle root can itself contain one or more focus cycle roots. The following Swing objects can be focus cycle roots: `JApplet`, `JDesktopPane`, `JDialog`, `JEditorPane`, `JFrame`, `JInternalFrame`, and `JWindow`. While it might appear that `JTable` and `JTree` objects are focus cycle roots, they are not.

A *focus traversal policy* determines the order in which a group of components are navigated. Swing provides the 
[`<code>LayoutFocusTraversalPolicy`</code>](https://docs.oracle.com/javase/8/docs/api/javax/swing/LayoutFocusTraversalPolicy.html) class, which decides the order of navigation based on layout manager-dependent factors, such as size, location, and orientation of components. Within a focus cycle, components can be navigated in a forward or backward direction. In a hierarchy of focus cycle roots, upwards traversal takes the focus out of the current cycle into the parent cycle. <a name="addreturn" id="addreturn"></a>

In most Look and Feel models, components are navigated using the Tab and Shift-Tab keys. These keys are the default *focus traversal keys* and can be changed programmatically. For example, you can add Enter as a forward focus traversal key with the following four lines of code:

```

Set forwardKeys = getFocusTraversalKeys(
    KeyboardFocusManager.FORWARD_TRAVERSAL_KEYS);
Set newForwardKeys = new HashSet(forwardKeys);
newForwardKeys.add(KeyStroke.getKeyStroke(KeyEvent.VK_ENTER, 0));
setFocusTraversalKeys(KeyboardFocusManager.FORWARD_TRAVERSAL_KEYS,
    newForwardKeys);

```

Tab shifts the focus in the forward direction. Shift-Tab moves the focus in the backward direction. For example, in FocusConceptsDemo, the first button has the initial focus. Tabbing moves the focus through the buttons into the text area. Additional tabbing moves the cursor within the text area but not out of the text area because, inside a text area, Tab is **not** a focus traversal key. However, Control-Tab moves the focus out of the text area and into the first text field. Likewise, Control-Shift-Tab moves the focus out of the text area and into the previous component. The Control key is used by convention to move the focus out of any component that treats Tab in a special way, such as `JTable`.

You have just received a brief introduction to the focus architecture. If you want more details, see the specification for the 
[Focus Subsystem](https://docs.oracle.com/javase/8/docs/api/java/awt/doc-files/FocusSpec.html).

## <a name="inputVerification" id="inputVerification">Validating Input</a>

A common requirement of GUI design is a component that restricts the user's input &#226;&#128;&#148; for example, a text field that allows only numeric input in decimal format (money, for example) or a text field that allows only 5 digits for a zip code. An easy-to-use 
[formatted text field](../components/formattedtextfield.html) component that allows input to be restricted to a variety of localizable formats. You can also specify a 
[custom formatter](../components/formattedtextfield.html) for the text field, which can perform special checking such as determining whether values are not only formatted correctly, but also reasonable.

You can use an input verifier as an alternative to a custom formatter, or when you have a component that is not a text field. An input verifier allows you to reject specific values, such as a properly formatted but invalid zip code, or values outside of a desired range, for example a body temperature higher than 110&#176;F. To use an input verifier, you create a subclass of 
[`<code>InputVerifier`</code>](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputVerifier.html), create an instance of your subclass, and set the instance as the input verifier for one or more components.

A component's input verifier is consulted whenever the component is about to lose the focus. If the component's value is not acceptable, the input verifier can take appropriate action, such as refusing to yield the focus on the component or replacing the user's input with the last valid value and then allowing the focus to transfer to the next component. However, `InputVerifier` is not called when the focus is transferred to another toplevel component.

The following two examples show mortgage calculators. One uses formatted text fields and the other uses input verification with standard text fields.

<li>Click the Launch button to run the FormattedTextFieldDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#FormattedTextFieldDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the FormattedTextFieldDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FormattedTextFieldDemoProject/FormattedTextFieldDemo.jnlp)<br /></li>
<li>Click the Launch button to run the InputVerificationDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#InputVerificationDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the InputVerificationDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/InputVerificationDemoProject/InputVerificationDemo.jnlp)<br /></li>
1. Compare the two mortgage calculators side by side. You will see that the input verification demo specifies valid input values in the associated label for each editable text field. Try entering badly formatted values in both examples to observe behavior. Then try entering a properly formatted, but unacceptable value.

You can find the code for the Input Verification demo in 
[`InputVerificationDemo.java`](../examples/misc/InputVerificationDemoProject/src/misc/InputVerificationDemo.java). Here is the code for the `InputVerifier` subclass, `MyVerifier`:

```

class MyVerifier extends InputVerifier
                 implements ActionListener {
    double MIN_AMOUNT = 10000.0;
    double MAX_AMOUNT = 10000000.0;
    double MIN_RATE = 0.0;
    int MIN_PERIOD = 1;
    int MAX_PERIOD = 40;

   public boolean shouldYieldFocus(JComponent input) {
        boolean inputOK = verify(input);
        makeItPretty(input);
        updatePayment();

        if (inputOK) {
            return true;
        } else {
            Toolkit.getDefaultToolkit().beep();
            return false;
        }
    }

    protected void updatePayment() {
        double amount = DEFAULT_AMOUNT;
        double rate = DEFAULT_RATE;
        int numPeriods = DEFAULT_PERIOD;
        double payment = 0.0;

        //Parse the values.
        try {
            amount = moneyFormat.parse(amountField.getText()).
                              doubleValue();
        } catch (ParseException pe) {pe.printStackTrace();}
        try {
            rate = percentFormat.parse(rateField.getText()).
                                 doubleValue();
        } catch (ParseException pe) {pe.printStackTrace();}
        try {
            numPeriods = decimalFormat.parse(numPeriodsField.getText()).
                              intValue();
        } catch (ParseException pe) {pe.printStackTrace();}

        //Calculate the result and update the GUI.
        payment = computePayment(amount, rate, numPeriods);
        paymentField.setText(paymentFormat.format(payment));
    }

    //This method checks input, but should cause no side effects.
    public boolean verify(JComponent input) {
        return checkField(input, false);
    }

    protected void makeItPretty(JComponent input) {
        checkField(input, true);
    }

    protected boolean checkField(JComponent input, boolean changeIt) {
        if (input == amountField) {
            return checkAmountField(changeIt);
        } else if (input == rateField) {
            return checkRateField(changeIt);
        } else if (input == numPeriodsField) {
            return checkNumPeriodsField(changeIt);
        } else {
            return true; //should not happen
        }
    }

    //Checks that the amount field is valid.  If it is valid,
    //it returns true; otherwise, returns false.  If the
    //change argument is true, this method sets the
    //value to the minimum or maximum value if necessary and
    // (even if not) sets it to the parsed number so that it
    // looks good -- no letters, for example.
    protected boolean checkAmountField(boolean change) {
        boolean wasValid = true;
        double amount = DEFAULT_AMOUNT;

        //Parse the value.
        try {
            amount = moneyFormat.parse(amountField.getText()).
                              doubleValue();
        } catch (ParseException pe) {
            pe.printStackTrace();
            wasValid = false;
        }

        //Value was invalid.
        if ((amount &lt; MIN_AMOUNT) || (amount &gt; MAX_AMOUNT)) {
            wasValid = false;
            if (change) {
                if (amount &lt; MIN_AMOUNT) {
                    amount = MIN_AMOUNT;
                } else { // amount is greater than MAX_AMOUNT
                    amount = MAX_AMOUNT;
                }
            }
        }

        //Whether value was valid or not, format it nicely.
        if (change) {
            amountField.setText(moneyFormat.format(amount));
            amountField.selectAll();
        }

        return wasValid;
    }

    //Checks that the rate field is valid.  If it is valid,
    //it returns true; otherwise, returns false.  If the
    //change argument is true, this method reigns in the
    //value if necessary and (even if not) sets it to the
    //parsed number so that it looks good -- no letters,
    //for example.
    protected boolean checkRateField(boolean change) {
        **...//Similar to checkAmountField...**
    }

    //Checks that the numPeriods field is valid.  If it is valid,
    //it returns true; otherwise, returns false.  If the
    //change argument is true, this method reigns in the
    //value if necessary and (even if not) sets it to the
    //parsed number so that it looks good -- no letters,
    //for example.
    protected boolean checkNumPeriodsField(boolean change) {
        **...//Similar to checkAmountField...**
    }

    public void actionPerformed(ActionEvent e) {
        JTextField source = (JTextField)e.getSource();
        shouldYieldFocus(source); //ignore return value
        source.selectAll();
    }
}

```

Note that the `verify` method is implemented to detect invalid values but does nothing else. The `verify` method exists only to determine whether the input is valid &#226;&#128;&#148; it should never bring up a dialog-box or cause any other side effects. The `shouldYieldFocus` method calls `verify` and, if a values is invalid, sets it to the minimum or maximum value. The `shouldYieldFocus` method is allowed to cause side effects, in this case, it always formats the text field and may also change its value. In our example, the `shouldYieldFocus` method always returns true so that the transfer of the focus is never actually prevented. This is just one way verification can be implemented. Find another version of this demo called [`InputVerificationDialogDemo`](../examples/misc/index.html#InputVerificationDialogDemo) that puts up a dialog-box when user input is invalid and requires the user to enter a legal value.

The input verifier is installed using the `setInputVerifier` method of the `JComponent` class. For example, the `InputVerificationDemo` has the following code:

```

private MyVerifier verifier = new MyVerifier();
...
amountField.setInputVerifier(verifier);

```

## <a name="focusable" id="focusable">Making a Custom Component Focusable</a>

For a component to gain the focus, it must satisfy three requirements: it must be visible, enabled, and focusable. An input map may also be given. For more information about input map, read 
[How to Use Key Bindings](../misc/keybinding.html).

The [TrackFocusDemo](#trackfocusdemo) example defines the simple component `Picture`. Its constructor is shown below:

```

public Picture(Image image) {
    this.image = image;
    setFocusable(true);
    addMouseListener(this);
    addFocusListener(this);
}

```

The call to the `setFocusable(true)` method makes the component focusable. If you explicitly give your component key bindings in its `WHEN_FOCUSED` input map, you do not need to call the `setFocusable` method.

To visually show changes in the focus (by drawing a red border only when the component has the focus), `Picture` has a 
[focus listener](../events/focuslistener.html).

To gain the focus when the user clicks on the picture, the component has a 
[mouse listener](../events/mouselistener.html). The listener's `mouseClicked` method requests for the focus to be transferred to the picture. Here is the code:

```

public void mouseClicked(MouseEvent e) {
    //Since the user clicked on us, let us get focus!
    requestFocusInWindow();
}

```

See [Tracking Focus Changes to Multiple Components](#trackingFocus) for more discussion of the TrackFocusDemo example.

## <a name="customFocusTraversal" id="customFocusTraversal">Customizing Focus Traversal</a>

The focus subsystem determines a default order that is applied when using the focus traversal keys (such as Tab) to navigate. The policy of a Swing application is determined by 
[`LayoutFocusTraversalPolicy`](https://docs.oracle.com/javase/8/docs/api/javax/swing/LayoutFocusTraversalPolicy.html) . You can set a focus traversal policy on any `Container` by using the 
[`setFocusCycleRoot`](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setFocusCycleRoot-boolean-) method. However, if the container is not a focus cycle root, it may have no apparent effect. Alternatively you can pass focus traversal policy providers to the `FocusTraversalPolicy` methods instead of focus cycle roots. Use the 
[`isFocusTraversalPolicyProvider()`](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#isFocusTraversalPolicyProvider--) method to determine whether a `Container` is a focus traversal policy provider. Use the 
[`setFocusTraversalPolicyProvider()`](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setFocusTraversalPolicyProvider-boolean-) method to set a container for providing focus traversal policy.

The `FocusTraversalDemo` example demonstrates how to customize focus behavior.

<a name="focustraversaldemo" id="focustraversaldemo"></a>

<li>Click the Launch button to run FocusTraversalDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#FocusTraversalDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the FocusTraversalDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FocusTraversalDemoProject/FocusTraversalDemo.jnlp)<br /></li>
1. Click the window, if necessary, to give it the focus.
1. Note the focus order as you tab through the components. The focus order was determined by the order that the components were added to the content pane. Note also that the check box never gets the focus; we removed it from the focus cycle.
1. To move the focus out of the table, use Control-Tab or Control-Shift-Tab.
1. Click the **Custom FocusTraversalPolicy** check box. This box installs a custom focus traversal policy on the frame.
1. Try tabbing through the components again. Note that the focus order is now in left-to-right, top-down order.

You can find the demo's code in 
[`FocusTraversalDemo.java`](../examples/misc/FocusTraversalDemoProject/src/misc/FocusTraversalDemo.java).

The check box was removed from the focus cycle with this line of code:

```

togglePolicy.setFocusable(false);

```

Here is the application's custom `FocusTraversalPolicy`:

```

...
JTextField tf1 = new JTextField("Field 1");
JTextField tf2 = new JTextField("A Bigger Field 2");
JTextField tf3 = new JTextField("Field 3");
JTextField tf4 = new JTextField("A Bigger Field 4");
JTextField tf5 = new JTextField("Field 5");
JTextField tf6 = new JTextField("A Bigger Field 6");
JTable table = new JTable(4,3);
...
public FocusTraversalDemo() {
    super(new BorderLayout());

    JTextField tf1 = new JTextField("Field 1");
    JTextField tf2 = new JTextField("A Bigger Field 2");
    JTextField tf3 = new JTextField("Field 3");
    JTextField tf4 = new JTextField("A Bigger Field 4");
    JTextField tf5 = new JTextField("Field 5");
    JTextField tf6 = new JTextField("A Bigger Field 6");
    JTable table = new JTable(4,3);
    togglePolicy = new JCheckBox("Custom FocusTraversalPolicy");
    togglePolicy.setActionCommand("toggle");
    togglePolicy.addActionListener(this);
    togglePolicy.setFocusable(false);  //Remove it from the focus cycle.
    //Note that HTML is allowed and will break this run of text
    //across two lines.
    label = new JLabel("&lt;html&gt;Use Tab (or Shift-Tab) to navigate from component to component.&lt;p&gt;Control-Tab 
    (or Control-Shift-Tab) allows you to break out of the JTable.&lt;/html&gt;");

    JPanel leftTextPanel = new JPanel(new GridLayout(3,2));
    leftTextPanel.add(tf1, BorderLayout.PAGE_START);
    leftTextPanel.add(tf3, BorderLayout.CENTER);
    leftTextPanel.add(tf5, BorderLayout.PAGE_END);
    leftTextPanel.setBorder(BorderFactory.createEmptyBorder(0,0,5,5));
    JPanel rightTextPanel = new JPanel(new GridLayout(3,2));
    rightTextPanel.add(tf2, BorderLayout.PAGE_START);
    rightTextPanel.add(tf4, BorderLayout.CENTER);
    rightTextPanel.add(tf6, BorderLayout.PAGE_END);
    rightTextPanel.setBorder(BorderFactory.createEmptyBorder(0,0,5,5));
    JPanel tablePanel = new JPanel(new GridLayout(0,1));
    tablePanel.add(table, BorderLayout.CENTER);
    tablePanel.setBorder(BorderFactory.createEtchedBorder());
    JPanel bottomPanel = new JPanel(new GridLayout(2,1));
    bottomPanel.add(togglePolicy, BorderLayout.PAGE_START);
    bottomPanel.add(label, BorderLayout.PAGE_END);

    add(leftTextPanel, BorderLayout.LINE_START);
    add(rightTextPanel, BorderLayout.CENTER);
    add(tablePanel, BorderLayout.LINE_END);
    add(bottomPanel, BorderLayout.PAGE_END);
    setBorder(BorderFactory.createEmptyBorder(20,20,20,20));
    Vector&lt;Component&gt; order = new Vector&lt;Component&gt;(7);
    order.add(tf1);
    order.add(tf2);
    order.add(tf3);
    order.add(tf4);
    order.add(tf5);
    order.add(tf6);
    order.add(table);
    newPolicy = new MyOwnFocusTraversalPolicy(order);
}

```

To use a custom `FocusTraversalPolicy`, implement the following code on any focus cycle root.

```

    MyOwnFocusTraversalPolicy newPolicy = new MyOwnFocusTraversalPolicy();
    frame.setFocusTraversalPolicy(newPolicy);

```

You can remove the custom focus traversal policy by setting the `FocusTraversalPolicy` to `null`, which will restore the default policy.

## <a name="trackingFocus" id="trackingFocus">Tracking Focus Changes to Multiple Components</a>

In some situations an application may need to track which component has the focus. This information might be used to dynamically update menus or perhaps a status bar. If you need to track the focus only on specific components, it may make sense to implement a 
[focus event listener](../events/focuslistener.html).

If a focus listener is not appropriate, you can instead register a `PropertyChangeListener` on the `KeyboardFocusManager`. The property change listener is notified of every change involving the focus, including changes to the focus owner, the focused window, and the default focus traversal policy. See the [KeyboardFocusManager Properties](#properties) table for a complete list.

<a name="trackfocusdemo" id="trackfocusdemo">The</a> following example demonstrates tracking the focus owner by installing a property change listener on the keyboard focus manager.

<li>Click the Launch button to run TrackFocusDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/misc/index.html#TrackFocusDemo).[<img src="../../images/jws-launch-button.png" width="88" align="bottom" alt="Launches the TrackFocusDemo application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TrackFocusDemoProject/TrackFocusDemo.jnlp)<br /></li>
1. If necessary, click the window to give it the focus.
1. The window shows six images, each of which is displayed by a `Picture` component. The `Picture` that has the focus is indicated with a red border. A label at the bottom of the window describes the `Picture` that has the focus.
1. Move the focus to another `Picture` by using Tab or Shift-Tab, or by clicking an image. Because a property change listener has been registered on the keyboard focus manager, the change in focus is detected and the label is updated appropriately.

You can view the demo's code in 
[`TrackFocusDemo.java`](../examples/misc/TrackFocusDemoProject/src/misc/TrackFocusDemo.java). The custom component used for drawing the images can be found in 
[`Picture.java`](../examples/misc/TrackFocusDemoProject/src/misc/Picture.java). Here is the code that defines and installs the property change listener:

```

KeyboardFocusManager focusManager =
    KeyboardFocusManager.getCurrentKeyboardFocusManager();
focusManager.addPropertyChangeListener(
    new PropertyChangeListener() {
        public void propertyChange(PropertyChangeEvent e) {
            String prop = e.getPropertyName();
            if (("focusOwner".equals(prop)) &amp;&amp;
                  ((e.getNewValue()) instanceof Picture)) {
                Component comp = (Component)e.getNewValue();
                String name = comp.getName();
                Integer num = new Integer(name);
                int index = num.intValue();
                if (index &lt; 0 || index &gt; comments.length) {
                    index = 0;
                }
                info.setText(comments[index]);
            }
        }
    }
);

```

The custom component, `Picture`, is responsible for drawing the image. All six components are defined in this manner:

```

pic1 = new Picture(createImageIcon("images/" +
            mayaString + ".gif", mayaString).getImage());
pic1.setName("1");

```

## <a name="transferTiming" id="transferTiming">Timing Focus Transfers</a>

Focus transfers are asynchronous. This quality can lead to some odd timing-related problems and assumptions, especially during automatic transfers of the focus. For example, imagine an application with a window containing a Start button, a Cancel button and a text field. The components are added in this order:

1. Start button
1. Text field
1. Cancel button

When the application is launched, the `LayoutFocusTraversalPolicy` determines the focus traversal policy &#226;&#128;&#148; in this case, it is the order that the components were added to their container. In this example, the desired behavior is that the Start button has the initial focus, and when the Start button is clicked, it is disabled, and then the Cancel button receives the focus. The correct way to implement this behavior would be to add the components to the container in the desired order or to create a custom focus traversal policy. If, for some reason, that is not possible, then you can implement this behavior with the following code snippet:

```

public void actionPerformed(ActionEvent e) {
    //This works.
    start.setEnabled(false);
    cancel.requestFocusInWindow();
}

```

As desired, the focus goes from the Start button to the Cancel button, rather than to the text field. But a different result would occur if the same methods were called in the opposite order as follows:

```

public void actionPerformed(ActionEvent e) {
    //This does not work.
    cancel.requestFocusInWindow();
    start.setEnabled(false);
}

```

In this case, the focus is requested on the Cancel button before it has left the Start button. The call to the `requestFocusInWindow` method initiates the focus transfer, but it does not immediately move the focus to the Cancel button. When the Start button is disabled, the focus is transferred to the next component (so there is always a component with the focus) and, in this case, it would then move the focus to the text field, **not** to the Cancel button.

There are several situations in which you need to make focus requests after all other changes that might affect the focus applies to:

- Hiding the focus owner.
- Making the focus owner non-focusable.
- Calling the `removeNotify` method on the focus owner.
- Doing any of the above operations to the container of the focus owner, or causing changes to the focus policy so that the container no longer accepts the component as the focus owner.
- Disposing of the top-level window that contains the focus owner.

### <a name="api" id="api">The Focus API</a>

The following tables list the commonly used constructors and methods related to focus. The focus API falls into four categories:

- [Useful Methods for Components](#focusMethods)
- [Creating and Using a Custom FocusTraversalPolicy](#focustraversal)
- [Input Verification API](#inputverificationapi)
- [KeyboardFocusManager Properties](#properties)

For more detailed information about the focus architecture, see the specification for the 
[Focus Subsystem](https://docs.oracle.com/javase/8/docs/api/java/awt/doc-files/FocusSpec.html). You may also find 
[How to Write a Focus Listener](../events/focuslistener.html) useful.

<a name="focusMethods" id="focusMethods">Useful Methods for Components</a>
<th id="h1" align="left">Method (in `Component`)</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[isFocusOwner()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#isFocusOwner--)</td><td headers="h2">Returns `true` if the component is the focus owner.</td>
<td headers="h1">[setRequestFocusEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setRequestFocusEnabled-boolean-)<br />[isRequestFocusEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#isRequestFocusEnabled--)<br />(**in `JComponent`)**</td><td headers="h2">Sets or checks on whether this component should get the focus. Setting the `setRequestFocusEnabled` to `false` typically prevents mouse clicks from giving the component the focus, while still allowing keyboard navigation to give the component the focus. This method applies only to components that receive mouse events. For example, you can use this method on a `JButton`, but not on a `JPanel`. If you write a custom component it is up to you to honor this property. This method is recommended over the `setFocusable` method and will allow your program to work better for users employing [assistive technologies](access.html).</td>
<td headers="h1">[setFocusable(boolean)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setFocusable-boolean-)<br />[isFocusable()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#isFocusable--)</td><td headers="h2">Sets or gets the focusable state of the component. A component must be focusable in order to gain the focus. When a component has been removed from the focus cycle with `setFocusable(false)`, it can no longer be navigated with the keyboard. The `setRequestFocusEnabled` method is recommended so that your program can be run by users employing [assistive technologies](access.html).</td>
<td headers="h1">[requestFocusInWindow()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#requestFocusInWindow--)</td><td headers="h2">Requests that this component should get the focus. The component's window must be the current focused window. For this request to be granted a subclass of `JComponent` must be visible, enabled, and focusable, and have an input map for this request to be granted. It should not be assumed that the component has the focus until it fires a `FOCUS_GAINED` event. This method is preferred to the `requestFocus` method, which is platform-dependent.</td>
<td headers="h1">[setFocusTraversalKeys(int, Set)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setFocusTraversalKeys-int-java.util.Set-)<br />[getFocusTraversalKeys(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getFocusTraversalKeys-int-)<br />[areFocusTraversalKeysSet(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#areFocusTraversalKeysSet-int-)<br />(**in `java.awt.Container`**)</td><td headers="h2">Sets or gets the focus traversal keys for a particular direction or determines whether any focus traversal keys have been explicitly set on this container. If no focus traversal keys have been set, they are inherited from an ancestor or from the keyboard focus manager. Focus traversal keys can be set for the following directions: `KeyboardFocusManager.FORWARD_TRAVERSAL_KEYS`, `KeyboardFocusManager.BACKWARD_TRAVERSAL_KEYS` `KeyboardFocusManager.UP_CYCLE_TRAVERSAL_KEYS`, or `KeyboardFocusManager.DOWN_CYCLE_TRAVERSAL_KEYS`. If you set the `UP_CYCLE_TRAVERSAL_KEYS` or the `DOWN_CYCLE_TRAVERSAL_KEYS`, you must also invoke [`setImplicitDownCycleTraversal(false)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SortingFocusTraversalPolicy.html#setImplicitDownCycleTraversal-boolean-) on the focus traversal policy.</td>

<a name="focustraversal" id="focustraversal">Creating and Using a Custom FocusTraversalPolicy</a>
<th id="h101" align="left">Class or Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[LayoutFocusTraversalPolicy](https://docs.oracle.com/javase/8/docs/api/javax/swing/LayoutFocusTraversalPolicy.html)</td><td headers="h102">The class that, by default, determines the focus traversal policy for Swing components.</td>
<td headers="h101">[getComponentAfter(Container, Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/LayoutFocusTraversalPolicy.html#getComponentAfter-java.awt.Container-java.awt.Component-)</td><td headers="h102">Given the component that is passed as input, returns the component that should next have the focus.</td>
<td headers="h101">[getComponentBefore(Container, Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/LayoutFocusTraversalPolicy.html#getComponentBefore-java.awt.Container-java.awt.Component-)</td><td headers="h102">Given the component that is passed as input, returns the component that should have the focus before this component. The method is used for backward tabbing.</td>
<td headers="h101">[getDefaultComponent(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SortingFocusTraversalPolicy.html#getDefaultComponent-java.awt.Container-)<br />(**in `javax.swing.SortingFocusTraversalPolicy`)**</td><td headers="h102">Returns the component that should have the default focus.</td>
<td headers="h101">[getFirstComponent(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/LayoutFocusTraversalPolicy.html#getFirstComponent-java.awt.Container-)</td><td headers="h102">Returns the first component in the traversal cycle.</td>
<td headers="h101">[getInitialComponent(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/InternalFrameFocusTraversalPolicy.html#getInitialComponent-java.awt.Container-)</td><td headers="h102">Returns the component that should receive the focus when a window is made visible for the first time.</td>
<td headers="h101">[getLastComponent(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/LayoutFocusTraversalPolicy.html#getLastComponent-java.awt.Container-)</td><td headers="h102">Returns the last component in the traversal cycle.</td>
<td headers="h101">[setFocusTraversalPolicy(FocusTraversalPolicy)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setFocusTraversalPolicy-java.awt.FocusTraversalPolicy-)<br />[getFocusTraversalPolicy(FocusTraversalPolicy)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getFocusTraversalPolicy--)<br />(**in `java.awt.Container`**)</td><td headers="h102">Sets or gets the focus traversal policy or determines if a policy has been set. Note that setting a focus traversal policy on a container that is not the focus cycle root may have no apparent effect. A value of `null` means that a policy has not been explicitly set. If no policy has been set, a policy is inherited from the parent focus cycle root.</td>
<td headers="h101">[isFocusCycleRoot()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#isFocusCycleRoot--)<br />[setFocusCycleRoot(boolean)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setFocusCycleRoot-boolean-)<br />(**in `java.awt.Container`**)</td><td headers="h102">Checks or sets whether a container is the root of a focus traversal cycle.</td>
<td headers="h101">[isFocusTraversalPolicyProvider()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#isFocusTraversalPolicyProvider--)<br />[setFocusTraversalPolicyProvider(boolean)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setFocusTraversalPolicyProvider-boolean-)<br />(**in `java.awt.Container`**)</td><td headers="h102">Checks or sets whether a container will be used to provide focus traversal policy.</td>

<a name="inputverificationapi" id="inputverificationapi">Input Verification API</a>
<th id="h201" align="left">Class or Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[InputVerifier](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputVerifier.html)</td><td headers="h202">Abstract class that allows input validation via the focus mechanism. When an attempt is made to shift the focus from a component containing an input verifier, the focus is not relinquished until the verifier is satisfied.</td>
<td headers="h201">[shouldYieldFocus(JComponent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputVerifier.html#shouldYieldFocus-javax.swing.JComponent-)<br />**(in `InputVerifier`)**</td><td headers="h202">When a component has an input verifier, this method is called by the system to determine whether the focus can leave this component. This method may cause side effects, such as bringing up a dialog-box. If this method returns `false`, the focus remains on the component passed in to the method.</td>
<td headers="h201">[verify(JComponent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/InputVerifier.html#verify-javax.swing.JComponent-)<br />**(in `InputVerifier`)**</td><td headers="h202">You need to override this method to check that the component's input is valid. It should return `true` if valid, otherwise return `false`. This method should not cause any side effects, such as bringing up a dialog-box. This method is called by `shouldYieldFocus`.</td>
<td headers="h201">[setInputVerifier(inputVerifier)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setInputVerifier-javax.swing.InputVerifier-)<br />[getInputVerifier()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getInputVerifier--)<br />**(in `JComponent`)**</td><td headers="h202">Sets or gets the input verifier assigned to the component. By default, components have no input verifier.</td>
<td headers="h201">[setVerifyInputWhenFocusTarget(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setVerifyInputWhenFocusTarget-boolean-)<br />[getVerifyInputWhenFocusTarget()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getVerifyInputWhenFocusTarget--)<br />**(in `JComponent`)**</td><td headers="h202">Sets or gets whether the input verifier for the current focus owner is called before this component requests the focus. The default is `true`. This method should be set to `false` for components, such as a Cancel button or a scroll bar, that should receive the focus even if input is invalid.</td>

<a name="properties" id="properties">KeyboardFocusManager Properties</a>

This table defines the bound properties for 
[`KeyboardFocusManager`](https://docs.oracle.com/javase/8/docs/api/java/awt/KeyboardFocusManager.html). A listener can be registered for these properties by calling 
[`addPropertyChangeListener`](https://docs.oracle.com/javase/8/docs/api/java/awt/KeyboardFocusManager.html#addPropertyChangeListener-java.beans.PropertyChangeListener-).
<th id="h301" align="left">Property</th><th id="h302" align="left">Purpose</th>
<td headers="h301">focusOwner</td><td headers="h302">The component that currently receives key events.</td>
<td headers="h301">permanentFocusOwner</td><td headers="h302">The component that most recently received a permanent `FOCUS_GAINED` event. Typically the same as `focusOwner`, unless a temporary focus change is currently in effect.</td>
<td headers="h301">focusedWindow</td><td headers="h302">The window that is or that contains the focus owner.</td>
<td headers="h301">activeWindow</td><td headers="h302">The component must always be either a `Frame` or a `Dialog`. The active window is either the focused window, or the first frame or dialog-box that is an owner of the focused window.</td>
<td headers="h301">defaultFocusTraversalPolicy</td><td headers="h302">The default focus traversal policy, which can be set by the `setFocusTraversalPolicy` method of the `Container` class.</td>
<td headers="h301">forwardDefaultFocusTraversalKeys</td><td headers="h302">The set of default focus keys for a forward traversal. For multi-line text components, these keys default to Control-Tab. For all other components, these keys default to Tab and Control-Tab.</td>
<td headers="h301">backwardDefaultFocusTraversalKeys</td><td headers="h302">The set of default focus keys for a backward traversal. For multi-line text components these keys default to Control-Shift-Tab. For all other components these keys default to Shift-Tab and Control-Shift-Tab.</td>
<td headers="h301">upCycleDefaultFocusTraversalKeys</td><td headers="h302">The set of default focus keys for an up cycle. These keys are null, by default, for Swing components. If you set these keys on the `KeyboardFocusManager`, or if you set the `downCycleFocusTraversalKeys` on a focus cycle root, you must also invoke the [`setImplicitDownCycleTraversal(false)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SortingFocusTraversalPolicy.html#setImplicitDownCycleTraversal-boolean-) method on the focus traversal policy.</td>
<td headers="h301">downCycleDefaultFocusTraversalKeys</td><td headers="h302">The set of default focus keys for a down cycle. These keys are null, by default, for Swing components. If you set these keys on the `KeyboardFocusManager`, or if you set the `upCycleFocusTraversalKeys` on a focus cycle root, you must also invoke the [`setImplicitDownCycleTraversal(false)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SortingFocusTraversalPolicy.html#setImplicitDownCycleTraversal-boolean-) method on the focus traversal policy.</td>
<td headers="h301">currentFocusCycleRoot</td><td headers="h302">The container that is the current focus cycle root.</td>

### <a name="eg" id="eg">Examples that Use Focus</a>

The following table lists examples that manipulate the focus:



