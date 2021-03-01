
# How to Use Tabbed Panes

With the 
[`JTabbedPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html) class, you can have several components, such as [panels](panel.html), share the same space. The user chooses which component to view by selecting the tab corresponding to the desired component. If you want similar functionality without the tab interface, you can use a 
[card layout](../layout/card.html) instead of a tabbed pane.

## To Create Tabbed Panes

To create a tabbed pane, instantiate `JTabbedPane`, create the components you wish it to display, and then add the components to the tabbed pane using the `addTab` method.

The following picture introduces an application called `TabbedPaneDemo` that has a tabbed pane with four tabs.

<li>Click the Launch button to run TabbedPaneDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#TabbedPaneDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the TabbedPaneDemo Application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TabbedPaneDemoProject/TabbedPaneDemo.jnlp)<br /></li>
<li>Put the cursor over a tab.<br />
The tool tip associated with the tab appears. As a convenience, you can specify tool tip text when you add a component to the tabbed pane.</li>
<li>Select a tab by clicking it.<br />
The tabbed pane displays the component corresponding to the tab.</li>
<li>Select a tab by entering its mnemonic.<br />
For example, in the Java look and feel you can select the tab labeled "Tab 3" by typing Alt-3.</li>
<li>Navigate between scrollable tabs.<br />
This example provides scrollable tabs. Resize the dialog box by moving its left or right boundary so that tabs do not fit within the dialog. Scroll arrows appear next to the tabs.<br />
Click the arrow to view one of the hidden tabs.<br />
Note that clicking the arrow only reveals hidden tabs. It does not select a new tab.</li>

As the `TabbedPaneDemo` example shows, a tab can have a tool tip and a mnemonic, and it can display both text and an image.

### Tab Placement

The default tab placement is set to the `TOP` location, as shown above. You can change the tab placement to `LEFT`, `RIGHT`, `TOP` or `BOTTOM` by using the `setTabPlacement` method.

### Code for Tabbed Panes

The following code from 
[`TabbedPaneDemo.java`](../examples/components/TabbedPaneDemoProject/src/components/TabbedPaneDemo.java) creates the tabbed pane in the previous example. Note that no event-handling code is necessary. The `JTabbedPane` object takes care of mouse and keyboard events for you.

```

JTabbedPane tabbedPane = new JTabbedPane();
ImageIcon icon = createImageIcon("images/middle.gif");

JComponent panel1 = makeTextPanel("Panel #1");
<b>tabbedPane.addTab("Tab 1", icon, panel1,
                  "Does nothing");</b>
tabbedPane.setMnemonicAt(0, KeyEvent.VK_1);

JComponent panel2 = makeTextPanel("Panel #2");
<b>tabbedPane.addTab("Tab 2", icon, panel2,
                  "Does twice as much nothing");</b>
tabbedPane.setMnemonicAt(1, KeyEvent.VK_2);

JComponent panel3 = makeTextPanel("Panel #3");
<b>tabbedPane.addTab("Tab 3", icon, panel3,
                  "Still does nothing");</b>
tabbedPane.setMnemonicAt(2, KeyEvent.VK_3);

JComponent panel4 = makeTextPanel(
        "Panel #4 (has a preferred size of 410 x 50).");
panel4.setPreferredSize(new Dimension(410, 50));
<b>tabbedPane.addTab("Tab 4", icon, panel4,
                      "Does nothing at all");</b>
tabbedPane.setMnemonicAt(3, KeyEvent.VK_4);

```

As the previous code shows, the `addTab` method handles the bulk of the work in setting up a tab in a tabbed pane. The `addTab` method has several forms, but they all use both a string title and the component to be displayed by the tab. Optionally, you can specify an icon and [tool tip](tooltip.html) string. The text or icon (or both) can be null. Another way to create a tab is to use the `insertTab` method, which lets you specify the index of the tab you're adding. Note that the `addTab` method does not allow index specification in this step.

## To Switch to Specific Tabs

There are three ways to switch to specific tabs using GUI.

1. **Using a mouse.** To switch to a specific tab, the user clicks it with the mouse.
1. **Using keyboard arrows.** When the `JTabbedPane` object has the focus, the keyboard arrows can be used to switch from tab to tab.
1. **Using key mnemonics.** The `setMnemonicAt` method allows the user to switch to a specific tab using the keyboard. For example, `setMnemonicAt(3, KeyEvent.VK_4)` makes '4' the mnemonic for the fourth tab (which is at index 3, since the indices start with 0); pressing Alt-4 makes the fourth tab's component appear. Often, a mnemonic uses a character in the tab's title that is then automatically underlined.

To switch to a specific tab programmatically, use the 
[`setSelectedIndex`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setSelectedIndex-int-) or the 
[`setSelectedComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setSelectedComponent-java.awt.Component-) methods.

