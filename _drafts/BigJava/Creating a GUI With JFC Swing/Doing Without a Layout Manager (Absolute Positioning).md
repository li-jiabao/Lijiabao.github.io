
# Doing Without a Layout Manager (Absolute Positioning)

Although it is possible to do without a layout manager, you should use a layout manager if at all possible. A layout manager makes it easier to adjust to look-and-feel-dependent component appearances, to different font sizes, to a container's changing size, and to different locales. Layout managers also can be reused easily by other containers, as well as other programs.

If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

If a container holds components whose size is not affected by the container's size or by font, look-and-feel, or language changes, then absolute positioning might make sense. Desktop panes, which contain 
[internal frames](../components/internalframe.html), are in this category. The size and position of internal frames does not depend directly on the desktop pane's size. The programmer determines the initial size and placement of internal frames within the desktop pane, and then the user can move or resize the frames. A layout manager is unnecessary in this situation.

Another situation in which absolute positioning might make sense is that of a custom container that performs size and position calculations that are particular to the container, and perhaps require knowledge of the container's specialized state. This is the situation with 
[split panes](../components/splitpane.html).

Creating a container without a layout manager involves the following steps.

1. Set the container's layout manager to null by calling `setLayout(null)`.
1. Call the `Component` class's `setbounds` method for each of the container's children.
1. Call the `Component` class's `repaint` method.

However, creating containers with absolutely positioned containers can cause problems if the window containing the container is resized.

Here is a snapshot of a frame whose content pane uses absolute positioning.

Click the Launch button to run AbsoluteLayoutDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/layout/index.html#AbsoluteLayoutDemo).

Its code is in 
[`AbsoluteLayoutDemo.java`](../examples/layout/AbsoluteLayoutDemoProject/src/layout/AbsoluteLayoutDemo.java). The following code snippet shows how the components in the content pane are created and laid out.

```

pane.setLayout(null);

JButton b1 = new JButton("one");
JButton b2 = new JButton("two");
JButton b3 = new JButton("three");

pane.add(b1);
pane.add(b2);
pane.add(b3);

Insets insets = pane.getInsets();
Dimension size = b1.getPreferredSize();
b1.setBounds(25 + insets.left, 5 + insets.top,
             size.width, size.height);
size = b2.getPreferredSize();
b2.setBounds(55 + insets.left, 40 + insets.top,
             size.width, size.height);
size = b3.getPreferredSize();
b3.setBounds(150 + insets.left, 15 + insets.top,
             size.width + 50, size.height + 20);

...**//In the main method:**
Insets insets = frame.getInsets();
frame.setSize(300 + insets.left + insets.right,
              125 + insets.top + insets.bottom);


```
