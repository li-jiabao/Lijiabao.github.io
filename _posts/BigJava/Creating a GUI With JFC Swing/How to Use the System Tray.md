
# How to Use the System Tray

The system tray is a specialized area of the desktop where users can access currently running programs. This area may be referred to differently on various operating systems. On Microsoft Windows, the system tray is referred to as the Taskbar Status Area, while on the GNU Network Object Model Environment (GNOME) Desktop it is referred to as the Notification Area. On K Desktop Environment (KDE) this area is referred to as the System Tray. However, on each system the tray area is shared by all applications running on the desktop.

The 
[`java.awt.SystemTray`](https://docs.oracle.com/javase/8/docs/api/java/awt/SystemTray.html) class introduced in Java&#8482; SE version 6 represents the system tray for a desktop. The system tray can be accessed by calling the static `SystemTray.getSystemTray()` method. Before calling this method, use the static method 
[`isSupported()`](https://docs.oracle.com/javase/8/docs/api/java/awt/SystemTray.html#isSupported--) to check that the system tray is supported. If the system tray is not supported on this platform, the `isSupported()` method returns false. If the application attempts to call the `getSystemTray()` method in such a case, the method will throw a `java.lang.UnsupportedOperationException`.

An application cannot create an instance of the `SystemTray` class. Only a single instance created within this class can exist, and this instance can be obtained using the 
[`getSystemTray()`](https://docs.oracle.com/javase/8/docs/api/java/awt/SystemTray.html#getSystemTray--) method.

The system tray contains one or more tray icons which are added to the tray using the 
[`add(java.awt.TrayIcon)`](https://docs.oracle.com/javase/8/docs/api/java/awt/SystemTray.html#add-java.awt.TrayIcon-) method. They can be removed when they are no longer needed with the 
[`remove(java.awt.TrayIcon)`](https://docs.oracle.com/javase/8/docs/api/java/awt/SystemTray.html#remove-java.awt.TrayIcon-) method.

The 
[`TrayIcon`](https://docs.oracle.com/javase/8/docs/api/java/awt/TrayIcon.html) class functionality goes beyond the icon that is displayed in the tray. It also includes a text tooltip, a pop-up menu, ballon messages, and a set of listeners associated with it. A `TrayIcon` object generates various mouse events and supports the addition of corresponding listeners to receive notification of these events. The `TrayIcon` class processes some of the events itself. For example, by default, when a right-click is performed on the tray icon, it displays the specified pop-up menu. When a double-click is performed, the `TrayIcon` object generates an `ActionEvent` to launch an application. When the mouse pointer hovers over the tray icon, the tooltip is displayed. The icon image is automatically resized to fit the space allocated for the image on the tray.

The following demo, developed using the AWT package, demonstrates the features of SystemTray and TrayIcon classes.

Unfortunately, the current implementation of the `TrayIcon` class provides limited support of the Swing pop-up menu (the `JPopupMenu` class) and does not enable an application to use all of the capabilities of the `javax.swing` package. The workaround proposal for this issue is described in the Bug Database, see Bug ID [6285881](http://bugs.java.com/bugdatabase/view_bug.do?bug_id=6285881).

<li>Place the 
[`bulb.gif`](../examples/misc/TrayIconDemoProject/src/misc/images/bulb.gif) image file in the `image` directory. Compile and run the example, consult the [example index](../examples/misc/index.html#TrayIconDemo).</li>
<li>The tray icon will appear in the system tray.
<center><img src="../../figures/uiswing/misc/bulb32x32.gif" width="219" height="27" align="bottom" alt="Tray icon image" /></center></li>
1. Double-click the tray icon to launch the corresponding application. The dialog box will be displayed.
1. Hover the mouse pointer over the tray icon and click the right mouse button. The pop-up menu appears.
<li>Select the Set auto size checkbox menu item. Notice that the icon appearance is changed as follows.
<center><img src="../../figures/uiswing/misc/bulb16x16.gif" width="16" height="16" align="bottom" alt="Tray icon image is resized" /></center></li>
1. Select the Set tooltip checkbox menu item. Hover the mouse pointer over the tray icon. The tooltip appears.
1. Choose the About menu item. The dialog box appears. Close the dialog box.
1. Choose any of the Display submenu items. Each of these items displays a message dialog box of a particular type: error, warning, info, or standard.
<li>Use the Exit menu item to quit the application.
<hr /></li>
The following code snippet shows how to add a tray icon to the system tray and apply a pop-up menu:

```

...
        //Check the SystemTray is supported
        if (!SystemTray.isSupported()) {
            System.out.println("SystemTray is not supported");
            return;
        }
        final PopupMenu popup = new PopupMenu();
        final TrayIcon trayIcon =
                new TrayIcon(createImage("images/bulb.gif", "tray icon"));
        final SystemTray tray = SystemTray.getSystemTray();
       
        // Create a pop-up menu components
        MenuItem aboutItem = new MenuItem("About");
        CheckboxMenuItem cb1 = new CheckboxMenuItem("Set auto size");
        CheckboxMenuItem cb2 = new CheckboxMenuItem("Set tooltip");
        Menu displayMenu = new Menu("Display");
        MenuItem errorItem = new MenuItem("Error");
        MenuItem warningItem = new MenuItem("Warning");
        MenuItem infoItem = new MenuItem("Info");
        MenuItem noneItem = new MenuItem("None");
        MenuItem exitItem = new MenuItem("Exit");
       
        //Add components to pop-up menu
        popup.add(aboutItem);
        popup.addSeparator();
        popup.add(cb1);
        popup.add(cb2);
        popup.addSeparator();
        popup.add(displayMenu);
        displayMenu.add(errorItem);
        displayMenu.add(warningItem);
        displayMenu.add(infoItem);
        displayMenu.add(noneItem);
        popup.add(exitItem);
       
        trayIcon.setPopupMenu(popup);
       
        try {
            tray.add(trayIcon);
        } catch (AWTException e) {
            System.out.println("TrayIcon could not be added.");
        }
...

```

The complete code for this demo is available in the 
[`TrayIconDemo.java`](../examples/misc/TrayIconDemoProject/src/misc/TrayIconDemo.java) file. This demo also uses the 
[`bulb.gif`](../examples/misc/TrayIconDemoProject/src/misc/images/bulb.gif) image file.

Removing the current limitations on applying Swing components will enable developers to add such components as `JMenuItem` (with image), `JRadioButtonMenuItem`, and `JCheckBoxMenuItem`.

## <a name="api" id="api">The SystemTray API</a>

Only a single instance created within `SystemTray` class can exist.
<th id="h1" align="left">Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[add](https://docs.oracle.com/javase/8/docs/api/java/awt/SystemTray.html#add-java.awt.TrayIcon-)</td><td headers="h2">Adds a tray icon to the system tray. The tray icon becomes visible in the system tray once it is added. The order in which icons are displayed in a tray is not specified &#8212; it is platform- and implementation-dependent.</td>
<td headers="h1">[getSystemTray](https://docs.oracle.com/javase/8/docs/api/java/awt/SystemTray.html#getSystemTray--)</td><td headers="h2">Gets the `SystemTray` instance that represents the desktop's tray area. This method always returns the same instance per application. On some platforms the system tray may not be supported. Use the `isSupported()` method to check if the system tray is supported.</td>
<td headers="h1">[isSupported](https://docs.oracle.com/javase/8/docs/api/java/awt/SystemTray.html#isSupported--)</td><td headers="h2">Returns information as to whether the system tray is supported on the current platform. In addition to displaying the tray icon, minimal system tray support includes either a pop-up menu (see the `TrayIcon.setPopupMenu(PopupMenu)` method) or an action event (see the `TrayIcon.addActionListener(ActionListener)`).</td>

## <a name="api__1" id="api__1">The TrayIcon API</a>

A `TrayIcon` object represents a tray icon that can be added to the system tray. A `TrayIcon` object can have a tooltip (text), an image, a pop-up menu, and a set of listeners associated with it.
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[setImageAutoSize](https://docs.oracle.com/javase/8/docs/api/java/awt/TrayIcon.html#setImageAutoSize-boolean-)</td><td headers="h102">Sets the auto-size property. Auto-size determines whether the tray image is automatically sized to fit the space allocated for the image on the tray. By default, the auto-size property is set to `false`.</td>
<td headers="h101">[setPopupMenu](https://docs.oracle.com/javase/8/docs/api/java/awt/TrayIcon.html#setPopupMenu-java.awt.PopupMenu-)</td><td headers="h102">Sets the pop-up menu for this `TrayIcon` object. If pop-up is `null`, no pop-up menu will be associated with this `TrayIcon` object.</td>
<td headers="h101">[setToolTip](https://docs.oracle.com/javase/8/docs/api/java/awt/TrayIcon.html#setToolTip-java.lang.String-)</td><td headers="h102">Sets the tooltip string for this `TrayIcon` object. The tooltip is displayed automatically when the mouse hovers over the icon. Setting the tooltip to `null` removes any tooltip text. When displayed, the tooltip string may be truncated on some platforms; the number of characters that may be displayed is platform-dependent.</td>

## <a name="eg" id="eg">Examples That Use the SystemTray API</a>
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
