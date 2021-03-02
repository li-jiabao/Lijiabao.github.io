
# How to Use Menus

A menu provides a space-saving way to let the user choose one of several options. Other components with which the user can make a one-of-many choice include [combo boxes](combobox.html), [lists](list.html), [radio buttons](button.html#radiobutton), [spinners](spinner.html), and [tool bars](toolbar.html). If any of your menu items performs an action that is duplicated by another menu item or by a tool-bar button, then in addition to this section you should read 
[How to Use Actions](../misc/action.html).

Menus are unique in that, by convention, they aren't placed with the other components in the UI. Instead, a menu usually appears either in a **menu bar** or as a **popup menu**. A menu bar contains one or more menus and has a customary, platform-dependent location &#151; usually along the top of a window. A popup menu is a menu that is invisible until the user makes a platform-specific mouse action, such as pressing the right mouse button, over a popup-enabled component. The popup menu then appears under the cursor.

The following figure shows many menu-related components: a menu bar, menus, menu items, radio button menu items, check box menu items, and separators. As you can see, a menu item can have either an image or text, or both. You can also specify other properties, such as font and color.

<!--
<font color=gray>
[PENDING: This figure will be updated.
We may also add callouts for each component type.]
</font>


The rest of this section teaches you about the menu components and tells you how to use various menu features:

- [The menu component hierarchy](#hierarchy)
- [Creating menus](#create)
- [Handling events from menu items](#event)
- [Enabling keyboard operation](#mnemonic)
- [Bringing up a popup menu](#popup)
- [Customizing menu layout](#custom)
- [The Menu API](#api)
- [Examples that use menus](#eg)

## <a name="hierarchy" id="hierarchy">The Menu Component Hierarchy</a>

Here is a picture of the inheritance hierarchy for the menu-related classes:

As the figure shows, menu items (including menus) are simply [buttons](button.html). You might be wondering how a menu, if it's only a button, shows its menu items. The answer is that when a menu is activated, it automatically brings up a popup menu that displays the menu items.

## <a name="create" id="create">Creating Menus</a>

The following code creates the menus shown near the beginning of this menu section. The bold lines of code create and connect the menu objects; the other code sets up or customizes the menu objects. You can find the entire program in 
[`MenuLookDemo.java`](../examples/components/MenuLookDemoProject/src/components/MenuLookDemo.java). Other required files are listed in the [example index](../examples/components/index.html#MenuLookDemo). <!-- *******  boilerplate stuff ******* -->

<li>Click the Launch button to run the MenuLook Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#MenuLookDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ButtonDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/MenuLookDemoProject/MenuLookDemo.jnlp)<br /></li>

Because this code has no event handling, the menus do nothing useful except to look as they should. If you run the example, you'll notice that despite the lack of custom event handling, menus and submenus appear when they should, and the check boxes and radio buttons respond appropriately when the user chooses them.

```

**//Where the GUI is created:**
JMenuBar menuBar;
JMenu menu, submenu;
JMenuItem menuItem;
JRadioButtonMenuItem rbMenuItem;
JCheckBoxMenuItem cbMenuItem;

//Create the menu bar.
**menuBar = new JMenuBar();**

//Build the first menu.
**menu = new JMenu("A Menu");**
menu.setMnemonic(KeyEvent.VK_A);
menu.getAccessibleContext().setAccessibleDescription(
        "The only menu in this program that has menu items");
**menuBar.add(menu);**

//a group of JMenuItems
**menuItem = new JMenuItem("A text-only menu item"**,
                         KeyEvent.VK_T**);**
menuItem.setAccelerator(KeyStroke.getKeyStroke(
        KeyEvent.VK_1, ActionEvent.ALT_MASK));
menuItem.getAccessibleContext().setAccessibleDescription(
        "This doesn't really do anything");
**menu.add(menuItem);**

<b>menuItem = new JMenuItem("Both text and icon",
                         new ImageIcon("images/middle.gif"));</b>
menuItem.setMnemonic(KeyEvent.VK_B);
**menu.add(menuItem);**

**menuItem = new JMenuItem(new ImageIcon("images/middle.gif"));**
menuItem.setMnemonic(KeyEvent.VK_D);
**menu.add(menuItem);**

//a group of radio button menu items
**menu.addSeparator();**
ButtonGroup group = new ButtonGroup();
**rbMenuItem = new JRadioButtonMenuItem("A radio button menu item");**
rbMenuItem.setSelected(true);
rbMenuItem.setMnemonic(KeyEvent.VK_R);
group.add(rbMenuItem);
**menu.add(rbMenuItem);**

**rbMenuItem = new JRadioButtonMenuItem("Another one");**
rbMenuItem.setMnemonic(KeyEvent.VK_O);
group.add(rbMenuItem);
**menu.add(rbMenuItem);**

//a group of check box menu items
**menu.addSeparator();**
**cbMenuItem = new JCheckBoxMenuItem("A check box menu item");**
cbMenuItem.setMnemonic(KeyEvent.VK_C);
**menu.add(cbMenuItem);**

**cbMenuItem = new JCheckBoxMenuItem("Another one");**
cbMenuItem.setMnemonic(KeyEvent.VK_H);
**menu.add(cbMenuItem);**

//a submenu
**menu.addSeparator();**
**submenu = new JMenu("A submenu");**
submenu.setMnemonic(KeyEvent.VK_S);

**menuItem = new JMenuItem("An item in the submenu");**
menuItem.setAccelerator(KeyStroke.getKeyStroke(
        KeyEvent.VK_2, ActionEvent.ALT_MASK));
**submenu.add(menuItem);**

**menuItem = new JMenuItem("Another item");**
**submenu.add(menuItem);**
**menu.add(submenu);**

//Build second menu in the menu bar.
**menu = new JMenu("Another Menu");**
menu.setMnemonic(KeyEvent.VK_N);
menu.getAccessibleContext().setAccessibleDescription(
        "This menu does nothing");
**menuBar.add(menu);**

...
**frame.setJMenuBar(**theJMenuBar**);**

```

As the code shows, to set the menu bar for a `JFrame`, you use the `setJMenuBar` method. To add a `JMenu` to a `JMenuBar`, you use the `add(JMenu)` method. To add menu items and submenus to a `JMenu`, you use the `add(JMenuItem)` method.

Menu items, like other components, can be in at most one container. If you try to add a menu item to a second menu, the menu item will be removed from the first menu before being added to the second. For a way of implementing multiple components that do the same thing, see 
[How to Use Actions](../misc/action.html).

Other methods in the preceding code include `setAccelerator` and `setMnemonic`, which are discussed a little later in [Enabling Keyboard Operation](#mnemonic). The `setAccessibleDescription` method is discussed in 
[How to Support Assistive Technologies](../misc/access.html).

## <a name="event" id="event">Handling Events from Menu Items</a>

To detect when the user chooses a `JMenuItem`, you can listen for action events (just as you would for a [`JButton`](button.html)). To detect when the user chooses a `JRadioButtonMenuItem`, you can listen for either action events or item events, as described in [How to Use Radio Buttons](button.html#radiobutton). For `JCheckBoxMenuItem`s, you generally listen for item events, as described in [How to Use Check Boxes](button.html#checkbox).

<!-- *******  boilerplate stuff ******* -->

<li>Click the Launch button to run the Menu Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#MenuDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ButtonDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/MenuDemoProject/MenuDemo.jnlp)<br /></li>

Here is the code that implements the event handling:

```

public class MenuDemo ... implements ActionListener,
                                     ItemListener {
    ...
    public MenuDemo() {
        **//...for each JMenuItem instance:**
        menuItem.addActionListener(this);
        ...
        **//for each JRadioButtonMenuItem: **
        rbMenuItem.addActionListener(this);
        ...
        **//for each JCheckBoxMenuItem: **
        cbMenuItem.addItemListener(this);
        ...
    }

    public void actionPerformed(ActionEvent e) {
        <em>//...Get information from the action event...
        //...Display it in the text area...</em>
    }

    public void itemStateChanged(ItemEvent e) {
        <em>//...Get information from the item event...
        //...Display it in the text area...</em>
    }

```

For examples of handling action and item events, see the [button](button.html), [radio button](button.html#radiobutton), and [check box](button.html#checkbox) sections, as well as the [list of examples](#eg) at the end of this section.

## <a name="mnemonic" id="mnemonic">Enabling Keyboard Operation</a>

Menus support two kinds of keyboard alternatives: mnemonics and accelerators. **Mnemonics** offer a way to use the keyboard to navigate the menu hierarchy, increasing the accessibility of programs. **Accelerators**, on the other hand, offer keyboard shortcuts to **bypass** navigating the menu hierarchy. Mnemonics are for all users; accelerators are for power users.

A mnemonic is a key that makes an already visible menu item be chosen. For example, in `MenuDemo` the first menu has the mnemonic A, and its second menu item has the mnemonic B. This means that, when you run `MenuDemo` with the Java look and feel, pressing the Alt and A keys makes the first menu appear. While the first menu is visible, pressing the B key (with or without Alt) makes the second menu item be chosen. A menu item generally displays its mnemonic by underlining the first occurrence of the mnemonic character in the menu item's text, as the following snapshot shows.

An accelerator is a key combination that causes a menu item to be chosen, whether or not it's visible. For example, pressing the Alt and 2 keys in `MenuDemo` makes the first item in the first menu's submenu be chosen, without bringing up any menus. Only leaf menu items &#151; menus that don't bring up other menus &#151; can have accelerators. The following snapshot shows how the Java look and feel displays a menu item that has an accelerator.

You can specify a mnemonic either when constructing the menu item or with the `setMnemonic` method. To specify an accelerator, use the `setAccelerator` method. Here are examples of setting mnemonics and accelerators:

```

//Setting the mnemonic when constructing a menu item:
menuItem = new JMenuItem("A text-only menu item",
                         KeyEvent.VK_T);

//Setting the mnemonic after creation time:
menuItem.setMnemonic(KeyEvent.VK_T);

//Setting the accelerator:
menuItem.setAccelerator(KeyStroke.getKeyStroke(
        KeyEvent.VK_T, ActionEvent.ALT_MASK));

```

As you can see, you set a mnemonic by specifying the 
[`KeyEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyEvent.html) constant corresponding to the key the user should press. To specify an accelerator you must use a 
[`KeyStroke`](https://docs.oracle.com/javase/8/docs/api/javax/swing/KeyStroke.html) object, which combines a key (specified by a `KeyEvent` constant) and a modifier-key mask (specified by an 
[`ActionEvent`](https://docs.oracle.com/javase/8/docs/api/java/awt/event/ActionEvent.html) constant).

Because popup menus, unlike regular menus, aren't always contained by a component, accelerators in popup menu items don't work unless the popup menu is visible.

## <a name="popup" id="popup">Bringing Up a Popup Menu</a>

To bring up a popup menu ( 
[`JPopupMenu`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html)), you must register a mouse listener on each component that the popup menu should be associated with. The mouse listener must detect user requests that the popup menu be brought up.

The exact gesture that should bring up a popup menu varies by look and feel. In Microsoft Windows, the user by convention brings up a popup menu by releasing the right mouse button while the cursor is over a component that is popup-enabled. In the Java look and feel, the customary trigger is either pressing the right mouse button (for a popup that goes away when the button is released) or clicking it (for a popup that stays up). <!-- 
In a future release,
a new mechanism for automatically triggering popup menus
in the appropriate way for the look and feel
might be added; see
[bug #4634626](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=4634626)-->

<!-- *******  boilerplate stuff ******* -->

<li>Click the Launch button to run the PopupMenu Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#PopupMenuDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the PopupMenuDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/PopupMenuDemoProject/PopupMenuDemo.jnlp)<br /></li>

```

**//...where instance variables are declared:**
JPopupMenu popup;

    **//...where the GUI is constructed:**
    //Create the popup menu.
    popup = new JPopupMenu();
    menuItem = new JMenuItem("A popup menu item");
    menuItem.addActionListener(this);
    popup.add(menuItem);
    menuItem = new JMenuItem("Another popup menu item");
    menuItem.addActionListener(this);
    popup.add(menuItem);

    //Add listener to components that can bring up popup menus.
    MouseListener popupListener = new PopupListener();
    output.addMouseListener(popupListener);
    menuBar.addMouseListener(popupListener);
...
class PopupListener extends MouseAdapter {
    public void mousePressed(MouseEvent e) {
        maybeShowPopup(e);
    }

    public void mouseReleased(MouseEvent e) {
        maybeShowPopup(e);
    }

    private void maybeShowPopup(MouseEvent e) {
        if (e.isPopupTrigger()) {
            popup.show(e.getComponent(),
                       e.getX(), e.getY());
        }
    }
}

```

Popup menus have a few interesting implementation details. One is that every menu has an associated popup menu. When the menu is activated, it uses its associated popup menu to show its menu items.

Another detail is that a popup menu itself uses another component to implement the window containing the menu items. Depending on the circumstances under which the popup menu is displayed, the popup menu might implement its "window" using a lightweight component (such as a `JPanel`), a "mediumweight" component (such as a 
[`Panel`](https://docs.oracle.com/javase/8/docs/api/java/awt/Panel.html)), or a heavyweight window (something that inherits from 
[`Window`](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html)).

Lightweight popup windows are more efficient than heavyweight windows but, prior to the Java SE Platform 6 Update 12 release, they didn't work well if you had any heavyweight components inside your GUI. Specifically, when the lightweight popup's display area intersects the heavyweight component's display area, the heavyweight component is drawn on top. This is one of the reasons that, prior to the 6u12 release, we recommended against mixing heavyweight and lightweight components. If you are using an older release and absolutely need to use a heavyweight component in your GUI, then you can invoke `JPopupMenu.setLightWeightPopupEnabled(false)` to disable lightweight popup windows. For information on mixing components in the 6u12 release and later, please see 
[Mixing Heavyweight and Lightweight Components](http://www.oracle.com/technetwork/articles/java/mixing-components-433992.html).

## <a name="custom" id="custom">Customizing Menu Layout</a>

Because menus are made up of ordinary Swing components, you can easily customize them. For example, you can add any lightweight component to a `JMenu` or `JMenuBar`. And because `JMenuBar` uses [`BoxLayout`](../layout/box.html), you can customize a menu bar's layout just by adding invisible components to it. Here is an example of adding a [glue](../layout/box.html#filler) component to a menu bar, so that the last menu is at the right edge of the menu bar:

```

**//...create and add some menus...**
menuBar.add(Box.createHorizontalGlue());
**//...create the rightmost menu...**
menuBar.add(rightMenu);

```

<li>Click the Launch button to run the MenuGlue Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#MenuGlueDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ButtonDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/MenuGlueDemoProject/MenuGlueDemo.jnlp)<br /></li>

Here's the modified menu layout that MenuGlueDemo displays:

Another way of changing the look of menus is to change the layout managers used to control them. For example, you can change a menu bar's layout manager from the default left-to-right `BoxLayout` to something such as `GridLayout`. <!-- *******  boilerplate stuff ******* -->

<li>Click the Launch button to run the MenuLayout Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#MenuLayoutDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the MenuLayoutDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/MenuLayoutDemoProject/MenuLayoutDemo.jnlp)<br /></li>

Here's a picture of the menu layout that `MenuLayoutDemo` creates:

## <a name="api" id="api">The Menu API</a>

The following tables list the commonly used menu constructors and methods. The API for using menus falls into these categories:

- [Creating and Setting Up Menu Bars](#menubarapi)
- [Creating and Populating Menus](#menuapi)
- [Creating, Populating, and Controlling Popup Menus](#popupapi)
- [Implementing Menu Items](#itemapi)
<th id="h1" align="left">Constructor or Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[JMenuBar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuBar.html#JMenuBar--)</td><td headers="h2">Creates a menu bar.</td>
<td headers="h1">[JMenu add(JMenu)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuBar.html#add-javax.swing.JMenu-)</td><td headers="h2">Adds the menu to the end of the menu bar.</td>
<td headers="h1">[void setJMenuBar(JMenuBar)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setJMenuBar-javax.swing.JMenuBar-)<br />[JMenuBar getJMenuBar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getJMenuBar--)<br />**(in `JApplet`, `JDialog`, `JFrame`, `JInternalFrame`, `JRootPane`)**</td><td headers="h2">Sets or gets the menu bar of an [applet](applet.html), [dialog](dialog.html), [frame](frame.html), [internal frame](internalframe.html), or [root pane](rootpane.html).</td>
<th id="h101" align="left">Constructor or Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[JMenu()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#JMenu--)<br />[JMenu(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#JMenu-java.lang.String-)<br />[JMenu(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#JMenu-javax.swing.Action-)</td><td headers="h102">Creates a menu. The string specifies the text to display for the menu. The `Action` specifies the text and other properties of the menu (see [How to Use Actions](../misc/action.html)).</td>
<td headers="h101">[JMenuItem add(JMenuItem)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#add-javax.swing.JMenuItem-)<br />[JMenuItem add(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#add-java.lang.String-)</td><td headers="h102">Adds a menu item to the current end of the menu. If the argument is a string, then the menu automatically creates a `JMenuItem` object that displays the specified text.</td>
<td headers="h101">[void addSeparator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#addSeparator--)</td><td headers="h102">Adds a separator to the current end of the menu.</td>
<td headers="h101">[JMenuItem insert(JMenuItem, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#insert-javax.swing.JMenuItem-int-)<br />[void insert(String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#insert-java.lang.String-int-)<br />[void insertSeparator(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#insertSeparator-int-)</td><td headers="h102">Inserts a menu item or separator into the menu at the specified position. The first menu item is at position 0, the second at position 1, and so on. The `JMenuItem` and `String` arguments are treated the same as in the corresponding `add` methods.</td>
<td headers="h101">[void remove(JMenuItem)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#remove-javax.swing.JMenuItem-)<br />[void remove(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#remove-int-)<br />[void removeAll()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenu.html#removeAll--)</td><td headers="h102">Removes the specified item(s) from the menu. If the argument is an integer, then it specifies the position of the menu item to be removed.</td>
<th id="h201" align="left">Constructor or Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[JPopupMenu()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#JPopupMenu--)<br />[JPopupMenu(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#JPopupMenu-java.lang.String-)</td><td headers="h202">Creates a popup menu. The optional string argument specifies the title that a look and feel might display as part of the popup window.</td>
<td headers="h201">[JMenuItem add(JMenuItem)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#add-javax.swing.JMenuItem-)<br />[JMenuItem add(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#add-java.lang.String-)</td><td headers="h202">Adds a menu item to the current end of the popup menu. If the argument is a string, then the menu automatically creates a `JMenuItem` object that displays the specified text.</td>
<td headers="h201">[void addSeparator()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#addSeparator--)</td><td headers="h202">Adds a separator to the current end of the popup menu.</td>
<td headers="h201">[void insert(Component, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#insert-java.awt.Component-int-)</td><td headers="h202">Inserts a menu item into the menu at the specified position. The first menu item is at position 0, the second at position 1, and so on. The `Component` argument specifies the menu item to add.</td>
<td headers="h201">[void remove(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#remove-int-)<br />[void removeAll()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#removeAll--)</td><td headers="h202">Removes the specified item(s) from the menu. If the argument is an integer, then it specifies the position of the menu item to be removed.</td>
<td headers="h201">[static void setLightWeightPopupEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#setLightWeightPopupEnabled-boolean-)</td><td headers="h202">By default, Swing implements a menu's window using a lightweight component. This can cause problems if you use any heavyweight components in your Swing program, as described in [Bringing Up a Popup Menu](#popup). (This is one of several reasons to avoid using heavyweight components.) As a workaround, invoke `JPopupMenu.setLightWeightPopupEnabled(false)`.</td>
<td headers="h201">[void show(Component, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPopupMenu.html#show-java.awt.Component-int-int-)</td><td headers="h202">Display the popup menu at the specified *x,y* position (specified in that order by the integer arguments) in the coordinate system of the specified component.</td>
<th id="h301" align="left">Constructor or Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[JMenuItem()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuItem.html#JMenuItem--)<br />[JMenuItem(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuItem.html#JMenuItem-java.lang.String-)<br />[JMenuItem(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuItem.html#JMenuItem-javax.swing.Icon-)<br />[JMenuItem(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuItem.html#JMenuItem-java.lang.String-javax.swing.Icon-)<br />[JMenuItem(String, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuItem.html#JMenuItem-java.lang.String-int-)<br />[JMenuItem(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuItem.html#JMenuItem-javax.swing.Action-)</td><td headers="h302">Creates an ordinary menu item. The icon argument, if present, specifies the icon that the menu item should display. Similarly, the string argument specifies the text that the menu item should display. The integer argument specifies the keyboard mnemonic to use. You can specify any of the relevant VK constants defined in the [KeyEvent](https://docs.oracle.com/javase/8/docs/api/java/awt/event/KeyEvent.html) class. For example, to specify the A key, use `KeyEvent.VK_A`.The constructor with the `Action` parameter sets the menu item's `Action`, causing the menu item's properties to be initialized from the `Action`. See [How to Use Actions](../misc/action.html) for details.</td>
<td headers="h301">[JCheckBoxMenuItem()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem--)<br />[JCheckBoxMenuItem(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-java.lang.String-)<br />[JCheckBoxMenuItem(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-javax.swing.Icon-)<br />[JCheckBoxMenuItem(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-java.lang.String-javax.swing.Icon-)<br />[JCheckBoxMenuItem(String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-java.lang.String-boolean-)<br />[JCheckBoxMenuItem(String, Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#JCheckBoxMenuItem-java.lang.String-javax.swing.Icon-boolean-)</td><td headers="h302">Creates a menu item that looks and acts like a check box. The string argument, if any, specifies the text that the menu item should display. If you specify `true` for the boolean argument, then the menu item is initially selected (checked). Otherwise, the menu item is initially unselected.</td>
<td headers="h301">[JRadioButtonMenuItem()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem--)<br />[JRadioButtonMenuItem(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-java.lang.String-)<br />[JRadioButtonMenuItem(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-javax.swing.Icon-)<br />[JRadioButtonMenuItem(String, Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-java.lang.String-javax.swing.Icon-)<br />[JRadioButtonMenuItem(String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-java.lang.String-boolean-)<br />[JRadioButtonMenuItem(Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-javax.swing.Icon-boolean-)<br />[JRadioButtonMenuItem(String, Icon, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRadioButtonMenuItem.html#JRadioButtonMenuItem-java.lang.String-javax.swing.Icon-boolean-)</td><td headers="h302">Creates a menu item that looks and acts like a radio button. The string argument, if any, specifies the text that the menu item should display. If you specify `true` for the boolean argument, then the menu item is initially selected. Otherwise, the menu item is initially unselected.</td>
<td headers="h301">[void setState(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#setState-boolean-)<br />[boolean getState()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JCheckBoxMenuItem.html#getState--)<br />**(in `JCheckBoxMenuItem`)**</td><td headers="h302">Set or get the selection state of a check box menu item.</td>
<td headers="h301">[void setEnabled(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setEnabled-boolean-)</td><td headers="h302">If the argument is true, enable the menu item. Otherwise, disable the menu item.</td>
<td headers="h301">[void setMnemonic(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setMnemonic-int-)</td><td headers="h302">Set the mnemonic that enables keyboard navigation to the menu or menu item. Use one of the VK constants defined in the `KeyEvent` class.</td>
<td headers="h301">[void setAccelerator(KeyStroke)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JMenuItem.html#setAccelerator-javax.swing.KeyStroke-)</td><td headers="h302">Set the accelerator that activates the menu item.</td>
<td headers="h301">[void setActionCommand(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setActionCommand-java.lang.String-)</td><td headers="h302">Set the name of the action performed by the menu item.</td>
<td headers="h301">[void addActionListener(ActionListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#addActionListener-java.awt.event.ActionListener-)<br />[void addItemListener(ItemListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#addItemListener-java.awt.event.ItemListener-)</td><td headers="h302">Add an event listener to the menu item. See [Handling Events from Menu Items](#event) for details.</td>
<td headers="h301">[void setAction(Action)](https://docs.oracle.com/javase/8/docs/api/javax/swing/AbstractButton.html#setAction-javax.swing.Action-)</td><td headers="h302">Set the `Action` associated with the menu item. See [How to Use Actions](../misc/action.html) for details.</td>
<td headers="h301"></td><td headers="h302">Many of the preceding methods are inherited from `AbstractButton`. See [The Button API](button.html#api) for information about other useful methods that `AbstractButton` provides.</td>

## <a name="eg" id="eg">Examples that Use Menus</a>

Menus are used in a few of our examples.
<th id="h401" align="left">Example</th><th id="h402" align="left">Where Described</th><th id="h403" align="left">Notes</th>

See the
[Using JavaFX UI Controls: Menu](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/menu_controls.htm) tutorial to learn how to create menus in JavaFX.
