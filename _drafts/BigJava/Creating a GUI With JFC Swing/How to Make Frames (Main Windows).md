
# How to Make Frames (Main Windows)

A Frame is a top-level window with a title and a border. The size of the frame includes any area designated for the border. The dimensions of the border area may be obtained using the `getInsets` method. Since the border area is included in the overall size of the frame, the border effectively obscures a portion of the frame, constraining the area available for rendering and/or displaying subcomponents to the rectangle which has an upper-left corner location of `(insets.left`, `insets.top)`, and has a size of `width - (insets.left + insets.right)` by `height - (insets.top + insets.bottom)`.

A frame, implemented as an instance of the 
[`JFrame`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html) class, is a window that has decorations such as a border, a title, and supports button components that close or iconify the window. Applications with a GUI usually include at least one frame. Applets sometimes use frames, as well.

To make a window that is dependent on another window &#151; disappearing when the other window is iconified, for example &#151; use a [`dialog`](dialog.html) instead of `frame.`. To make a window that appears within another window, use an [internal frame](internalframe.html). <a name="anexample" id="anexample"></a>

## <a name="anexample__1" id="anexample__1">Creating and Showing Frames</a>

Here is a picture of the extremely plain window created by the `FrameDemo` demonstration application. You can find the source code in 
[`FrameDemo.java`](../examples/components/FrameDemoProject/src/components/FrameDemo.java). You can [run FrameDemo](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FrameDemoProject/FrameDemo.jnlp) ( 
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).

The following `FrameDemo` code shows how to create and set up a frame.

```

<!-- removed as per Shannon's notes. dsd
//1. Optional: Specify who draws the window decorations. 
JFrame.setDefaultLookAndFeelDecorated(true);
-->
//1. Create the frame.
JFrame frame = new JFrame("FrameDemo");

//2. Optional: What happens when the frame closes?
frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

//3. Create components and put them in the frame.
**//...create emptyLabel...**
frame.getContentPane().add(emptyLabel, BorderLayout.CENTER);

//4. Size the frame.
frame.pack();

//5. Show it.
frame.setVisible(true);

```

