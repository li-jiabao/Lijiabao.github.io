
# How to Use Root Panes

In general, you don't directly create a 
[`JRootPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRootPane.html) object. Instead, you get a `JRootPane` (whether you want it or not!) when you instantiate [`JInternalFrame`](internalframe.html) or one of the top-level Swing containers, such as [`JApplet`](applet.html), [`JDialog`](dialog.html), and [`JFrame`](frame.html).

[Using Top-Level Containers](toplevel.html) tells you the basics of using root panes &#151; getting the content pane, setting its layout manager, and adding Swing components to it. This section tells you more about root panes, including the components that make up a root pane and how you can use them.

As the preceding figure shows, a root pane has four parts:

## <a name="glasspane" id="glasspane">The Glass Pane</a>

The glass pane is useful when you want to be able to catch events or paint over an area that already contains one or more components. For example, you can deactivate mouse events for a multi-component region by having the glass pane intercept the events. Or you can display an image over multiple components using the glass pane.

Here's a picture of an application that demonstrates glass pane features. It contains a check box that lets you set whether the glass pane is "visible" &#151; whether it can get events and paint itself onscreen. When the glass pane is visible, it blocks all input events from reaching the components in the content pane. It also paints a red dot in the place where it last detected a mouse-pressed event.

<li>Click the Launch button to run GlassPaneDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#GlassPaneDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the GlassPaneDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/GlassPaneDemoProject/GlassPaneDemo.jnlp)<br /></li>
<li>Click Button 1.<br />
The button's appearance changes to show that it's been clicked.</li>
<li>Click the check box so that the glass pane becomes "visible," and then click Button 1 again.<br />
The button does not act clicked because the glass pane intercepts all the mouse events. The glass pane paints a red circle where you release the mouse.</li>
<li>Click the check box again so that the glass pane is hidden.<br />
When the glass pane detects an event over the check box, it forwards it to the check box. Otherwise, the check box would not respond to clicks.</li>

The following code from 
[`GlassPaneDemo.java`](../examples/components/GlassPaneDemoProject/src/components/GlassPaneDemo.java) shows and hides the glass pane. This program happens to create its own glass pane. However, if a glass pane doesn't do any painting, the program might simply attach listeners to the default glass pane, as returned by `getGlassPane`.

```

myGlassPane = new MyGlassPane(...);
changeButton.addItemListener(myGlassPane);
frame.setGlassPane(myGlassPane);
...
class MyGlassPane extends JComponent
                  implements ItemListener {
    ...
    //React to change button clicks.
    public void itemStateChanged(ItemEvent e) {
        **setVisible(e.getStateChange() == ItemEvent.SELECTED);**
    }
...
}

```

The next code snippet implements the mouse-event handling for the glass pane. If a mouse event occurs over the check box, then the glass pane redispatches the event so that the check box receives it.

```

**...//In the implementation of the glass pane's mouse listener:**
public void mouseMoved(MouseEvent e) {
    redispatchMouseEvent(e, false);
}

<em>.../* The mouseDragged, mouseClicked, mouseEntered,
    * mouseExited, and mousePressed methods have the same
    * implementation as mouseMoved. */...</em>

public void mouseReleased(MouseEvent e) {
    redispatchMouseEvent(e, true);
}

private void redispatchMouseEvent(MouseEvent e,
                                  boolean repaint) {
    Point glassPanePoint = e.getPoint();
    Container container = contentPane;
    Point containerPoint = SwingUtilities.convertPoint(
                                    glassPane,
                                    glassPanePoint,
                                    contentPane);

    if (containerPoint.y &lt; 0) { //we're not in the content pane
        <em>//Could have special code to handle mouse events over
        //the menu bar or non-system window decorations, such as
        //the ones provided by the Java look and feel.</em>
    } else {
        //The mouse event is probably over the content pane.
        //Find out exactly which component it's over.
        Component component =
            SwingUtilities.getDeepestComponentAt(
                                    container,
                                    containerPoint.x,
                                    containerPoint.y);

        if ((component != null)
            &amp;&amp; (component.equals(liveButton))) {
            //Forward events over the check box.
            Point componentPoint = SwingUtilities.convertPoint(
                                        glassPane,
                                        glassPanePoint,
                                        component);
            component.dispatchEvent(new MouseEvent(component,
                                                 e.getID(),
                                                 e.getWhen(),
                                                 e.getModifiers(),
                                                 componentPoint.x,
                                                 componentPoint.y,
                                                 e.getClickCount(),
                                                 e.isPopupTrigger()));
        }
    }

    //Update the glass pane if requested.
    if (repaint) {
        glassPane.setPoint(glassPanePoint);
        glassPane.repaint();
    }
}

```

Here is the code in `MyGlassPane` that implements the painting.

```

protected void paintComponent(Graphics g) {
    if (point != null) {
        g.setColor(Color.red);
        g.fillOval(point.x - 10, point.y - 10, 20, 20);
    }
}

```

## <a name="layeredpane" id="layeredpane">The Layered Pane</a>

A layered pane is a container with depth such that overlapping components can appear one on top of the other. General information about layered panes is in 
[How to Use Layered Panes](layeredpane.html). This section discusses the particulars of how root panes use layered panes.

Each root pane places its menu bar and content pane in an instance of `JLayeredPane`. The Z ordering that the layered pane provides enables behavior such as displaying popup menus above other components.

You can choose to put components in the root pane's layered pane. If you do, then you should be aware that certain depths are defined to be used for specific functions, and you should use the depths as intended. Otherwise, your components might not play well with the others. Here's a diagram that shows the functional layers and their relationship:

The table below describes the intended use for each layer and lists the `JLayeredPane` constant that corresponds to each layer:
<th id="h1" align="left">Layer Name</th><th id="h2" align="left">Value</th><th id="h3" align="left">Description</th>

[LayeredPaneDemo](../examples/components/index.html#LayeredPaneDemo) that uses the root pane's layered pane, rather than creating a new layered pane.

<li>Click the Launch button to run RootLayeredPaneDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#RootLayeredPaneDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the RootLayeredPaneDemo" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/RootLayeredPaneDemoProject/RootLayeredPaneDemo.jnlp)<br /></li>
<li>Move the cursor around in the window, so that Duke moves on top of other components.<br />
Note that when the cursor is on top of non-label components &#151; whether it's in the content pane or in the Java-look-and-feel provided title bar &#151; Duke's movement is temporarily stopped. This is because mouse-motion events go to the component that's deepest in the containment hierarchy and is interested in mouse events. The mouse-motion listener that moves Duke is registered on the layered pane, and most of the components in that pane (with the exception of the labels) happen to have mouse-motion listeners. When the mouse moves over an interested component in the layered pane, the layered pane doesn't get the event and the interested component does.</li>
<li>Making sure the Top Position in Layer check box is selected, change Duke's layer to Yellow (-30000).<br />
As before, he appears on top of other components, except for the Magenta (0) and Cyan (301) rectangles.</li>
<li>Keeping Duke in the Yellow layer, click the check box to send Duke to the back of layer -30000.<br />
Duke disappears because the content pane and all the components in it are now above him.</li>
<li>Change Duke's layer to Cyan (301), move Duke down a bit so he's standing on the top edge of the Yellow rectangle, and then press Space to bring up the combo box's drop-down list.<br />
If the look and feel implements the drop-down list as a lightweight popup, Duke appears on top of the drop-down list.</li>

## <a name="api" id="api">The Root Pane API</a>

The tables that follow list the API for using root panes, glass panes, and content panes. For more information on using content panes, go to [Using Top-Level Containers](toplevel.html). Here are the tables in this section:

- [Using a Root Pane](#rootpaneapi)
- [Setting or Getting the Root Pane's Contents](#contentapi)

The API for using other parts of the root pane is described elsewhere:

- [The Layered Pane API](layeredpane.html#api)
- [The Menu API](menu.html#api)
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[JRootPane getRootPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getRootPane--)<br />**(in `JApplet`, `JDialog`, `JFrame`, `JInternalFrame`, and `JWindow`)**</td><td headers="h102">Get the root pane of the applet, dialog, frame, internal frame, or window.</td>
<td headers="h101">[static JRootPane getRootPane(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingUtilities.html#getRootPane-java.awt.Component-)<br />**(in `SwingUtilities`)**</td><td headers="h102">If the component contains a root pane, return that root pane. Otherwise, return the root pane (if any) that contains the component.</td>
<td headers="h101">[JRootPane getRootPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#getRootPane--)<br />**(in `JComponent`)**</td><td headers="h102">Invoke the `SwingUtilities` `getRootPane` method for the `JComponent`.</td>
<td headers="h101">[void setDefaultButton(JButton)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRootPane.html#setDefaultButton-javax.swing.JButton-)<br />[JButton getDefaultButton()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JRootPane.html#getDefaultButton--)</td><td headers="h102">Set or get which button (if any) is the default button in the root pane. A look-and-feel-specific action, such as pressing Enter, causes the button's action to be performed.</td>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[void setGlassPane(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setGlassPane-java.awt.Component-)<br />[Component getGlassPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getGlassPane--)</td><td headers="h202">Set or get the glass pane.</td>
<td headers="h201">[void setLayeredPane(JLayeredPane)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setLayeredPane-javax.swing.JLayeredPane-)<br />[Container getLayeredPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getLayeredPane--)</td><td headers="h202">Set or get the layered pane.</td>
<td headers="h201">[void setContentPane(Container)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setContentPane-java.awt.Container-)<br />[Container getContentPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getContentPane--)</td><td headers="h202">Set or get the content pane.</td>
<td headers="h201">[void setJMenuBar(JMenuBar)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#setJMenuBar-javax.swing.JMenuBar-)<br />[JMenuBar getJMenuBar()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JFrame.html#getJMenuBar--)<br />**(not defined in `JWindow`)**</td><td headers="h202">Set or get the menu bar.</td>




## <a name="eg" id="eg">Examples that Use Root Panes</a>

Every Swing program has a root pane, but few reference it directly. The examples in the following list illustrate how to use features of `JRootPane` or the glass pane. Also see these lists:

- [Examples that Use Layered Panes](layeredpane.html#eg)
- [Examples that Use Menus](menu.html#eg)
- [Examples that Use Frames](frame.html#eg) (for examples of using content panes)
<th id="h301" align="left">Example</th><th id="h302" align="left">Where Described</th><th id="h303" align="left">Notes</th>
<td headers="h301">[`GlassPaneDemo`](../examples/components/index.html#GlassPaneDemo)</td><td headers="h302">This section</td><td headers="h303">Uses a glass pane that paints a bit and redispatches events.</td>
<td headers="h301">[`RootLayeredPaneDemo`](../examples/components/index.html#RootLayeredPaneDemo)</td><td headers="h302">This section</td><td headers="h303">Adapts LayeredPaneDemo to use the root pane's layered pane.</td>
<td headers="h301">[`ListDialog`](../examples/components/index.html#ListDialog)</td><td headers="h302">[How to Use Lists](list.html)</td><td headers="h303">Sets the default button for a `JDialog`.</td>
<td headers="h301">[`FrameDemo2`](../examples/components/index.html#FrameDemo2)</td><td headers="h302">[How to Make Frames](frame.html)</td><td headers="h303">Sets the default button for a `JFrame`.</td>
