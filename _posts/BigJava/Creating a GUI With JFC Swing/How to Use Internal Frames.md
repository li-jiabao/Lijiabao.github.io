
# How to Use Internal Frames

With the
[`JInternalFrame`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html) class you can display a [`JFrame`](frame.html)-like window within another window. Usually, you add internal frames to a desktop pane. The desktop pane, in turn, might be used as the content pane of a `JFrame`. The desktop pane is an instance of
[`JDesktopPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDesktopPane.html), which is a subclass of [`JLayeredPane`](layeredpane.html) that has added API for managing multiple overlapping internal frames.

You should consider carefully whether to base your program's GUI around frames or internal frames. Switching from internal frames to frames or vice versa is not necessarily a simple task. By experimenting with both frames and internal frames, you can get an idea of the tradeoffs involved in choosing one over the other.

Here is a picture of an application that has two internal frames (one of which is iconified) inside a regular frame:

<li>Click the Launch button to run InternalFrameDemo using
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#InternalFrameDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the InternalFrameDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/InternalFrameDemoProject/InternalFrameDemo.jnlp)<br /></li>
<li>Create new internal frames using the Create item in the Document menu.<br />
Each internal frame comes up 30 pixels lower and to the right of the place where the previous internal frame first appeared. This functionality is implemented in the `MyInternalFrame` class, which is the custom subclass of `JInternalFrame`.</li>

The following code, taken from
[`InternalFrameDemo.java`](../examples/components/InternalFrameDemoProject/src/components/InternalFrameDemo.java), creates the desktop and internal frames in the previous example.

```

**...//In the constructor of InternalFrameDemo, a JFrame subclass:**
    desktop = new JDesktopPane();
    createFrame(); //Create first window
    setContentPane(desktop);
    ...
    //Make dragging a little faster but perhaps uglier.
    desktop.setDragMode(JDesktopPane.OUTLINE_DRAG_MODE);
...
protected void createFrame() {
    MyInternalFrame frame = new MyInternalFrame();
    frame.setVisible(true);
    desktop.add(frame);
    try {
        frame.setSelected(true);
    } catch (java.beans.PropertyVetoException e) {}
}

**...//In the constructor of MyInternalFrame, a JInternalFrame subclass:**
static int openFrameCount = 0;
static final int xOffset = 30, yOffset = 30;

public MyInternalFrame() {
    super("Document #" + (++openFrameCount),
          true, //resizable
          true, //closable
          true, //maximizable
          true);//iconifiable
    //...Create the GUI and put it in the window...
    //...Then set the window size or call pack...
    ...
    //Set the window's location.
    setLocation(xOffset*openFrameCount, yOffset*openFrameCount);
}

```

## <a name="frame" id="frame">Internal Frames vs. Regular Frames</a>

The code for using internal frames is similar in many ways to the code for using regular Swing frames. Because internal frames have root panes, setting up the GUI for a `JInternalFrame` is very similar to setting up the GUI for a `JFrame`. `JInternalFrame` also provides other API, such as `pack`, that makes it similar to `JFrame`.

Just as for a regular frame, you must invoke `setVisible(true)` or `show()` on an internal frame to display it. The internal frame does not appear until you explicitly make it visible.

Internal frames are not windows or top-level containers, however, which makes them different from frames. For example, you must add an internal frame to a container (usually a `JDesktopPane`); an internal frame cannot be the root of a containment hierarchy. Also, internal frames do not generate window events. Instead, the user actions that would cause a frame to fire window events cause an internal frame to fire internal frame events.

Because internal frames are implemented with platform-independent code, they add some features that frames cannot give you. One such feature is that internal frames give you more control over their state and capabilities than frames do. You can programatically iconify or maximize an internal frame. You can also specify what icon goes in the internal frame's title bar. You can even specify whether the internal frame has the window decorations to support resizing, iconifying, closing, and maximizing.

Another feature is that internal frames are designed to work within desktop panes. The `JInternalFrame` API contains methods such as `moveToFront` that work only if the internal frame's container is a layered pane such as a `JDesktopPane`.

## <a name="rules" id="rules">Rules of Using Internal Frames</a>

If you have built any programs using `JFrame` and the other Swing components, then you already know a lot about how to use internal frames. The following list summarizes the rules for using internal frames. For additional information, see [How to Make Frames](frame.html) and [The JComponent Class](jcomponent.html).

<a name="drag" id="drag"></a>

When a desktop has many internal frames, the user might notice that moving them seems slow. Outline dragging is one way to avoid this problem. With outline dragging, only the outline of the internal frame is painted at the current mouse position while the internal frame's being dragged. The internal frame's innards are not repainted at a new position until dragging stops. The default behavior (called **live** dragging) is to reposition and repaint some or all of internal frame continuously while it is being moved; this can be slow if the desktop has many internal frames.

Use the `JDesktopPane` method `setDragMode`* to specify outline dragging. For example:

```

desktop.setDragMode(JDesktopPane.OUTLINE_DRAG_MODE);

```

## <a name="api" id="api">The Internal Frame API</a>

The following tables list the commonly used `JInternalFrame` constructors and methods, as well as a few methods that `JDesktopPane` provides. Besides the API listed in this section, `JInternalFrame` inherits useful API from its superclasses, `JComponent`, `Component`, and `Container`. See [The JComponent Class](jcomponent.html) for lists of methods from those classes.

Like `JInternalFrame`, `JDesktopPane` descends from `JComponent`, and thus provides the methods described in [The JComponent Class](jcomponent.html). Because `JDesktopPane` extends `JLayeredPane`, it also supports the methods described in [The Layered Pane API](layeredpane.html#api).

The API for using internal frames falls into these categories:

- [Creating the internal frame](#construct)
- [Adding components to the internal frame](#add)
- [Specifying the internal frame's visibility, size, and location](#layout)
- [Performing window operations on the internal frame](#window)
- [Controlling window decorations and capabilities](#decorate)
- [Using the JDesktopPane API](#JDesktopPane)
<th id="h1" align="left">Constructor or Method</th><th id="h2" align="left">Purpose</th>
<td headers="h1">[JInternalFrame()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#JInternalFrame--)<br />[JInternalFrame(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#JInternalFrame-java.lang.String-)<br />[JInternalFrame(String, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#JInternalFrame-java.lang.String-boolean-)<br />[JInternalFrame(String, boolean, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#JInternalFrame-java.lang.String-boolean-boolean-)<br />[JInternalFrame(String, boolean, boolean, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#JInternalFrame-java.lang.String-boolean-boolean-boolean-)<br />[JInternalFrame(String, boolean, boolean, boolean, boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#JInternalFrame-java.lang.String-boolean-boolean-boolean-boolean-)</td><td headers="h2">Create a `JInternalFrame` instance. The first argument specifies the title (if any) to be displayed by the internal frame. The rest of the arguments specify whether the internal frame should contain decorations allowing the user to resize, close, maximize, and iconify the internal frame (specified in that order). The default value for each boolean argument is `false`, which means that the operation is not allowed.</td>
<td headers="h1">[static int showInternalConfirmDialog(Component, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInternalConfirmDialog-java.awt.Component-java.lang.Object-)<br />[static String showInternalInputDialog(Component, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInternalInputDialog-java.awt.Component-java.lang.Object-)<br />[static Object showInternalMessageDialog(Component, Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInternalMessageDialog-java.awt.Component-java.lang.Object-)<br />[static int showInternalOptionDialog(Component, Object, String, int, int, Icon, Object[], Object)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html#showInternalOptionDialog-java.awt.Component-java.lang.Object-java.lang.String-int-int-javax.swing.Icon-java.lang.Object:A-java.lang.Object-)</td><td headers="h2">Create a `JInternalFrame` that simulates a dialog. See [How to Make Dialogs](dialog.html) for details.</td>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[void setContentPane(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setContentPane-java.awt.Container-)<br />[Container getContentPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#getContentPane--)</td><td headers="h102">Set or get the internal frame's content pane, which generally contains all of the internal frame's GUI, with the exception of the menu bar and window decorations.</td>
<td headers="h101">[void setJMenuBar(JMenuBar)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setJMenuBar-javax.swing.JMenuBar-)<br />[JMenuBar getJMenuBar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#getJMenuBar--)</td><td headers="h102">Set or get the internal frame's menu bar.</td>
<td headers="h101">[void setLayeredPane(JLayeredPane)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setLayeredPane-javax.swing.JLayeredPane-)<br />[JLayeredPane getLayeredPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#getLayeredPane--)</td><td headers="h102">Set or get the internal frame's layered pane.</td>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[void setVisible(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setVisible-boolean-)</td><td headers="h202">Make the internal frame visible (if `true`) or invisible (if `false`). You should invoke `setVisible(true)` on each `JInternalFrame` before adding it to its container. (Inherited from `Component`).</td>
<td headers="h201">[void pack()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#pack--)</td><td headers="h202">Size the internal frame so that its components are at their preferred sizes.</td>
<td headers="h201">[void setLocation(Point)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setLocation-java.awt.Point-)<br />[void setLocation(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setLocation-int-int-)</td><td headers="h202">Set the position of the internal frame. (Inherited from `Component`).</td>
<td headers="h201">[void setBounds(Rectangle)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setBounds-java.awt.Rectangle-)<br />[void setBounds(int, int, int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setBounds-int-int-int-int-)</td><td headers="h202">Explicitly set the size and location of the internal frame. (Inherited from `Component`).</td>
<td headers="h201">[void setSize(Dimension)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setSize-java.awt.Dimension-)<br />[void setSize(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setSize-int-int-)</td><td headers="h202">Explicitly set the size of the internal frame. (Inherited from `Component`).</td>
<th id="h301" align="left">Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[void setDefaultCloseOperation(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setDefaultCloseOperation-int-)<br />[int getDefaultCloseOperation()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#getDefaultCloseOperation--)</td><td headers="h302">Set or get what the internal frame does when the user attempts to "close" the internal frame. The default value is `DISPOSE_ON_CLOSE`. Other possible values are `DO_NOTHING_ON_CLOSE` and `HIDE_ON_CLOSE` See [Responding to Window-Closing Events](frame.html#windowevents) for details.</td>
<td headers="h301">[void addInternalFrameListener(InternalFrameListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#addInternalFrameListener-javax.swing.event.InternalFrameListener-)<br />[void removeInternalFrameListener(InternalFrameListener)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#removeInternalFrameListener-javax.swing.event.InternalFrameListener-)</td><td headers="h302">Add or remove an internal frame listener (`JInternalFrame`'s equivalent of a window listener). See [How to Write an Internal Frame Listener](../events/internalframelistener.html) for more information.</td>
<td headers="h301">[void moveToFront()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#moveToFront--)<br />[void moveToBack()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#moveToBack--)</td><td headers="h302">If the internal frame's parent is a layered pane such as a desktop pane, moves the internal frame to the front or back (respectively) of its layer.</td>
<td headers="h301">[void setClosed(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setClosed-boolean-)<br />[boolean isClosed()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#isClosed--)</td><td headers="h302">Set or get whether the internal frame is currently closed. The argument to `setClosed` must be `true`. When reopening a closed internal frame, you make it visible and add it to a container (usually the desktop pane you originally added it to).</td>
<td headers="h301">[void setIcon(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setIcon-boolean-)<br />[boolean isIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#isIcon--)</td><td headers="h302">Iconify or deiconify the internal frame, or determine whether it is currently iconified.</td>
<td headers="h301">[void setMaximum(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setMaximum-boolean-)<br />[boolean isMaximum()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#isMaximum--)</td><td headers="h302">Maximize or restore the internal frame, or determine whether it is maximized.</td>
<td headers="h301">[void setSelected(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setSelected-boolean-)<br />[boolean isSelected()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#isSelected--)</td><td headers="h302">Set or get whether the internal frame is the currently "selected" (activated) internal frame.</td>
<th id="h401" align="left">Method</th><th id="h402" align="left">Purpose</th>
<td headers="h401">[void setFrameIcon(Icon)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setFrameIcon-javax.swing.Icon-)<br />[Icon getFrameIcon()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#getFrameIcon--)</td><td headers="h402">Set or get the icon displayed in the title bar of the internal frame (usually in the top-left corner).</td>
<td headers="h401">[void setClosable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setClosable-boolean-)<br />[boolean isClosable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#isClosable--)</td><td headers="h402">Set or get whether the user can close the internal frame.</td>
<td headers="h401">[void setIconifiable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setIconifiable-boolean-)<br />[boolean isIconifiable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#isIconifiable--)</td><td headers="h402">Set or get whether the internal frame can be iconified.</td>
<td headers="h401">[void setMaximizable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setMaximizable-boolean-)<br />[boolean isMaximizable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#isMaximizable--)</td><td headers="h402">Set or get whether the user can maximize this internal frame.</td>
<td headers="h401">[void setResizable(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setResizable-boolean-)<br />[boolean isResizable()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#isResizable--)</td><td headers="h402">Set or get whether the internal frame can be resized.</td>
<td headers="h401">[void setTitle(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#setTitle-java.lang.String-)<br />[String getTitle()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JInternalFrame.html#getTitle--)</td><td headers="h402">Set or get the window title.</td>
<th id="h501" align="left">Constructor or Method</th><th id="h502" align="left">Purpose</th>
<td headers="h501">[JDesktopPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDesktopPane.html#JDesktopPane--)</td><td headers="h502">Creates a new instance of `JDesktopPane`.</td>
<td headers="h501">[JInternalFrame[] getAllFrames()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDesktopPane.html#getAllFrames--)</td><td headers="h502">Returns all `JInternalFrame` objects that the desktop contains.</td>
<td headers="h501">[JInternalFrame[] getAllFramesInLayer(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDesktopPane.html#getAllFramesInLayer-int-)</td><td headers="h502">Returns all `JInternalFrame` objects that the desktop contains that are in the specified layer. See [How to Use Layered Panes](layeredpane.html) for information about layers.</td>
<td headers="h501">[void setDragMode(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDesktopPane.html#setDragMode-int-)<br />[int getDragMode()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JDesktopPane.html#getDragMode--)</td><td headers="h502">Set or get the drag mode used for internal frames in this desktop. The integer can be either `JDesktopPane.LIVE_DRAG_MODE` or `JDesktopPane.OUTLINE_DRAG_MODE`. The default for the Java look and feel is live-drag mode.</td>

## <a name="eg" id="eg">Examples that Use Internal Frames</a>

The following examples use internal frames. Because internal frames are similar to regular frames, you should also look at [Examples that Use Frames](frame.html#eg).
<th id="h601" align="left">Example</th><th id="h602" align="left">Where Described</th><th id="h603" align="left">Notes</th>
<td headers="h601">[`MyInternalFrame`](../examples/components/index.html#InternalFrameDemo)</td><td headers="h602">This page.</td><td headers="h603">Implements an internal frame that appears at an offset to the previously created internal frame.</td>
<td headers="h601">[`InternalFrameDemo`](../examples/components/index.html#InternalFrameDemo)</td><td headers="h602">This page.</td><td headers="h603">Lets you create internal frames (instances of `MyInternalFrame`) that go into the application's `JDesktopPane`.</td>
<td headers="h601">[`InternalFrameEventDemo`](../examples/events/index.html#InternalFrameEventDemo)</td><td headers="h602">[How to Write an Internal Frame Listener](../events/internalframelistener.html)</td><td headers="h603">Demonstrates listening for internal frame events. Also demonstrates positioning internal frames within a desktop pane.</td>
