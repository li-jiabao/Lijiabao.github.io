
# How to Use Scroll Panes

A 
[`JScrollPane`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html) provides a scrollable view of a component. When screen real estate is limited, use a scroll pane to display a component that is large or one whose size can change dynamically. Other containers used to save screen space include [split panes](splitpane.html) and [tabbed panes](tabbedpane.html).

The code to create a scroll pane can be minimal. For example, here's a picture of a demo program that puts a text area in a scroll pane because the text area's size grows dynamically as text is appended to it: 


Here's the code that creates the text area, makes it the scroll pane's client, and adds the scroll pane to a container:

```

**//In a container that uses a BorderLayout:**
textArea = new JTextArea(5, 30);
...
**JScrollPane scrollPane = new JScrollPane(textArea);**
...
setPreferredSize(new Dimension(450, 110));
...
add(scrollPane, BorderLayout.CENTER);

```

The boldface line of code creates the `JScrollPane`, specifying the text area as the scroll pane's client. The program doesn't invoke any methods on the `JScrollPane` object, since the scroll pane handles everything automatically: creating the scroll bars when necessary, redrawing the client when the user moves the scroll knobs, and so on.

You might have noticed that the preceding code sets the preferred size of the scroll pane's container. In the Java look and feel, this preferred size happens to be a bit less tall than required for the text area to display the 5 rows that we requested when creating it, so the scroll bar initially displays a vertical scroll bar. If we didn't restrict the size of the scroll pane's container, the scroll pane would be big enough for the text area to display the full 5 rows and 30 columns specified with the `JTextArea` constructor. Refer to [Sizing a Scroll Pane](#sizing) for information about techniques for making a scroll pane the size you want.

The rest of this section discusses the following topics:

- [How a Scroll Pane Works](#operation)
- [Setting the Scroll Bar Policy](#scrollbars)
- [Providing Custom Decorations](#decorations)
- [Implementing a Scrolling-Savvy Client](#scrollable)
- [Sizing a Scroll Pane](#sizing)
- [Dynamically Changing the Client's Size](#update)
- [The Scroll Pane API](#api)
- [Examples that Use Scroll Panes](#eg)

<a name="operation" id="operation"></a>

## <a name="operation__1" id="operation__1">How a Scroll Pane Works</a>

Here is a snapshot of an application that uses a customized scroll pane to view a photograph:

The scroll pane in this application looks very different from the one in the previous demo program. Rather than displaying text, this scroll pane contains a image. The scroll pane also has two scroll bars, a row header, a column header, and four corners, three of which have been customized. <!-- ***************** boilerplate stuff **************** -->

<li>Click the Launch button to run ScrollDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/components/index.html#ScrollDemo).[<img src="../../images/jws-launch-button.png" width="88" height="23" align="bottom" alt="Launches the ScrollDemo example" />](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ScrollDemoProject/ScrollDemo.jnlp)<br />
<p><!--  ******* end boilerplate stuff  *******  -->
 <!-- ======================================== -->
 <!-- 

<hr />**Try this:**&nbsp;
<ol>
<li> [Run ScrollDemo](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ScrollDemoProject/ScrollDemo.jnlp) (
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).    Or, to compile and run the example yourself,
     consult the
     [example index](../examples/components/index.html#ScrollDemo).
--></p>
</li>
1. Move the knobs on the scroll bars. Watch the image scroll and the horizontal and vertical rulers scroll along.
1. If you have a mouse with a wheel (which is generally between the mouse buttons) use the mouse wheel to scroll the image vertically.
1. Click the **cm** toggle in the upper left corner of the scroll pane. The units on the row and column headers change to inches (or back to centimeters).
1. Click the arrow buttons on the scroll bars. Also, try clicking on the track above or below the knob on the vertical scroll bar, or to the left or right of the horizontal one.
1. Move the cursor over the image and press the cursor. Continuing to press the cursor, drag to a point outside the image and pause. The visible area of the image moves toward the cursor. This scroll-by-dragging functionality is enabled by the scroll pane, and with the `JComponent` API, but it is implemented by the custom component that displays the image.
1. Resize the window. Notice that the scroll bars disappear when the scroll pane is large enough to display the entire image and reappear again when the scroll pane is too small to show the entire image.

The ScrollDemo program establishes the scroll pane's client when creating the scroll pane:

```

//**Where the member variables are declared:**
private ScrollablePicture picture;
...
//**Where the GUI is created:**
picture = new ScrollablePicture(** ... **);
JScrollPane pictureScrollPane = new JScrollPane(picture);

```

The scroll pane's client is also known as the **view** or **viewport view**. You can change the client dynamically by calling the `setViewportView` method. Note that `JScrollPane` has no corresponding `getViewportView` method. If you need to refer to the client object again, you can either cache it in a variable or invoke `getViewport().getViewportView()` on the scroll pane.

When the user manipulates the scroll bars in a scroll pane, the area of the client that is visible changes accordingly. This picture shows the relationship between the scroll pane and its client and indicates the classes that the scroll pane commissions to help:

A scroll pane uses a 
[`JViewport`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JViewport.html) instance to manage the visible area of the client. The viewport is responsible for positioning and sizing the client, based on the positions of the scroll bars, and displaying it.

A scroll pane may use two separate instances of 
[`JScrollBar`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollBar.html) for the scroll bars. The scroll bars provide the interface for the user to manipulate the visible area. The following figure shows the three areas of a scroll bar: the knob (sometimes called the **thumb**), the (arrow) buttons, and the track.

When the user moves the knob on the vertical scroll bar up and down, the visible area of the client moves up and down. Similarly, when the user moves the knob on the horizontal scroll bar to the right and left, the visible area of the client moves back and forth accordingly. The position of the knob relative to its track is proportionally equal to the position of the visible area relative to the client. In the Java look and feel and some others, the size of the knob gives a visual clue as to how much of the client is visible.

By clicking an arrow button, the user can scroll by a **unit increment**. By clicking within the track, the user can scroll by a **block increment**. If the user has a mouse with a wheel, then the user can scroll vertically using the mouse wheel. The amount that the mouse wheel scrolls is platform dependent. For example, by default on Windows XP, the mouse wheel scrolls three unit increments; the Mouse control panel allows you to specify a different number of unit increments or to use a block increment instead. More information about unit and block increments is in [Implementing a Scrolling-Savvy Client](#scrollable).

Typical programs don't directly instantiate or call methods on a viewport or scroll bar. Instead, programs achieve their scrolling behavior using the `JScrollPane` API and the API discussed in [Implementing a Scrolling-Savvy Client](#scrollable). Some scrolling-savvy components such as `JList`, `JTable`, and `JTree` also provide [additional API](#otherAPI) to help you affect their scrolling behavior.

## <a name="scrollbars" id="scrollbars">Setting the Scroll Bar Policy</a>

On startup, the scroll pane in the `ScrollDemo` application has two scroll bars. If you make the window large, both scroll bars disappear because they are no longer needed. If you then shrink the height of the window without changing its width, the vertical scroll bar reappears. Further experimentation will show that in this application both scroll bars disappear and reappear as needed. This behavior is controlled by the scroll pane's **scroll bar policy**, Actually, it's two policies: each scroll bar has its own.

`ScrollDemo` doesn't explicitly set the scroll pane's scroll bar policies &#151; it uses the default. You can set the policies when you create the scroll pane or change them dynamically.

Of the constructors provided by `JScrollPane`, these two let you set the scroll bar policies when you create the scroll pane:

```

JScrollPane(Component, int, int)
JScrollPane(int, int)

```

The first `int` specifies the policy for the vertical scroll bar; the second specifies the policy for the horizontal scroll bar. You can also set the policies dynamically with the `setHorizontalScrollBarPolicy` and `setVerticalScrollBarPolicy` methods. With both the constructors and the methods, use one of the following constants defined in the 
[`ScrollPaneConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ScrollPaneConstants.html) interface (which is implemented by `JScrollPane`):
<th id="h1" align="left">Policy</th><th id="h2" align="left">Description</th>
<td headers="h1" valign="top">`VERTICAL_SCROLLBAR_AS_NEEDED`<br />`HORIZONTAL_SCROLLBAR_AS_NEEDED`</td><td headers="h2" valign="top">The default. The scroll bar appears when the viewport is smaller than the client and disappears when the viewport is larger than the client.</td>
<td headers="h1" valign="top">`VERTICAL_SCROLLBAR_ALWAYS`<br />`HORIZONTAL_SCROLLBAR_ALWAYS`</td><td headers="h2" valign="top">Always display the scroll bar. The knob disappears if the viewport is large enough to show the whole client.</td>
<td headers="h1" valign="top">`VERTICAL_SCROLLBAR_NEVER`<br />`HORIZONTAL_SCROLLBAR_NEVER`</td><td headers="h2" valign="top">Never display the scroll bar. Use this option if you don't want the user to directly control what part of the client is shown, or if you want them to use only non-scroll-bar techniques (such as dragging).</td>

## <a name="decorations" id="decorations">Providing Custom Decorations</a>

The area drawn by a scroll pane consists of up to nine parts: the center, four sides, and four corners. The center is the only component that is always present in all scroll panes. Besides scroll bars, the sides can contain column and row headers. A corner component is visible only if both sides that intersect at that corner contain visible components.

As shown in the figure, the scroll pane in `ScrollDemo` has custom row and column headers. Additionally, because all four sides are populated, all four corners are present. The program customizes three of the corners &#151; two just fill their area with the same color as the `Rule`s, and the other contains a toggle button. The fourth corner, the lower right corner, is the default provided by the scroll pane. Notice that because the row and column headers are always present in this example, the toggle button is also always present.

If a corner contains a control that the user needs access to all the time, make sure the sides that intersect at the corner are always present. For example, if this application placed the toggle in the lower right corner where the scroll bars intersect, then the toggle would disappear if the user resized the window and even one of the scroll bars disappeared.

The scroll pane's row and column headers are provided by a custom `JComponent` subclass, 
[`Rule`](../examples/components/ScrollDemoProject/src/components/Rule.java), that draws a ruler in centimeters or inches. Here's the code that creates and sets the scroll pane's row and column headers:

```

**//Where the member variables are defined:**
private Rule columnView;
private Rule rowView;
...
**//Where the GUI is initialized:**
ImageIcon bee = createImageIcon("images/flyingBee.jpg");
...
//Create the row and column headers.
columnView = new Rule(Rule.HORIZONTAL, true);
rowView = new Rule(Rule.VERTICAL, true);

...
**pictureScrollPane.setColumnHeaderView(columnView);**
**pictureScrollPane.setRowHeaderView(rowView);**

```

You can use any component for a scroll pane's row and column headers. The scroll pane puts the row and column headers in `JViewPort`s of their own. Thus, when scrolling horizontally, the column header follows along, and when scrolling vertically, the row header follows along. Make sure the row and column have the same width and height as the view, because JScrollPane does not enforce these values to have the same size. If one differs from the other, you are likely to not get the desired behavior.

As a `JComponent` subclass, our custom `Rule` class puts its rendering code in its `paintComponent` method. The `Rule` rendering code takes care to draw only within the current clipping bounds, to ensure speedy scrolling. Your custom row and column headers should do the same.

You can also use any component for the corners of a scroll pane. `ScrollDemo` illustrates this by putting a toggle button in the upper left corner, and custom 
[`Corner`](../examples/components/ScrollDemoProject/src/components/Corner.java) objects in the upper right and lower left corners. Here's the code that creates the `Corner` objects and calls `setCorner` to place them:

```

//Create the corners.
JPanel buttonCorner = new JPanel(); //use FlowLayout
isMetric = new JToggleButton("cm", true);
isMetric.setFont(new Font("SansSerif", Font.PLAIN, 11));
isMetric.setMargin(new Insets(2,2,2,2));
isMetric.addItemListener(this);
buttonCorner.add(isMetric);
...
//Set the corners.
<strong>pictureScrollPane.setCorner(JScrollPane.UPPER_LEFT_CORNER,
                            buttonCorner);
pictureScrollPane.setCorner(JScrollPane.LOWER_LEFT_CORNER,
                            new Corner());
pictureScrollPane.setCorner(JScrollPane.UPPER_RIGHT_CORNER,
                            new Corner());</strong>

```

Remember that the size of each corner is determined by the size of the sides intersecting there. For some components you must take care that the specific instance of the component fits in its corner. For example, the program sets the font and margins on the toggle button so that it fits within the space established by the headers. It's not an issue with the `Corner` class because that class colors its entire bounds, whatever they happen to be, with a solid color.

As you can see from the code, constants indicate the corner positions. This figure shows the constant for each position:

The constants are defined in the 
[`ScrollPaneConstants`](https://docs.oracle.com/javase/8/docs/api/javax/swing/ScrollPaneConstants.html) interface, which `JScrollPane` implements.

## <a name="scrollable" id="scrollable">Implementing a Scrolling-Savvy Client</a>

To customize the way that a client component interacts with its scroll pane, you can make the component implement the 
[`Scrollable`](https://docs.oracle.com/javase/8/docs/api/javax/swing/Scrollable.html) interface. By implementing `Scrollable`, a client can specify both the size of the viewport used to view it and the amount to scroll for clicks on the different controls on a scroll bar. You can also specify if the view should track the size of the viewport. This is typically used when the viewport is bigger than the view, but the view should fill the available space.

```

scrollPane.getVerticalScrollBar().setUnitIncrement(10);

```

Here again are the three control areas of a scroll bar: the knob, the buttons, and the track.

You might have noticed when manipulating the scroll bars in `ScrollDemo` that clicking the buttons scrolls the image to a tick boundary. You might also have noticed that clicking in the track scrolls the picture by a "screenful". More generally, the button scrolls the visible area by a unit increment and the track scrolls the visible area by a block increment. The behavior you see in the example is not the scroll pane's default behavior, but is specified by the client in its implementation of the `Scrollable` interface.

The client for the `ScrollDemo` program is 
[`ScrollablePicture`](../examples/components/ScrollDemoProject/src/components/ScrollablePicture.java). `ScrollablePicture` is a subclass of `JLabel` that provides implementations of all five `Scrollable` methods:

- `getScrollableBlockIncrement`
- `getScrollableUnitIncrement`
- `getPreferredScrollableViewportSize`
- `getScrollableTracksViewportHeight`
- `getScrollableTracksViewportWidth`

`ScrollablePicture` implements the `Scrollable` interface primarily to affect the unit and block increments. However, it must provide implementations for all five methods. Thus, it provides reasonable defaults for the other three methods that you might want to copy for your scrolling-savvy classes.

The scroll pane calls the client's `getScrollableUnitIncrement` method whenever the user clicks one of the buttons on the scroll bar. This is true as long as the client implements Scrollable. This method returns the number of pixels to scroll. An obvious implementation of this method returns the number of pixels between tick marks on the header rulers. `ScrollablePicture`, however, does something different: It returns the value required to position the image on a tick mark boundary. Here's the implementation:

```

public int getScrollableUnitIncrement(Rectangle visibleRect,
                                      int orientation,
                                      int direction) {
    //Get the current position.
    int currentPosition = 0;
    if (orientation == SwingConstants.HORIZONTAL) {
        currentPosition = visibleRect.x;
    } else {
        currentPosition = visibleRect.y;
    }

    //Return the number of pixels between currentPosition
    //and the nearest tick mark in the indicated direction.
    if (direction &lt; 0) {
        int newPosition = currentPosition -
                         (currentPosition / maxUnitIncrement)
                          * maxUnitIncrement;
        return (newPosition == 0) ? maxUnitIncrement : newPosition;
    } else {
        return ((currentPosition / maxUnitIncrement) + 1)
                 * maxUnitIncrement
                 - currentPosition;
    }
}

```

If the image is already on a tick mark boundary, this method returns the number of pixels between ticks. Otherwise, it returns the number of pixels from the current location to the nearest tick.

Likewise, the scroll pane calls the client's `getScrollableBlockIncrement` method each time the user clicks on the track, but only if the client implements Scrollable. Here's `ScrollablePicture`'s implementation of this method:

```

public int getScrollableBlockIncrement(Rectangle visibleRect,
                                       int orientation,
                                       int direction) {
    if (orientation == SwingConstants.HORIZONTAL)
        return visibleRect.width - maxUnitIncrement;
    else
        return visibleRect.height - maxUnitIncrement;
}

```

This method returns the height of the visible rectangle minus a tick mark. This behavior is typical, but true if scrolling vertically, otherwise, it's the width.A block increment should be slightly smaller than the viewport to leave a little of the previous visible area for context. For example, a text area might leave one or two lines of text for context and a table might leave a row or column (depending on the scroll direction).

`ScrollablePicture.java` has one more bit of code that's not required by the `Scrollable` interface, but is common in scrollable components: a mouse motion listener that lets the user scroll the picture by dragging from it. The boldface code in the following snippet implements scrolling by dragging:

```

public class ScrollablePicture extends JLabel
                               implements Scrollable,
                                          **MouseMotionListener** {
    ...
    public ScrollablePicture(...) {
        ...
        **setAutoscrolls(true);** //enable synthetic drag events
        **addMouseMotionListener(this);** //handle mouse drags
    }
    ...
    **public void mouseDragged(MouseEvent e) {**
        //The user is dragging us, so scroll!
        <b>Rectangle r = new Rectangle(e.getX(), e.getY(), 1, 1);
        scrollRectToVisible(r);
    }</b>
...
}

```

This snippet scrolls the picture whenever the user drags from the picture to a location outside the picture and pauses. The `setAutoscrolls` method is defined by `JComponent` for the purpose of assisting &#151; but not implementing &#151; scrolling by dragging. Setting the autoscrolls property to `true` makes the component fire synthetic mouse-dragged events even when the mouse isn't moving (because it stopped, mid-drag, outside the component). It's up to the component's mouse motion listener to listen for these events and react accordingly.

## <a name="sizing" id="sizing">Sizing a Scroll Pane</a>

Unless you explicitly set a scroll pane's preferred size, the scroll pane computes it based on the preferred size of its nine components (the viewport, and, if present, the two scroll bars, the row and column headers, and the four corners). The largest factor, and the one most programmers care about, is the size of the viewport used to display the client.

If the client is not scrolling-savvy, then the scroll pane sizes itself so that the client displays at its preferred size. For typical unsavvy clients, this makes the scroll pane redundant. That is, the scroll pane has no scroll bars because the client's preferred size is big enough to display the entire client. In this case, if the client doesn't change size dynamically, you should probably limit the size of the scroll pane by setting its preferred size or the preferred size of its container.

If the client is scrolling-savvy, then the scroll pane uses the value returned by the client's `getPreferredScrollableViewportSize` method to compute the size of its viewport. Implementations of this method generally report a preferred size for scrolling that's smaller than the component's standard preferred size. For example, by default, the value returned by `JList`'s implementation of `getPreferredScrollableViewportSize` is just big enough to display eight rows.

Scrolling-savvy classes, like [lists](list.html), [tables](table.html), [text components](text.html), and [trees](tree.html), often provide one or more methods that let programmers affect the size returned from `getPreferredScrollableViewportSize`. For example, you can set the number of visible rows in a list or a tree by calling the `setVisibleRowCount` method. The list or tree takes care of figuring out the size needed to display that number of rows.

Refer to [Methods in Other Classes Related to Scrolling](#otherAPI) for information about scrolling-related methods provided by classes other than `JScrollPane`. And remember &#151; if you don't like the value that `getPreferredScrollableViewportSize` returns, you can always set the preferred size of the scroll pane or its container.

## <a name="update" id="update">Dynamically Changing the Client's Size</a>

Changing the size of a scroll pane's client is a two-step process. First, set the client's preferred size. Then, call `revalidate` on the client to let the scroll pane know that it should update itself and its scroll bars. Let's look at an example.

Here's a picture of an application that changes the client's size whenever the user places a circle whose bounds fall outside of the client's current bounds. The program also changes the client's size when the user clears the drawing area:

You can find the full source code for this example in 
[`ScrollDemo2.java`](../examples/components/ScrollDemo2Project/src/components/ScrollDemo2.java), which is based on an example provided by tutorial reader John Vella. You can [run ScrollDemo2](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/ScrollDemo2Project/ScrollDemo2.jnlp) ( 
[download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)).

Here's the code that changes the drawing area's size when necessary:

```

if (changed) {
    //Update client's preferred size because
    //the area taken up by the graphics has
    //gotten larger or smaller (if cleared).
    drawingArea.setPreferredSize(**/* the new size */**);

    //Let the scroll pane know to update itself
    //and its scroll bars.
    drawingArea.revalidate();
}

```

Note that when the client changes size, the scroll bars adjust. The scroll pane doesn't resize, nor does the viewport.

Refer to [`SplitPaneDemo`](../examples/components/index.html#SplitPaneDemo) for another example in which the client object changes size.

## <a name="api" id="api">The Scroll Pane API</a>

The following tables list the commonly used scroll-related constructors and methods. Other methods you are most likely to invoke on a `JScrollPane` object are those such as `setPreferredSize` that its superclasses provide. See [The JComponent API](jcomponent.html#api) for tables of commonly used inherited methods.

The API for using scroll panes falls into these categories:

- [Setting Up the Scroll Pane](#setup)
- [Decorating the Scroll Pane](#decorationsAPI)
- [Implementing a Scrolling-Savvy Client](#scrollableAPI)
- [Methods in Other Classes Related to Scrolling](#otherAPI)
<th id="h101" align="left">Method or Constructor</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[JScrollPane()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#JScrollPane--)<br />[JScrollPane(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#JScrollPane-java.awt.Component-)<br />[JScrollPane(int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#JScrollPane-int-int-)<br />[JScrollPane(Component, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#JScrollPane-java.awt.Component-int-int-)</td><td headers="h102">Create a scroll pane. The `Component` parameter, when present, sets the scroll pane's client. The two `int` parameters, when present, set the vertical and horizontal scroll bar policies (respectively).</td>
<td headers="h101">[`void setViewportView(Component)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#setViewportView-java.awt.Component-)</td><td headers="h102">Set the scroll pane's client.</td>
<td headers="h101">[`void setVerticalScrollBarPolicy(int)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#setVerticalScrollBarPolicy-int-)<br />[`int getVerticalScrollBarPolicy()`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#getVerticalScrollBarPolicy--)</td><td headers="h102">Set or get the vertical scroll policy. `ScrollPaneConstants` defines three values for specifying this policy: `VERTICAL_SCROLLBAR_AS_NEEDED` (the default), `VERTICAL_SCROLLBAR_ALWAYS`, and `VERTICAL_SCROLLBAR_NEVER`.</td>
<td headers="h101">[void setHorizontalScrollBarPolicy(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#setHorizontalScrollBarPolicy-int-)<br />[int getHorizontalScrollBarPolicy()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#getHorizontalScrollBarPolicy--)</td><td headers="h102">Set or get the horizontal scroll policy. `ScrollPaneConstants` defines three values for specifying this policy: `HORIZONTAL_SCROLLBAR_AS_NEEDED` (the default), `HORIZONTAL_SCROLLBAR_ALWAYS`, and `HORIZONTAL_SCROLLBAR_NEVER`.</td>
<td headers="h101">[void setViewportBorder(Border)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#setViewportBorder-javax.swing.border.Border-)<br />[Border getViewportBorder()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#getViewportBorder--)</td><td headers="h102">Set or get the border around the viewport.This is preferred over setting the border on the component.</td>
<td headers="h101">[boolean isWheelScrollingEnabled()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#isWheelScrollingEnabled--)</td><td headers="h102">Set or get whether scrolling occurs in response to the mouse wheel. Mouse-wheel scrolling is enabled by default.</td>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[void setColumnHeaderView(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#setColumnHeaderView-java.awt.Component-)<br />[void setRowHeaderView(Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#setRowHeaderView-java.awt.Component-)</td><td headers="h202">Set the column or row header for the scroll pane.</td>
<td headers="h201">[void setCorner(String, Component)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#setCorner-java.lang.String-java.awt.Component-)<br />[Component getCorner(String)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JScrollPane.html#getCorner-java.lang.String-)</td><td headers="h202">Set or get the corner specified. The `int` parameter specifies which corner and must be one of the following constants defined in `ScrollPaneConstants`: `UPPER_LEFT_CORNER`, `UPPER_RIGHT_CORNER`, `LOWER_LEFT_CORNER`, `LOWER_RIGHT_CORNER`, `LOWER_LEADING_CORNER`, `LOWER_TRAILING_CORNER`, `UPPER_LEADING_CORNER`, and `UPPER_TRAILING_CORNER`.</td>
<th id="h301" align="left">Method</th><th id="h302" align="left">Purpose</th>
<td headers="h301">[int getScrollableUnitIncrement(Rectangle, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/Scrollable.html#getScrollableUnitIncrement-java.awt.Rectangle-int-int-)<br />[int getScrollableBlockIncrement(Rectangle, int, int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/Scrollable.html#getScrollableBlockIncrement-java.awt.Rectangle-int-int-)<br />**(required by the `Scrollable` interface)**</td><td headers="h302">Get the unit or block increment in pixels. The `Rectangle` parameter is the bounds of the currently visible rectangle. The first `int` parameter is either `SwingConstants.HORIZONTAL` or `SwingConstants.VERTICAL` depending on what scroll bar the user clicked on. The second `int` parameter indicates which direction to scroll. A value less than 0 indicates up or left. A value greater than 0 indicates down or right.</td>
<td headers="h301">[Dimension getPreferredScrollableViewportSize()](https://docs.oracle.com/javase/8/docs/api/javax/swing/Scrollable.html#getPreferredScrollableViewportSize--)<br />**(required by the `Scrollable` interface)**</td><td headers="h302">Get the preferred size of the viewport. This allows the client to influence the size of the viewport in which it is displayed. If the viewport size is unimportant, implement this method to return `getPreferredSize`.</td>
<td headers="h301">[boolean getScrollableTracksViewportWidth()](https://docs.oracle.com/javase/8/docs/api/javax/swing/Scrollable.html#getScrollableTracksViewportWidth--)<br />[boolean getScrollableTracksViewportHeight()](https://docs.oracle.com/javase/8/docs/api/javax/swing/Scrollable.html#getScrollableTracksViewportHeight--)<br />**(required by the `Scrollable` interface)**</td><td headers="h302">Get whether the scroll pane should force the client to be the same width or height as the viewport. A return value of `true` from either of these methods effectively disallows horizontal or vertical scrolling (respectively).</td>
<td headers="h301">[void setAutoscrolls(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setAutoscrolls-boolean-)<br />**(in `JComponent`)**</td><td headers="h302">Set whether synthetic mouse dragged events should be generated when the user drags the mouse outside of the component and stops; these events are necessary for scrolling by dragging. By default, the value is `false`, but many scrollable components such as `JTable` and custom components set the value to `true`.</td>
<th id="h401" align="left">Method</th><th id="h402" align="left">Purpose</th>
<td headers="h401">[void scrollRectToVisible(Rectangle)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#scrollRectToVisible-java.awt.Rectangle-)<br />**(in `JComponent`)**</td><td headers="h402">If the component is in a container that supports scrolling, such as a scroll pane, then calling this method scrolls the scroll pane such that the specified rectangle is visible.</td>
<td headers="h401">[void setVisibleRowCount(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#setVisibleRowCount-int-)<br />[int getVisibleRowCount()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#getVisibleRowCount--)<br />**(in `JList`)**</td><td headers="h402">Set or get how many rows of the list are visible. The `getPreferredScrollableViewportSize` method uses the visible row count to compute its return value.</td>
<td headers="h401">[void ensureIndexIsVisible(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JList.html#ensureIndexIsVisible-int-)<br />**(in `JList`)**</td><td headers="h402">Scroll so that the row at the specified index is visible. This method calls `scrollRectToVisible` and works only if the list is in a container, such as a scroll pane, that supports scrolling.</td>
<td headers="h401">[void setVisibleRowCount(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#setVisibleRowCount-int-)<br />[int getVisibleRowCount()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#getVisibleRowCount--)<br />**(in `JTree`)**</td><td headers="h402">Set or get how many rows of the tree are visible. The `getPreferredScrollableViewportSize` method uses the visible row count to compute its return value.</td>
<td headers="h401">[void scrollPathToVisible(TreePath)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#scrollPathToVisible-javax.swing.tree.TreePath-)<br />[void scrollRowToVisible(int)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#scrollRowToVisible-int-)<br />**(in `JTree`)**</td><td headers="h402">Scroll so that the specified tree path or row at the specified index is visible. These methods call `scrollRectToVisible` and work only if the tree is in a container, such as a scroll pane, that supports scrolling.</td>
<td headers="h401">[void setScrollsOnExpand(boolean)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#setScrollsOnExpand-boolean-)<br />[boolean getScrollsOnExpand()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTree.html#getScrollsOnExpand--)<br />**(in `JTree`)**</td><td headers="h402">Set or get whether scrolling occurs automatically when the user expands a node. True by default. This feature works only when the tree is in a container, such as a scroll pane, that supports scrolling.</td>
<td headers="h401">[void setPreferredScrollableViewportSize(Dimension)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JTable.html#setPreferredScrollableViewportSize-java.awt.Dimension-)<br />**(in `JTable`)**</td><td headers="h402">Set the value to be returned by `getPreferredScrollableViewportSize`.</td>

## <a name="eg" id="eg">Examples that Use Scroll Panes</a>

This table shows the examples that use `JScrollPane` and where those examples are described.
<th id="h501" align="left">Example</th><th id="h502" align="left">Where Described</th><th id="h503" align="left">Notes</th>
<td headers="h501">[`ToolBarDemo`](../examples/components/index.html#ToolBarDemo)</td><td headers="h502">This section,<br />[How to Use Tool Bars](toolbar.html)</td><td headers="h503">Shows a simple, yet typical, use of a scroll pane.</td>
<td headers="h501">[`ScrollDemo`](../examples/components/index.html#ScrollDemo)</td><td headers="h502">This section</td><td headers="h503">Uses many of scroll pane's bells and whistles.</td>
<td headers="h501">[`ScrollDemo2`](../examples/components/index.html#ScrollDemo2)</td><td headers="h502">This section</td><td headers="h503">Shows how to change the client's size.</td>
<td headers="h501">[`SplitPaneDemo`](../examples/components/index.html#SplitPaneDemo)</td><td headers="h502">[How to Use Split Panes](splitpane.html),<br />[How to Use Lists](list.html)</td><td headers="h503">Puts a list and a label in a scroll pane. Also, shows how to handle the case when a scroll pane's client changes size.</td>
<td headers="h501">[`TableDemo`](../examples/components/index.html#TableDemo)</td><td headers="h502">[How to Use Tables](table.html)</td><td headers="h503">Puts a table in a scroll pane.</td>
<td headers="h501">[`TextSamplerDemo`](../examples/components/index.html#TextSamplerDemo)</td><td headers="h502">[Using Text Components](text.html)</td><td headers="h503">Puts a text area, an editor pane, and a text pane each in a scroll pane.</td>
<td headers="h501">[`TreeDemo`](../examples/components/index.html#TreeDemo)</td><td headers="h502">[How to Use Trees](tree.html)</td><td headers="h503">Puts a tree in a scroll pane.</td>

If you are programming in JavaFX, see
[Scroll Pane](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/scrollpane.htm).