Here are some details about the code:
<!-- commented out as per Shannon's notes.dsd
<li>
Calling `setDefaultLookAndFeelDecorated(true)` 
requests that 
any subsequently created frames
have window decorations provided
by the look and feel,
and not by the window system.
For details, see
[Specifying Window Decorations](#setDefaultLookAndFeelDecorated).
<p>
-->
1. The first line of code creates a frame using a constructor that lets you set the frame title. The other frequently used `JFrame` constructor is the no-argument constructor.
<li>Next the code specifies what happens when your user closes the frame. The `EXIT_ON_CLOSE` operation exits the program when your user closes the frame. This behavior is appropriate for this program because the program has only one frame, and closing the frame makes the program useless.
See [Responding to Window-Closing Events](#windowevents) for more information.
</li>
<li>The next bit of code adds a blank label to the frame content pane. If you're not already familiar with content panes and how to add components to them, please read [Adding Components to the Content Pane](toplevel.html#contentpane).
For frames that have menus, you'd typically add the menu bar to the frame here using the `setJMenuBar` method. See [How to Use Menus](menu.html) for details.
</li>
<li>The `pack` method sizes the frame so that all its contents are at or above their preferred sizes. An alternative to `pack` is to establish a frame size explicitly by calling `setSize` or `setBounds` (which also sets the frame location). In general, using `pack` is preferable to calling `setSize`, since `pack` leaves the frame layout manager in charge of the frame size, and layout managers are good at adjusting to platform dependencies and other factors that affect component size.
This example does not set the frame location, but it is easy to do so using either the `setLocationRelativeTo` or `setLocation` method. For example, the following code centers a frame onscreen:
<pre><code>
frame.setLocationRelativeTo(null);
</code></pre>
</li>
1. Calling `setVisible(true)` makes the frame appear onscreen. Sometimes you might see the `show` method used instead. The two usages are equivalent, but we use `setVisible(true)` for consistency's sake.




## <a name="setDefaultLookAndFeelDecorated" id="setDefaultLookAndFeelDecorated">Specifying Window Decorations</a>

By default, window decorations are supplied by the native window system. However, you can request that the look-and-feel provide the decorations for a frame. You can also specify that the frame have no window decorations at all, a feature that can be used on its own, or to provide your own decorations, or with 
[full-screen exclusive mode](../../extra/fullscreen/index.html).

Besides specifying who provides the window decorations, you can also specify which icon is used to represent the window. Exactly how this icon is used depends on the window system or look and feel that provides the window decorations. If the window system supports minimization, then the icon is used to represent the minimized window. Most window systems or look and feels also display the icon in the window decorations. A typical icon size is 16x16 pixels, but some window systems use other sizes.

The following snapshots show three frames that are identical except for their window decorations. As you can tell by the appearance of the button in each frame, all three use the Java look and feel. The first uses decorations provided by the window system, which happen to be Microsoft Windows, but could as easily be any other system running the Java platform.The second and third use window decorations provided by the Java look and feel. The third frame uses Java look and feel window decorations, but has a custom icon. <!-- changed the order of screen shots as per Shannon's notes.dsd -->
|<img src="../../figures/uiswing/components/FrameDemo2_2.png" width="170" height="100" alt="A frame with decorations provided by the window system" />|<img src="../../figures/uiswing/components/FrameDemo2_1.png" width="170" height="100" alt="A frame with decorations provided by the look and feel" />|<img src="../../figures/uiswing/components/FrameDemo2_3.png" width="170" height="100" alt="A frame with a custom icon" />
|Window decorations provided by the look and feel|Window decorations provided by the window system|Custom icon; window decorations provided by the look and feel

Here is an example of creating a frame with a custom icon and with window decorations provided by the look and feel:

```

//Ask for window decorations provided by the look and feel.
JFrame.setDefaultLookAndFeelDecorated(true);

//Create the frame.
JFrame frame = new JFrame("A window");

//Set the frame icon to an image loaded from a file.
frame.setIconImage(new ImageIcon(imgURL).getImage());

```

As the preceding code snippet implies, you must invoke the `setDefaultLookAndFeelDecorated` method **before** creating the frame whose decorations you wish to affect. The value you set with `setDefaultLookAndFeelDecorated` is used for all subsequently created `JFrame`s. You can switch back to using window system decorations by invoking `JFrame.setDefaultLookAndFeelDecorated(false)`. Some look and feels might not support window decorations; in this case, the window system decorations are used.

The full source code for the application that creates the frames pictured above is in 
[`FrameDemo2.java`](../examples/components/FrameDemo2Project/src/components/FrameDemo2.java). Besides showing how to choose window decorations, FrameDemo2 also shows how to disable all window decorations and gives an example of positioning windows. It includes two methods that create the `Image` objects used as icons &#151; one is loaded from a file, and the other is painted from scratch. <!-- ***************** boilerplate stuff **************** -->

<li>Click the Launch button to run the Frame Demo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#FrameDemo2).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the FrameDemo2 example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FrameDemo2Project/FrameDemo2.jnlp)<br />
<p><!--  ******* end boilerplate stuff  *******  -->
 <!--

<hr />**Try this:**&nbsp;<ol>
<li> [Run FrameDemo2](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/FrameDemo2Project/FrameDemo2.jnlp) (
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).    Or, to compile and run the example yourself,
     consult the
     [example index](../examples/components/index.html#FrameDemo2).

        --></p>
</li>
<li>Bring up two windows, both with look-and-feel-provided decorations, but with different icons.<br />
The Java look and feel displays the icons in its window decorations. Depending on your window system, the icon may be used elsewhere to represent the window, especially when the window is minimized.</li>
<li>Bring up one or more windows with window system decorations.<br />
See if your window system treats these icons differently.</li>
<li>Bring up one or more windows with no window decorations.<br />
Play with the various types of windows to see how the window decorations, window system, and frame icons interact.</li>

## <a name="windowevents" id="windowevents">Responding to Window-Closing Events</a>

By default, when the user closes a frame onscreen, the frame is hidden. Although invisible, the frame still exists and the program can make it visible again. If you want different behavior, then you need to either register a window listener that handles window-closing events, or you need to specify default close behavior using the `setDefaultCloseOperation` method. You can even do both.

The argument to `setDefaultCloseOperation` must be one of the following values, the first three of which are defined in the 
[`WindowConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/WindowConstants.html) interface (implemented by `JFrame`, `JInternalPane`, and `JDialog`):

<a name="4030718" id="4030718"></a>

`DISPOSE_ON_CLOSE` can have results similar to `EXIT_ON_CLOSE` if only one window is onscreen. More precisely, when the last displayable window within the Java virtual machine (VM) is disposed of, the VM may terminate. See 
[AWT Threading Issues](https://docs.oracle.com/javase/8/docs/api/java/awt/doc-files/AWTThreadIssues.html) for details.

The default close operation is executed after any window listeners handle the window-closing event. So, for example, assume that you specify that the default close operation is to dispose of a frame. You also implement a window listener that tests whether the frame is the last one visible and, if so, saves some data and exits the application. Under these conditions, when the user closes a frame, the window listener will be called first. If it does not exit the application, then the default close operation &#151; disposing of the frame &#151; will then be performed.

For more information about handling window-closing events, see 
[How to Write Window Listeners](../events/windowlistener.html). Besides handling window-closing events, window listeners can also react to other window state changes, such as iconification and activation.

## <a name="api" id="api">The Frame API</a>

The following tables list the commonly used `JFrame` constructors and methods. Other methods you might want to call are defined by the 
[`java.awt.Frame`](https://docs.oracle.com/javase/8/docs/api/java/awt/Frame.html), 
[`java.awt.Window`](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html), and 
[`java.awt.Component`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html) classes, from which `JFrame` descends.

Because each `JFrame` object has a root pane, frames have support for interposing input and painting behavior in front of the frame children, placing children on different "layers", and for Swing menu bars. These topics are introduced in [Using Top-Level Containers](toplevel.html) and explained in detail in [How to Use Root Panes](rootpane.html).

The API for using frames falls into these categories:

- [Creating and Setting Up a Frame](#creating)
- [Setting the Window Size and Location](#sizeplace)
- [Methods Related to the Root Pane](#rootpane)
<th id="h101" align="left">Method or Constructor</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[JFrame()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#JFrame--)<br />[JFrame(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#JFrame-java.lang.String-)</td><td headers="h102">Create a frame that is initially invisible. The `String` argument provides a title for the frame. To make the frame visible, invoke `setVisible(true)` on it.</td>
<td headers="h101">[void setDefaultCloseOperation(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setDefaultCloseOperation-int-)<br />[int getDefaultCloseOperation()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getDefaultCloseOperation--)</td><td headers="h102">Set or get the operation that occurs when the user pushes the close button on this frame. Possible choices are:- `DO_NOTHING_ON_CLOSE`- `HIDE_ON_CLOSE`- `DISPOSE_ON_CLOSE`- `EXIT_ON_CLOSE`The first three constants are defined in the [`WindowConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/WindowConstants.html) interface, which `JFrame` implements. The `EXIT_ON_CLOSE` constant is defined in the [`JFrame`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html) class.</td>
<td headers="h101">[void setIconImage(Image)](https://docs.oracle.com/javase/8/docs/api/java/awt/Frame.html#setIconImage-java.awt.Image-)<br />[Image getIconImage()](https://docs.oracle.com/javase/8/docs/api/java/awt/Frame.html#getIconImage--)<br />**(in `Frame`)**</td><td headers="h102">Set or get the icon that represents the frame. Note that the argument is a [java.awt.Image](https://docs.oracle.com/javase/8/docs/api/java/awt/Image.html) object, not a `javax.swing.ImageIcon` (or any other `javax.swing.Icon` implementation).</td>
<td headers="h101">[void setTitle(String)](https://docs.oracle.com/javase/8/docs/api/java/awt/Frame.html#setTitle-java.lang.String-)<br />[String getTitle()](https://docs.oracle.com/javase/8/docs/api/java/awt/Frame.html#getTitle--)<br />**(in `Frame`)**</td><td headers="h102">Set or get the frame title.</td>
<td headers="h101">[void setUndecorated(boolean)](https://docs.oracle.com/javase/8/docs/api/java/awt/Frame.html#setUndecorated-boolean-)<br />[boolean isUndecorated()](https://docs.oracle.com/javase/8/docs/api/java/awt/Frame.html#isUndecorated--)<br />**(in `Frame`)**</td><td headers="h102">Set or get whether this frame should be decorated. Works only if the frame is not yet displayable (has not been packed or shown). Typically used with [full-screen exclusive mode](../../extra/fullscreen/index.html) or to enable custom window decorations.</td>
<td headers="h101">[static void setDefaultLookAndFeelDecorated(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setDefaultLookAndFeelDecorated-boolean-)<br />[static boolean isDefaultLookAndFeelDecorated()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#isDefaultLookAndFeelDecorated--)</td><td headers="h102">Determine whether subsequently created `JFrame`s should have their Window decorations (such as borders, and widgets for closing the window) provided by the current look-and-feel. Note that this is only a hint, as some look and feels may not support this feature.</td>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[void pack()](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html#pack--)<br />**(in `Window`)**</td><td headers="h202">Size the window so that all its contents are at or above their preferred sizes.</td>
<td headers="h201">[void setSize(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setSize-int-int-)<br />[void setSize(Dimension)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setSize-java.awt.Dimension-)<br />[Dimension getSize()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getSize--)<br />**(in `Component`)**</td><td headers="h202">Set or get the total size of the window. The integer arguments to `setSize` specify the width and height, respectively.</td>
<td headers="h201">[void setBounds(int, int, int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setBounds-int-int-int-int-)<br />[void setBounds(Rectangle)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setBounds-java.awt.Rectangle-)<br />[Rectangle getBounds()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getBounds--)<br />**(in `Component`)**</td><td headers="h202">Set or get the size and position of the window. For the integer version of `setBounds`, the window upper left corner is at the **x, y** location specified by the first two arguments, and has the width and height specified by the last two arguments.</td>
<td headers="h201">[void setLocation(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#setLocation-int-int-)<br />[Point getLocation()](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getLocation--)<br />**(in `Component`)**</td><td headers="h202">Set or get the location of the upper left corner of the window. The parameters are the **x** and **y** values, respectively.</td>
<td headers="h201">[void setLocationRelativeTo(Component)](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html#setLocationRelativeTo-java.awt.Component-)<br />**(in `Window`)**</td><td headers="h202">Position the window so that it is centered over the specified component. If the argument is `null`, the window is centered onscreen. To properly center the window, you should invoke this method after the window size has been set.</td>
<th id="h301" align="left">Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[void setContentPane(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setContentPane-java.awt.Container-)<br />[Container getContentPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getContentPane--)</td><td headers="h302">Set or get the frame content pane. The content pane contains the visible GUI components within the frame. </td>
<td headers="h301">[JRootPane createRootPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#createRootPane--)<br />[void setRootPane(JRootPane)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setRootPane-javax.swing.JRootPane-)<br />[JRootPane getRootPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getRootPane--)</td><td headers="h302">Create, set, or get the frame root pane. The root pane manages the interior of the frame including the content pane, the glass pane, and so on.</td>
<td headers="h301">[void setJMenuBar(JMenuBar)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setJMenuBar-javax.swing.JMenuBar-)<br />[JMenuBar getJMenuBar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getJMenuBar--)</td><td headers="h302">Set or get the frame menu bar to manage a set of menus for the frame. </td>
<td headers="h301">[void setGlassPane(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setGlassPane-java.awt.Component-)<br />[Component getGlassPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getGlassPane--)</td><td headers="h302">Set or get the frame glass pane. You can use the glass pane to intercept mouse events or paint on top of your program GUI.</td>
<td headers="h301">[void setLayeredPane(JLayeredPane)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setLayeredPane-javax.swing.JLayeredPane-)<br />[JLayeredPane getLayeredPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getLayeredPane--)</td><td headers="h302">Set or get the frame layered pane. You can use the frame layered pane to put components on top of or behind other components. </td>

## <a name="eg" id="eg">Examples that Use Frames</a>

All of the standalone applications in this trail use `JFrame`. The following table lists a few and tells you where each is discussed.
<th id="h401" align="left">Example</th><th id="h402" align="left">Where Described</th><th id="h403" align="left">Notes</th>
<td headers="h401">[`FrameDemo`](../examples/components/index.html#FrameDemo)</td><td headers="h402">[The Example Explained](#anexample)</td><td headers="h403">Displays a basic frame with one component.</td>
<td headers="h401">[`FrameDemo2`](../examples/components/index.html#FrameDemo2)</td><td headers="h402">[Specifying Window Decorations](#setDefaultLookAndFeelDecorated)</td><td headers="h403">Lets you create frames with various window decorations.</td>
<td headers="h401">[`Framework`](../examples/components/index.html#Framework)</td><td headers="h402">&#151;</td><td headers="h403">A study in creating and destroying windows, in implementing a menu bar, and in exiting an application.</td>
<td headers="h401">[`LayeredPaneDemo`](../examples/components/index.html#LayeredPaneDemo)</td><td headers="h402">[How to Use Layered Panes](layeredpane.html)</td><td headers="h403">Illustrates how to use a layered pane (but not the frame layered pane).</td>
<td headers="h401">[`GlassPaneDemo`](../examples/components/index.html#GlassPaneDemo)</td><td headers="h402">[The Glass Pane](rootpane.html#glasspane)</td><td headers="h403">Illustrates the use of a frame glass pane.</td>
<td headers="h401">[`MenuDemo`](../examples/components/index.html#MenuDemo)</td><td headers="h402">[How to Use Menus](menu.html)</td><td headers="h403">Shows how to put a `JMenuBar` in a `JFrame`.</td>
