
# How to Use Buttons, Check Boxes, and Radio Buttons

To create a button, you can instantiate one of the many classes that descend from the 
[`AbstractButton`](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html) class. The following table shows the Swing-defined `AbstractButton` subclasses that you might want to use:
<th id="h1" align="left">Class</th><th id="h2" align="left">Summary</th><th id="h3" align="left">Where Described</th>

First, this section explains the basic button API that `AbstractButton` defines &#151; and thus all Swing buttons have in common. Next, it describes the small amount of API that `JButton` adds to `AbstractButton`. After that, this section shows you how to use specialized API to implement check boxes and radio buttons.

## <a name="abstractbutton" id="abstractbutton">How to Use the Common Button API</a>

Here is a picture of an application that displays three buttons:
<!-- *******  boilerplate stuff ******* -->
<li>Click the Launch button to run the Button Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ButtonDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ButtonDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ButtonDemoProject/ButtonDemo.jnlp)<br />
<p><!--  ******* end boilerplate stuff  *******  -->
 <!-- 

<li> [Run ButtonDemo](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ButtonDemoProject/ButtonDemo.jnlp) (
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).    Or, to compile and run the example yourself,
     consult the 
     [example index](../examples/components/index.html#ButtonDemo).
        
--></p>
</li>
<li>Click the left button.<br />
It disables the middle button (and itself, since it is no longer useful) and enables the right button.</li>
<li>Click the right button.<br />
It enables the middle button and the left button, and disables itself.</li>

As the `ButtonDemo` example shows, a Swing button can display both text and an image. In `ButtonDemo`, each button has its text in a different place, relative to its image. The underlined letter in each button's text shows the **mnemonic** &#151; the keyboard alternative &#151; for each button. In most look and feels, the user can click a button by pressing the Alt key and the mnemonic. For example, Alt-M would click the Middle button in ButtonDemo.

When a button is disabled, the look and feel automatically generates the button's disabled appearance. However, you could provide an image to be substituted for the normal image. For example, you could provide gray versions of the images used in the left and right buttons.

How you implement event handling depends on the type of button you use and how you use it. Generally, you implement an 
[action listener](../events/actionlistener.html), which is notified every time the user clicks the button. For [check boxes](#checkbox) you usually use an 
[item listener](../events/itemlistener.html), which is notified when the check box is selected or deselected.

Below is the code from 
[`ButtonDemo.java`](../examples/components/ButtonDemoProject/src/components/ButtonDemo.java) that creates the buttons in the previous example and reacts to button clicks. The bold code is the code that would remain if the buttons had no images.

```

**//In initialization code:**
    ImageIcon leftButtonIcon = createImageIcon("images/right.gif");
    ImageIcon middleButtonIcon = createImageIcon("images/middle.gif");
    ImageIcon rightButtonIcon = createImageIcon("images/left.gif");

    **b1 = new JButton("Disable middle button"**, leftButtonIcon**);**
    b1.setVerticalTextPosition(AbstractButton.CENTER);
    b1.setHorizontalTextPosition(AbstractButton.LEADING); //aka LEFT, for left-to-right locales
    **b1.setMnemonic(KeyEvent.VK_D);**
    **b1.setActionCommand("disable");**

    **b2 = new JButton("Middle button"**, middleButtonIcon**);**
    b2.setVerticalTextPosition(AbstractButton.BOTTOM);
    b2.setHorizontalTextPosition(AbstractButton.CENTER);
    **b2.setMnemonic(KeyEvent.VK_M);**

    **b3 = new JButton("Enable middle button"**, rightButtonIcon**);**
    //Use the default text position of CENTER, TRAILING (RIGHT).
    <b>b3.setMnemonic(KeyEvent.VK_E);
    b3.setActionCommand("enable");
    b3.setEnabled(false);</b>

    //Listen for actions on buttons 1 and 3.
    <b>b1.addActionListener(this);
    b3.addActionListener(this);

    b1.setToolTipText("Click this button to disable "
                      + "the middle button.");
    b2.setToolTipText("This middle button does nothing "
                      + "when you click it.");
    b3.setToolTipText("Click this button to enable the "
                      + "middle button.");</b>
    ...
}

<b>public void actionPerformed(ActionEvent e) {
    if ("disable".equals(e.getActionCommand())) {
        b2.setEnabled(false);
        b1.setEnabled(false);
        b3.setEnabled(true);
    } else {
        b2.setEnabled(true);
        b1.setEnabled(true);
        b3.setEnabled(false);
    }
} </b>

protected static ImageIcon createImageIcon(String path) {
    java.net.URL imgURL = ButtonDemo.class.getResource(path);
    **...//error handling omitted for clarity...**
    return new ImageIcon(imgURL);
}

```

## <a name="jbutton" id="jbutton">How to Use JButton Features</a>

Ordinary buttons &#151; `JButton` objects &#151; have just a bit more functionality than the `AbstractButton` class provides: You can make a `JButton` be the default button.

At most one button in a top-level container can be the default button. The default button typically has a highlighted appearance and acts clicked whenever the top-level container has the keyboard focus and the user presses the Return or Enter key. Here is a picture of a dialog, implemented in the [ListDialog](../examples/components/index.html#ListDialog) example, in which the **Set** button is the default button:

You set the default button by invoking the `setDefaultButton` method on a top-level container's root pane. Here is the code that sets up the default button for the `ListDialog` example:

```

**//In the constructor for a JDialog subclass:**
getRootPane().setDefaultButton(setButton);

```

The exact implementation of the default button feature depends on the look and feel. For example, in the Windows look and feel, the default button changes to whichever button has the focus, so that pressing Enter clicks the focused button. When no button has the focus, the button you originally specified as the default button becomes the default button again.

## <a name="checkbox" id="checkbox">How to Use Check Boxes</a>

The 
[`JCheckBox`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html) class provides support for check box buttons. You can also put check boxes in [menus](menu.html), using the 
[`JCheckBoxMenuItem`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html) class. Because `JCheckBox` and `JCheckBoxMenuItem` inherit from `AbstractButton`, Swing check boxes have all the usual button characteristics, as discussed earlier in this section. For example, you can specify images to be used in check boxes.

Check boxes are similar to [radio buttons](#radiobutton) but their selection model is different, by convention. Any number of check boxes in a group &#151; none, some, or all &#151; can be selected. A group of radio buttons, on the other hand, can have only one button selected.

Here is a picture of an application that uses four check boxes to customize a cartoon:

<li>Click the Launch button to run the CheckBox Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#CheckBoxDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ButtonDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/CheckBoxDemoProject/CheckBoxDemo.jnlp)<br />
<p><!--  ******* end boilerplate stuff  *******  -->
 <!-- 

<li> [Run CheckBoxDemo](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/CheckBoxDemoProject/CheckBoxDemo.jnlp) (
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).
     Or, to compile and run the example yourself,
     consult the 
     [example index](../examples/components/index.html#CheckBoxDemo).
--></p>
</li>
<li>Click the **Chin** button or press Alt-c.<br />
The **Chin** check box becomes unselected, and the chin disappears from the picture. The other check boxes remain selected. This application has one item listener that listens to all the check boxes. Each time the item listener receives an event, the application loads a new picture that reflects the current state of the check boxes.</li>

A check box generates one item event and one action event per click. Usually, you listen only for item events, since they let you determine whether the click selected or deselected the check box. Below is the code from 
[`CheckBoxDemo.java`](../examples/components/CheckBoxDemoProject/src/components/CheckBoxDemo.java) that creates the check boxes in the previous example and reacts to clicks.

```

**//In initialization code:**
    chinButton = new JCheckBox("Chin");
    chinButton.setMnemonic(KeyEvent.VK_C); 
    chinButton.setSelected(true);

    glassesButton = new JCheckBox("Glasses");
    glassesButton.setMnemonic(KeyEvent.VK_G); 
    glassesButton.setSelected(true);

    hairButton = new JCheckBox("Hair");
    hairButton.setMnemonic(KeyEvent.VK_H); 
    hairButton.setSelected(true);

    teethButton = new JCheckBox("Teeth");
    teethButton.setMnemonic(KeyEvent.VK_T); 
    teethButton.setSelected(true);

    //Register a listener for the check boxes.
    chinButton.addItemListener(this);
    glassesButton.addItemListener(this);
    hairButton.addItemListener(this);
    teethButton.addItemListener(this);
...
public void itemStateChanged(ItemEvent e) {
    ...
    Object source = e.getItemSelectable();

    if (source == chinButton) {
        **//...make a note of it...**
    } else if (source == glassesButton) {
        **//...make a note of it...**
    } else if (source == hairButton) {
        **//...make a note of it...**
    } else if (source == teethButton) {
        **//...make a note of it...**
    }

    if (e.getStateChange() == ItemEvent.DESELECTED)
        **//...make a note of it...**
    ...
    updatePicture();
}

```

## <a name="radiobutton" id="radiobutton">How to Use Radio Buttons</a>

Radio buttons are groups of buttons in which, by convention, only one button at a time can be selected. The Swing release supports radio buttons with the 
[`JRadioButton`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html) and 
[`ButtonGroup`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html) classes. To put a radio button in a [menu](menu.html), use the 
[`JRadioButtonMenuItem`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html) class. Other ways of displaying one-of-many choices are [combo boxes](combobox.html) and [lists](list.html). Radio buttons look similar to [check boxes](#checkbox), but, by convention, check boxes place no limits on how many items can be selected at a time.

Because `JRadioButton` inherits from `AbstractButton`, Swing radio buttons have all the usual button characteristics, as discussed earlier in this section. For example, you can specify the image displayed in a radio button.

Here is a picture of an application that uses five radio buttons to let you choose which kind of pet is displayed:
<!-- *******  boilerplate stuff ******* -->
<li>Click the Launch button to run the RadioButton Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#RadioButtonDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ButtonDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/RadioButtonDemoProject/RadioButtonDemo.jnlp)<br />
<p><!--  ******* end boilerplate stuff  *******  -->
 <!-- 

<li> [Run RadioButtonDemo](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/RadioButtonDemoProject/RadioButtonDemo.jnlp) (
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).
     Or, to compile and run the example yourself,
     consult the 
     [example index](../examples/components/index.html#RadioButtonDemo).
        
--></p>
</li>
<li>Click the **Dog** button or press Alt-d.<br />
The **Dog** button becomes selected, which makes the **Bird** button become unselected. The picture switches from a bird to a dog. This application has one action listener that listens to all the radio buttons. Each time the action listener receives an event, the application displays the picture for the radio button that was just clicked.</li>

Each time the user clicks a radio button (even if it was already selected), the button fires an 
[action event](../events/actionlistener.html). One or two 
[item events](../events/itemlistener.html) also occur &#151; one from the button that was just selected, and another from the button that lost the selection (if any). Usually, you handle radio button clicks using an action listener.

Below is the code from 
[`RadioButtonDemo.java`](../examples/components/RadioButtonDemoProject/src/components/RadioButtonDemo.java) that creates the radio buttons in the previous example and reacts to clicks.

```

**//In initialization code:**
    //Create the radio buttons.
    JRadioButton birdButton = new JRadioButton(birdString);
    birdButton.setMnemonic(KeyEvent.VK_B);
    birdButton.setActionCommand(birdString);
    birdButton.setSelected(true);

    JRadioButton catButton = new JRadioButton(catString);
    catButton.setMnemonic(KeyEvent.VK_C);
    catButton.setActionCommand(catString);

    JRadioButton dogButton = new JRadioButton(dogString);
    dogButton.setMnemonic(KeyEvent.VK_D);
    dogButton.setActionCommand(dogString);

    JRadioButton rabbitButton = new JRadioButton(rabbitString);
    rabbitButton.setMnemonic(KeyEvent.VK_R);
    rabbitButton.setActionCommand(rabbitString);

    JRadioButton pigButton = new JRadioButton(pigString);
    pigButton.setMnemonic(KeyEvent.VK_P);
    pigButton.setActionCommand(pigString);

    //Group the radio buttons.
    ButtonGroup group = new ButtonGroup();
    group.add(birdButton);
    group.add(catButton);
    group.add(dogButton);
    group.add(rabbitButton);
    group.add(pigButton);

    //Register a listener for the radio buttons.
    birdButton.addActionListener(this);
    catButton.addActionListener(this);
    dogButton.addActionListener(this);
    rabbitButton.addActionListener(this);
    pigButton.addActionListener(this);
...
public void actionPerformed(ActionEvent e) {
    picture.setIcon(new ImageIcon("images/" 
                                  + e.getActionCommand() 
                                  + ".gif"));
}

```

For each group of radio buttons, you need to create a `ButtonGroup` instance and add each radio button to it. The `ButtonGroup` takes care of unselecting the previously selected button when the user selects another button in the group.

You should generally initialize a group of radio buttons so that one is selected. However, the API doesn't enforce this rule &#151; a group of radio buttons can have no initial selection. Once the user has made a selection, exactly one button is selected from then on.

## <a name="api" id="api">The Button API</a>

The following tables list the commonly used button-related API. Other methods you might call, such as `setFont` and `setForeground`, are listed in the API tables in [The JComponent Class](jcomponent.html#api).

The API for using buttons falls into these categories:

- [Setting or Getting the Button's Contents](#contents)
- [Fine Tuning the Button's Appearance](#looks)
- [Implementing the Button's Functionality](#acts)
- [Check Box Constructors](#checkboxapi)
- [Radio Button Constructors](#radiobuttonapi)
- [Toggle Button Constructors](#togglebuttonapi)
- [Commonly Used Button Group Constructors/Methods](#buttongroup)
<th id="h101">Method or Constructor</th><th id="h102">Purpose</th>
<td headers="h101">[JButton(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JButton.html#JButton-javax.swing.Action-)<br />[JButton(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JButton.html#JButton-java.lang.String-javax.swing.Icon-)<br />[JButton(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JButton.html#JButton-java.lang.String-)<br />[JButton(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JButton.html#JButton-javax.swing.Icon-)<br />[JButton()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JButton.html#JButton--)</td><td headers="h102">Create a `JButton` instance, initializing it to have the specified text/image/action.</td>
<td headers="h101">[void setAction(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setAction-javax.swing.Action-)<br />[Action getAction()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getAction--)<br /></td><td headers="h102">Set or get the button's properties according to values from the `Action` instance.</td>
<td headers="h101">[void setText(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setText-java.lang.String-)<br />[String getText()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getText--)<br /></td><td headers="h102">Set or get the text displayed by the button. You can use HTML formatting, as described in [Using HTML in Swing Components](html.html).</td>
<td headers="h101">[void setIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setIcon-javax.swing.Icon-)<br />[Icon getIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getIcon--)<br /></td><td headers="h102">Set or get the image displayed by the button when the button isn't selected or pressed.</td>
<td headers="h101">[void setDisabledIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setDisabledIcon-javax.swing.Icon-)<br />[Icon getDisabledIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getDisabledIcon--)<br /></td><td headers="h102">Set or get the image displayed by the button when it is disabled. If you do not specify a disabled image, then the look and feel creates one by manipulating the default image.</td>
<td headers="h101">[void setPressedIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setPressedIcon-javax.swing.Icon-)<br />[Icon getPressedIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getPressedIcon--)<br /></td><td headers="h102">Set or get the image displayed by the button when it is being pressed.</td>
<td headers="h101">[void setSelectedIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setSelectedIcon-javax.swing.Icon-)<br />[Icon getSelectedIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getSelectedIcon--)<br />[void setDisabledSelectedIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setDisabledSelectedIcon-javax.swing.Icon-)<br />[Icon getDisabledSelectedIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getDisabledSelectedIcon--)<br /></td><td headers="h102">Set or get the image displayed by the button when it is selected. If you do not specify a disabled selected image, then the look and feel creates one by manipulating the selected image.</td>
<td headers="h101">[setRolloverEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setRolloverEnabled-boolean-)<br />[boolean isRolloverEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#isRolloverEnabled--)<br />[void setRolloverIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setRolloverIcon-javax.swing.Icon-)<br />[Icon getRolloverIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getRolloverIcon--)<br />[void setRolloverSelectedIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setRolloverSelectedIcon-javax.swing.Icon-)<br />[Icon getRolloverSelectedIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getRolloverSelectedIcon--)<br /></td><td headers="h102">Use `setRolloverIcon(someIcon)` to make the button display the specified icon when the cursor passes over it. The `setRolloverSelectedIcon` method lets you specify the rollover icon when the button is selected &#151; this is useful for two-state buttons such as toggle buttons. Setting the rollover icon automatically calls `setRollover(true)`, enabling rollover.</td>
<th id="h201">Method or Constructor</th><th id="h202">Purpose</th>
<td headers="h201">[void setHorizontalAlignment(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setHorizontalAlignment-int-)<br />[void setVerticalAlignment(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setVerticalAlignment-int-)<br />[int getHorizontalAlignment()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getHorizontalAlignment--)<br />[int getVerticalAlignment()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getVerticalAlignment--)</td><td headers="h202">Set or get where in the button its contents should be placed. The `AbstractButton` class allows any one of the following values for horizontal alignment: `RIGHT`, `LEFT`, `CENTER` (the default), `LEADING`, and `TRAILING`. For vertical alignment: `TOP`, `CENTER` (the default), and `BOTTOM`.</td>
<td headers="h201">[void setHorizontalTextPosition(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setHorizontalTextPosition-int-)<br />[void setVerticalTextPosition(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setVerticalTextPosition-int-)<br />[int getHorizontalTextPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getHorizontalTextPosition--)<br />[int getVerticalTextPosition()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getVerticalTextPosition--)</td><td headers="h202">Set or get where the button's text should be placed, relative to the button's image. The `AbstractButton` class allows any one of the following values for horizontal position: `LEFT`, `CENTER`, `RIGHT`, `LEADING`, and `TRAILING` (the default). For vertical position: `TOP`, `CENTER` (the default), and `BOTTOM`.</td>
<td headers="h201">[void setMargin(Insets)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setMargin-java.awt.Insets-)<br />[Insets getMargin()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getMargin--)</td><td headers="h202">Set or get the number of pixels between the button's border and its contents.</td>
<td headers="h201">[void setFocusPainted(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setFocusPainted-boolean-)<br />[boolean isFocusPainted()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#isFocusPainted--)</td><td headers="h202">Set or get whether the button should look different when it has the focus.</td>
<td headers="h201">[void setBorderPainted(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setBorderPainted-boolean-)<br />[boolean isBorderPainted()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#isBorderPainted--)</td><td headers="h202">Set or get whether the border of the button should be painted.</td>
<td headers="h201">[void setIconTextGap(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setIconTextGap-int-)<br />[int getIconTextGap()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getIconTextGap--)<br /></td><td headers="h202">Set or get the amount of space between the text and the icon displayed in this button.</td>
<th id="h301">Method or Constructor</th><th id="h302">Purpose</th>
<td headers="h301">[void setMnemonic(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setMnemonic-int-)<br />[char getMnemonic()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getMnemonic--)<br /></td><td headers="h302">Set or get the keyboard alternative to clicking the button. One form of the `setMnemonic` method accepts a character argument; however, the Swing team recommends that you use an `int` argument instead, specifying a `KeyEvent.VK_**X**` constant.</td>
<td headers="h301">[void setDisplayedMnemonicIndex(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setDisplayedMnemonicIndex-int-)<br />[int getDisplayedMnemonicIndex()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getDisplayedMnemonicIndex--)<br /></td><td headers="h302">Set or get a hint as to which character in the text should be decorated to represent the mnemonic. Note that not all look and feels may support this.</td>
<td headers="h301">[void setActionCommand(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setActionCommand-java.lang.String-)<br />[String getActionCommand()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getActionCommand--)<br /></td><td headers="h302">Set or get the name of the action performed by the button.</td>
<td headers="h301">[void addActionListener(ActionListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#addActionListener-java.awt.event.ActionListener-)<br />[ActionListener removeActionListener()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#removeActionListener--)<br /></td><td headers="h302">Add or remove an object that listens for action events fired by the button.</td>
<td headers="h301">[void addItemListener(ItemListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#addItemListener-java.awt.event.ItemListener-)<br />[ItemListener removeItemListener()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#removeItemListener--)<br /></td><td headers="h302">Add or remove an object that listens for item events fired by the button.</td>
<td headers="h301">[void setSelected(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setSelected-boolean-)<br />[boolean isSelected()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#isSelected--)<br /></td><td headers="h302">Set or get whether the button is selected. Makes sense only for buttons that have on/off state, such as check boxes. </td>
<td headers="h301">[void doClick()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#doClick--)<br />[void doClick(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#doClick-int-)<br /></td><td headers="h302">Programmatically perform a "click". The optional argument specifies the amount of time (in milliseconds) that the button should look pressed.</td>
<td headers="h301">[void setMultiClickThreshhold(long)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setMultiClickThreshhold-long-)<br />[long getMultiClickThreshhold()](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#getMultiClickThreshhold--)</td><td headers="h302">Set or get the amount of time (in milliseconds) required between mouse press events for the button to generate corresponding action events.</td>
<th id="h401">Constructor</th><th id="h402">Purpose</th>
<td headers="h401">[JCheckBox(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html#JCheckBox-javax.swing.Action-)<br />[JCheckBox(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html#JCheckBox-java.lang.String-)<br />[JCheckBox(String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html#JCheckBox-java.lang.String-boolean-)<br />[JCheckBox(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html#JCheckBox-javax.swing.Icon-)<br />[JCheckBox(Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html#JCheckBox-javax.swing.Icon-boolean-)<br />[JCheckBox(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html#JCheckBox-java.lang.String-javax.swing.Icon-)<br />[JCheckBox(String, Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html#JCheckBox-java.lang.String-javax.swing.Icon-boolean-)<br />[JCheckBox()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBox.html#JCheckBox--)</td><td headers="h402">Create a `JCheckBox` instance. The string argument specifies the text, if any, that the check box should display. Similarly, the `Icon` argument specifies the image that should be used instead of the look and feel's default check box image. Specifying the boolean argument as `true` initializes the check box to be selected. If the boolean argument is absent or `false`, then the check box is initially unselected.</td>
<td headers="h401">[JCheckBoxMenuItem(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-javax.swing.Action-)<br />[JCheckBoxMenuItem(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-java.lang.String-)<br />[JCheckBoxMenuItem(String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-java.lang.String-boolean-)<br />[JCheckBoxMenuItem(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-javax.swing.Icon-)<br />[JCheckBoxMenuItem(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-java.lang.String-javax.swing.Icon-)<br />[JCheckBoxMenuItem(String, Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-java.lang.String-javax.swing.Icon-boolean-)<br />[JCheckBoxMenuItem()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem--)</td><td headers="h402">Create a `JCheckBoxMenuItem` instance. The arguments are interpreted in the same way as the arguments to the `JCheckBox` constructors, except that any specified icon is shown in addition to the normal check box icon.</td>
<th id="h501">Constructor</th><th id="h502">Purpose</th>
<td headers="h501">[JRadioButton(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html#JRadioButton-javax.swing.Action-)<br />[JRadioButton(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html#JRadioButton-java.lang.String-)<br />[JRadioButton(String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html#JRadioButton-java.lang.String-boolean-)<br />[JRadioButton(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html#JRadioButton-javax.swing.Icon-)<br />[JRadioButton(Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html#JRadioButton-javax.swing.Icon-boolean-)<br />[JRadioButton(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html#JRadioButton-java.lang.String-javax.swing.Icon-)<br />[JRadioButton(String, Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html#JRadioButton-java.lang.String-javax.swing.Icon-boolean-)<br />[JRadioButton()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButton.html#JRadioButton--)</td><td headers="h502">Create a `JRadioButton` instance. The string argument specifies the text, if any, that the radio button should display. Similarly, the `Icon` argument specifies the image that should be used instead of the look and feel's default radio button image. Specifying the boolean argument as `true` initializes the radio button to be selected, subject to the approval of the `ButtonGroup` object. If the boolean argument is absent or `false`, then the radio button is initially unselected.</td>
<td headers="h501">[JRadioButtonMenuItem(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-javax.swing.Action-)<br />[JRadioButtonMenuItem(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-java.lang.String-)<br />[JRadioButtonMenuItem(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-javax.swing.Icon-)<br />[JRadioButtonMenuItem(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-java.lang.String-javax.swing.Icon-)<br />[JRadioButtonMenuItem()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem--)</td><td headers="h502">Create a `JRadioButtonMenuItem` instance. The arguments are interpreted in the same way as the arguments to the `JRadioButton` constructors, except that any specified icon is shown in addition to the normal radio button icon.</td>
<th id="h601">Constructor</th><th id="h602">Purpose</th>
<td headers="h601">[JToggleButton(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToggleButton.html#JToggleButton-javax.swing.Action-)<br />[JToggleButton(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToggleButton.html#JToggleButton-java.lang.String-)<br />[JToggleButton(String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToggleButton.html#JToggleButton-java.lang.String-boolean-)<br />[JToggleButton(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToggleButton.html#JToggleButton-javax.swing.Icon-)<br />[JToggleButton(Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToggleButton.html#JToggleButton-javax.swing.Icon-boolean-)<br />[JToggleButton(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToggleButton.html#JToggleButton-java.lang.String-javax.swing.Icon-)<br />[JToggleButton(String, Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToggleButton.html#JToggleButton-java.lang.String-javax.swing.Icon-boolean-)<br />[JToggleButton()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JToggleButton.html#JToggleButton--)</td><td headers="h602">Create a `JToggleButton` instance, which is similar to a `JButton`, but with two states. Normally, you use a `JRadioButton` or `JCheckBox` instead of directly instantiating `JToggleButton`, but `JToggleButton` can be useful when you do not want the typical radio button or check box appearance. The string argument specifies the text, if any, that the toggle button should display. Similarly, the `Icon` argument specifies the image that should be used. Specifying the boolean argument as `true` initializes the toggle button to be selected. If the boolean argument is absent or `false`, then the toggle button is initially unselected.</td>
<th id="h701">Constructor or Method</th><th id="h702">Purpose</th>
<td headers="h701">[ButtonGroup()](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#ButtonGroup--)</td><td headers="h702">Create a `ButtonGroup` instance.</td>
<td headers="h701">[void add(AbstractButton)](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#add-javax.swing.AbstractButton-)<br />[void remove(AbstractButton)](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#remove-javax.swing.AbstractButton-)</td><td headers="h702">Add a button to the group, or remove a button from the group. </td>
<td headers="h701">[public ButtonGroup getGroup()](https://docs.oracle.com/javase/8/docs/api/javax/swing/DefaultButtonModel.html#getGroup--)<br />**(in `DefaultButtonModel`)**</td><td headers="h702">Get the `ButtonGroup`, if any, that controls a button. For example:<br />`ButtonGroup group = ((DefaultButtonModel)button.getModel()).getGroup();`</td>
<td headers="h701">[public ButtonGroup clearSelection()](https://docs.oracle.com/javase/8/docs/api/javax/swing/ButtonGroup.html#ButtonGroup--)</td><td headers="h702">Clears the state of selected buttons in the ButtonGroup. None of the buttons in the ButtonGroup are selected .</td>

## <a name="eg" id="eg">Examples that Use Various Kinds of Buttons</a>

The following examples use buttons. Also see [Examples that Use Tool Bars](toolbar.html#eg), which lists programs that add `JButton` objects to `JToolBar`s.
<th id="h801" align="left">Example</th><th id="h802" align="left">Where Described</th><th id="h803" align="left">Notes</th>
<td headers="h801">[`ButtonDemo`](../examples/components/index.html#ButtonDemo)</td><td headers="h802">[How to Use the Common Button API](#abstractbutton)</td><td headers="h803">Uses mnemonics and icons. Specifies the button text position, relative to the button icon. Uses action commands.</td>
<td headers="h801">[`ButtonHtmlDemo`](../examples/components/index.html#ButtonHtmlDemo)</td><td headers="h802">[Using HTML in Swing Components](html.html)</td><td headers="h803">A version of ButtonDemo that uses HTML formatting in its buttons.</td>
<td headers="h801">[`ListDialog`](../examples/components/index.html#ListDialog)</td><td headers="h802">[How to Use JButton Features](#jbutton)</td><td headers="h803">Implements a dialog with two buttons, one of which is the default button.</td>
<td headers="h801">[`DialogDemo`](../examples/components/index.html#DialogDemo)</td><td headers="h802">[How to Make Dialogs](dialog.html)</td><td headers="h803">Has "Show it" buttons whose behavior is tied to the state of radio buttons. Uses sizable, though anonymous, inner classes to implement the action listeners.</td>
<td headers="h801">[`ProgressBarDemo`](../examples/components/index.html#ProgressBarDemo)</td><td headers="h802">[How to Monitor Progress](progress.html)</td><td headers="h803">Implements a button's action listener with a named inner class.</td>
<td headers="h801">[`CheckBoxDemo`](../examples/components/index.html#CheckBoxDemo)</td><td headers="h802">[How to Use Check Boxes](#checkbox)</td><td headers="h803">Uses check box buttons to determine which of 16 images it should display.</td>
<td headers="h801">[`ActionDemo`](../examples/misc/index.html#ActionDemo)</td><td headers="h802">[How to Use Actions](../misc/action.html)</td><td headers="h803">Uses check box menu items to set the state of the program.</td>
<td headers="h801">[`RadioButtonDemo`](../examples/components/index.html#RadioButtonDemo)</td><td headers="h802">[How to Use Radio Buttons](#radiobutton)</td><td headers="h803">Uses radio buttons to determine which of five images it should display.</td>
<td headers="h801">[`DialogDemo`](../examples/components/index.html#DialogDemo)</td><td headers="h802">[How to Make Dialogs](dialog.html)</td><td headers="h803">Contains several sets of radio buttons, which it uses to determine which dialog to bring up.</td>
<td headers="h801">[`MenuDemo`](../examples/components/index.html#MenuDemo)</td><td headers="h802">[How to Use Menus](menu.html)</td><td headers="h803">Contains radio button menu items and check box menu items.</td>
<td headers="h801">[`ColorChooserDemo2`](../examples/components/index.html#ColorChooserDemo2)</td><td headers="h802">[How to Use Color Choosers](colorchooser.html)</td><td headers="h803">The crayons in `CrayonPanel` are implemented as toggle buttons.</td>
<td headers="h801">[`ScrollDemo`](../examples/components/index.html#ScrollDemo)</td><td headers="h802">[How to Use Scroll Panes](scrollpane.html)</td><td headers="h803">The **cm** button is a toggle button.</td>


You can learn more about JavaFX button components from the following documents:

<li>
[Using JavaFX UI Controls: Button](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/button.htm)</li>
<li>
[Using JavaFX UI Controls: Radio Button](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/radio-button.htm)</li>
<li>
[Using JavaFX UI Controls: Toggle Button](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/toggle-button.htm)</li>
<li>
[Using JavaFX UI Controls: Checkbox](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/checkbox.htm)</li>
