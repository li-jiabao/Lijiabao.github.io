
# Creating the CelsiusConverter GUI

This section explains how to use the NetBeans IDE to create the application's GUI. As you drag each component from the Palette to the Design Area, the IDE auto-generates the appropriate source code.

<a name="a7a" id="a7a"></a>

## Step 1: Set the Title

First, set the title of the application's `JFrame` to "Celsius Converter", by single-clicking the `JFrame` in the Inspector:

Selecting the JFrame

Then, set its title with the Property Editor:

Setting the Title

You can set the title by either double-clicking the title property and entering the new text directly, or by clicking the <img alt="Ellipsis button" src="../../figures/uiswing/learn/nb-swing-11b.png" /> button and entering the title in the provided field. Or, as a shortcut, you could single-click the `JFrame` of the inspector and enter its new text directly without using the property editor. <a name="a14" id="a14"></a>

## Step 2: Add a JTextField

Next, drag a `JTextField` from the Palette to the upper left corner of the Design Area. As you approach the upper left corner, the GUI builder provides visual cues (dashed lines) that suggest the appropriate spacing. Using these cues as a guide, drop a `JTextField` into the upper left hand corner of the window as shown below:

Adding a JTextField

You may be tempted to erase the default text "JTextField1", but just leave it in place for now. We will replace it later in this lesson as we make the final adjustments to each component. For more information about this component, see 
[How to Use Text Fields](../components/textfield.html). <a name="a15" id="a15"></a>

## Step 3: Add a JLabel

Next, drag a `JLabel` onto the Design Area. Place it to the right of the `JTextField`, again watching for visual cues that suggest an appropriate amount of spacing. Make sure that text base for this component is aligned with that of the `JTextField`. The visual cues provided by the IDE should make this easy to determine.

Adding a JLabel

For more information about this component, see 
[How to Use Labels](../components/label.html). <a name="a16" id="a16"></a>

## Step 4: Add a JButton

Next, drag a `JButton` from the Palette and position it to the left and underneath the `JTextField`. Again, the visual cues help guide it into place.

Adding a JButton

You may be tempted to manually adjust the width of the `JButton` and `JTextField`, but just leave them as they are for now. You will learn how to correctly adjust these components later in this lesson. For more information about this component, see 
[How to Use Buttons](../components/button.html). <a name="a17" id="a17"></a>

## Step 5: Add a Second JLabel

Adding a Second JLabel

Finally, add a second `JLabel`, repeating the process in step 2. Place this second label to the right of the `JButton`, as shown above.