### Preferred Size in Tabs

When building components to add to a tabbed pane, keep in mind that no matter which child of a tabbed pane is visible, each child gets the same amount of space in which to display itself. The preferred size of the tabbed pane is just big enough to display its tallest child at its preferred height, and its widest child at its preferred width. Similarly, the minimum size of the tabbed pane depends on the biggest minimum width and height of all its children.

In the `TabbedPaneDemo` example, the fourth panel has a preferred width and height that are larger than those of the other panels. Thus, the preferred size of the tabbed pane is just big enough to display the fourth panel at its preferred size. Every panel gets exactly the same amount of space &#151; 410 pixels wide and 50 high, assuming the tabbed pane is at its preferred size. If you do not understand how preferred size is used, please refer to 
[How Layout Management Works](../layout/howLayoutWorks.html).

### Tabs With Custom Components

The `TabComponentsDemo` example introduces a tabbed pane whose tabs contain real components. The use of custom components brings new features such as buttons, combo boxes, labels and other components to tabs, and allows more complex user interaction.

Here is a tabbed pane with close buttons on its tabs.

<li>Click the Launch button to run TabComponentsDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#TabComponentsDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the TabComponentsDemo Application" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/TabComponentsDemoProject/TabComponentsDemo.jnlp)<br /></li>
1. Put the cursor over a tab.
1. Select a tab by clicking it (make sure not to hit the little cross).
<li>Put the cursor over one of the widgets with a little cross.<br />
The cross turns magenta and gets enclosed in a square. A tool tip associated with the close button appears.<br />
Click the cross with the left mouse button to close the tab.</li>
1. Restore the tabs that have been removed by choosing the Reset JTabbedPane item from the Options menu.
<li>Note that tabs with custom components are displayed on top of original tabbed pane tabs.<br />
To view the tabs underneath, open the Options menu and clear the Use TabComponents checkbox.</li>
1. Display the tabs with components by selecting the Use TabComponents checkbox again.
1. Close all tabs. Now the tabbed pane is empty.

## To Remove Tabs

The following code from 
[`ButtonTabComponent.java`](../examples/components/TabComponentsDemoProject/src/components/ButtonTabComponent.java) removes a tab from the tabbed pane. Note that event-handling code is necessary. Since each tab contains a real `JButton` object, you must attach an `ActionListener` to the close button. As the user clicks the button, the `actionPerformed` method determines the index of the tab it belongs to and removes the corresponding tab.

```

public void actionPerformed(ActionEvent e) {
    **int i = pane.indexOfTabComponent(ButtonTabComponent.this);**
    if (i != -1) {
    pane.remove(i);
    }
}

```

## To Give Titles to Customized Tabs

The code below, taken from 
[`ButtonTabComponent.java`](../examples/components/TabComponentsDemoProject/src/components/ButtonTabComponent.java), shows how a customized tab component gets a title from an original tabbed pane tab.

```

JLabel label = new JLabel(title) {
    public String getText() {
        int i = pane.indexOfTabComponent(ButtonTabComponent.this);
        if (i != -1) {
            return pane.getTitleAt(i);
        }
        return null;
    }
};

```

## <a name="api" id="api">The Tabbed Pane API</a>

The following tables list the commonly used `JTabbedPane` constructors and methods. The API for using tabbed panes falls into the following categories:

- [Creating and Setting Up a Tabbed Pane](#creating)
- [Inserting, Removing, Finding, and Selecting Tabs](#tabapi)
- [Changing Tab Appearance](#appearanceapi)
- [Setting Up Custom Components on Tabs&lt;/&gt;](#tabscomponents)

[](#tabscomponents)
<th id="h1" align="left">Method or Constructor</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[JTabbedPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#JTabbedPane--)<br />[JTabbedPane(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#JTabbedPane-int-)<br />[JTabbedPane(int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#JTabbedPane-int-int-)</td><td headers="h2">Creates a tabbed pane. The first optional argument specifies where the tabs should appear. By default, the tabs appear at the top of the tabbed pane. You can specify these positions (defined in the `SwingConstants` interface, which `JTabbedPane` implements): `TOP`, `BOTTOM`, `LEFT`, `RIGHT`. The second optional argument specifies the tab layout policy. You can specify one of these policies (defined in `JTabbedPane`): [`WRAP_TAB_LAYOUT`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#WRAP_TAB_LAYOUT) or [`SCROLL_TAB_LAYOUT`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#SCROLL_TAB_LAYOUT).</td>
<td headers="h1">[addTab(String, Icon, Component, String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#addTab-java.lang.String-javax.swing.Icon-java.awt.Component-java.lang.String-)<br />[addTab(String, Icon, Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#addTab-java.lang.String-javax.swing.Icon-java.awt.Component-)<br />[addTab(String, Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#addTab-java.lang.String-java.awt.Component-)</td><td headers="h2">Adds a new tab to the tabbed pane. The first argument specifies the text on the tab. The optional icon argument specifies the tab's icon. The component argument specifies the component that the tabbed pane should show when the tab is selected. The fourth argument, if present, specifies the tool tip text for the tab.</td>
<td headers="h1">[void setTabLayoutPolicy(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setTabLayoutPolicy-int-)<br />[int getTabLayoutPolicy()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getTabLayoutPolicy--)</td><td headers="h2">Sets or gets the policy that the tabbed pane uses in laying out tabs when all tabs do not fit within a single run. Possible values are `WRAP_TAB_LAYOUT` and `SCROLL_TAB_LAYOUT`. The default policy is `WRAP_TAB_LAYOUT`.</td>
<td headers="h1">[void setTabPlacement(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setTabPlacement-int-)<br />[int getTabPlacement()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getTabPlacement--)</td><td headers="h2">Sets or gets the location where the tabs appear relative to the content. Possible values (defined in `SwingConstants`, which is implemented by `JTabbedPane`) are `TOP`, `BOTTOM`, `LEFT`, and `RIGHT`.</td>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[insertTab(String, Icon, Component, String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#insertTab-java.lang.String-javax.swing.Icon-java.awt.Component-java.lang.String-int-)</td><td headers="h102">Inserts a tab at the specified index, where the first tab is at index 0. The arguments are the same as for `addTab`.</td>
<td headers="h101">[remove(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#remove-java.awt.Component-)<br />[removeTabAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#removeTabAt-int-)</td><td headers="h102">Removes the tab corresponding to the specified component or index.</td>
<td headers="h101">[removeAll()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#removeAll--)</td><td headers="h102">Removes all tabs.</td>
<td headers="h101">[int indexOfComponent(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#indexOfComponent-java.awt.Component-)<br />[int indexOfTab(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#indexOfTab-java.lang.String-)<br />[int indexOfTab(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#indexOfTab-javax.swing.Icon-)</td><td headers="h102">Returns the index of the tab that has the specified component, title, or icon.</td>
<td headers="h101">[void setSelectedIndex(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setSelectedIndex-int-)<br />[void setSelectedComponent(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setSelectedComponent-java.awt.Component-)</td><td headers="h102">Selects the tab that has the specified component or index. Selecting a tab has the effect of displaying its associated component.</td>
<td headers="h101">[int getSelectedIndex()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getSelectedIndex--)<br />[Component getSelectedComponent()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getSelectedComponent--)</td><td headers="h102">Returns the index or component for the selected tab.</td>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[void setComponentAt(int, Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setComponentAt-int-java.awt.Component-)<br />[Component getComponentAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getComponentAt-int-)</td><td headers="h202">Sets or gets which component is associated with the tab at the specified index. The first tab is at index 0.</td>
<td headers="h201">[void setTitleAt(int, String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setTitleAt-int-java.lang.String-)<br />[String getTitleAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getTitleAt-int-)</td><td headers="h202">Sets or gets the title of the tab at the specified index.</td>
<td headers="h201">[void setIconAt(int, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setIconAt-int-javax.swing.Icon-)<br />[Icon getIconAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getIconAt-int-)<br />[void setDisabledIconAt(int, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setDisabledIconAt-int-javax.swing.Icon-)<br />[Icon getDisabledIconAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getDisabledIconAt-int-)</td><td headers="h202">Sets or gets the icon displayed by the tab at the specified index.</td>
<td headers="h201">[void setBackgroundAt(int, Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setBackgroundAt-int-java.awt.Color-)<br />[Color getBackgroundAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getBackgroundAt-int-)<br />[void setForegroundAt(int, Color)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setForegroundAt-int-java.awt.Color-)<br />[Color getForegroundAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getForegroundAt-int-)</td><td headers="h202">Sets or gets the background or foreground color used by the tab at the specified index. By default, a tab uses the tabbed pane's background and foreground colors. For example, if the tabbed pane's foreground is black, then each tab's title is black except for any tabs for which you specify another color using `setForegroundAt`.</td>
<td headers="h201">[void setEnabledAt(int, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setEnabledAt-int-boolean-)<br />[boolean isEnabledAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#isEnabledAt-int-)</td><td headers="h202">Sets or gets the enabled state of the tab at the specified index.</td>
<td headers="h201">[void setMnemonicAt(int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setMnemonicAt-int-int-)<br />[int getMnemonicAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getMnemonicAt-int-)</td><td headers="h202">Sets or gets the keyboard mnemonic for accessing the specified tab.</td>
<td headers="h201">[void setDisplayedMnemonicIndexAt(int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setDisplayedMnemonicIndexAt-int-int-)<br />[int getDisplayedMnemonicIndexAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getDisplayedMnemonicIndexAt-int-)</td><td headers="h202">Sets or gets which character should be decorated to represent the mnemonic. This is useful when the mnemonic character appears multiple times in the tab's title and you don't want the first occurrence underlined.</td>
<td headers="h201">[void setToolTipTextAt(int, String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setToolTipTextAt-int-java.lang.String-)<br />[String getToolTipTextAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getToolTipTextAt-int-)</td><td headers="h202">Sets or gets the text displayed on tool tips for the specified tab.</td>
<th id="h301" align="left">Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[void setTabComponentAt(int, Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#setTabComponentAt-int-java.awt.Component-)</td><td headers="h302">Sets the component that is responsible for rendering the title or icon (or both) for the tab specified by the first argument. When a null value is specified, `JTabbedPane` renders the title or icon. The same component cannot be used for several tabs.</td>
<td headers="h301">[Component getTabComponentAt(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#getTabComponentAt-int-)</td><td headers="h302">Gets the tab component for the tab at the index specified by the argument. If there is no tab component for the specified tab, a null value is returned.</td>
<td headers="h301">[int indexOfTabComponent(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTabbedPane.html#indexOfTabComponent-java.awt.Component-)</td><td headers="h302">Checks if the specified component belongs to one of the tabs. Return the index of the corresponding tab or -1 if there is no such a tab.</td>

## <a name="eg" id="eg">Examples That Use Tabbed Panes</a>

This table lists examples that use `JTabbedPane` and points to where those examples are described.
<th id="h401" align="left">Example</th><th id="h402" align="left">Where Described</th><th id="h403" align="left">Notes</th>
<td headers="h401">[`TabbedPaneDemo`](../examples/components/index.html#TabbedPaneDemo)</td><td headers="h402">This page</td><td headers="h403">Demonstrates a few tabbed pane features, such as tool tips, icons, scrollable layout, and mnemonics.</td>
<td headers="h401">[`TabComponentsDemo`](../examples/components/index.html#TabComponentsDemo)</td><td headers="h402">This page</td><td headers="h403">Demonstrates custom components on tabs. Uses a tabbed pane with close buttons.</td>
<td headers="h401">[`BoxAlignmentDemo`](../examples/layout/index.html#BoxAlignmentDemo)</td><td headers="h402">[How to Use BoxLayout](../layout/box.html)</td><td headers="h403">Uses a `JTabbedPane` as the only child of a frame's content pane.</td>
<td headers="h401">[`BorderDemo`](../examples/components/index.html#BorderDemo)</td><td headers="h402">[How to Use Borders](../components/border.html)</td><td headers="h403">Uses its tabbed pane in a manner similar to `BoxAlignmentDemo`.</td>
<td headers="h401">[`DialogDemo`](../examples/components/index.html#DialogDemo)</td><td headers="h402">[How to Use Dialogs](dialog.html)</td><td headers="h403">Has a tabbed pane in the center of a frame's content pane, with a label below it.</td>
