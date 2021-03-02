
# How to Use BorderLayout

If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

The following figure represents a snapshot of an application that uses the 
[`BorderLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/BorderLayout.html) class.

Click the Launch button to run BorderLayoutDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/layout/index.html#BorderLayoutDemo).

The complete code of this demo is in the 
[`BorderLayoutDemo.java`](../examples/layout/BorderLayoutDemoProject/src/layout/BorderLayoutDemo.java) file.

As the preceding picture shows, a `BorderLayout` object has five areas. These areas are specified by the `BorderLayout` constants:

- `PAGE_START`
- `PAGE_END`
- `LINE_START`
- `LINE_END`
- `CENTER`

Before JDK release 1.4, the preferred names for the various areas were different, ranging from points of the compass (for example, `BorderLayout.NORTH` for the top area) to wordier versions of the constants we use in our examples. The constants our examples use are preferred because they are standard and enable programs to adjust to languages that have different orientations.

If the window is enlarged, the center area gets as much of the available space as possible. The other areas expand only as much as necessary to fill all available space. Often a container uses only one or two of the areas of the `BorderLayout` object &#151; just the center, or the center and the bottom.

The following code adds components to a frame's content pane. Because content panes use the `BorderLayout` class by default, the code does not need to set the layout manager. The complete program is in the 
[`BorderLayoutDemo.java`](../examples/layout/BorderLayoutDemoProject/src/layout/BorderLayoutDemo.java) file.

```

...**//Container pane = aFrame.getContentPane()**...
JButton button = new JButton("Button 1 (PAGE_START)");
pane.add(button, BorderLayout.PAGE_START);

//Make the center component big, since that's the
//typical usage of BorderLayout.
button = new JButton("Button 2 (CENTER)");
button.setPreferredSize(new Dimension(200, 100));
pane.add(button, BorderLayout.CENTER);

button = new JButton("Button 3 (LINE_START)");
pane.add(button, BorderLayout.LINE_START);

button = new JButton("Long-Named Button 4 (PAGE_END)");
pane.add(button, BorderLayout.PAGE_END);

button = new JButton("5 (LINE_END)");
pane.add(button, BorderLayout.LINE_END);

```

Specify the component's location (for example, `BorderLayout.LINE_END`) as one of the arguments to the `add` method. If this component is missing from a container controlled by a `BorderLayout` object, make sure that the component's location was specified and no another component was placed in the same location.

All tutorial examples that use the `BorderLayout` class specify the component as the first argument to the `add` method. For example:

```

add(component, BorderLayout.CENTER)  //preferred

```

However, the code in other programs specifies the component as the second argument. For example, here are alternate ways of writing the preceding code:

```

add(BorderLayout.CENTER, component)  //valid but old fashioned
    **or**
add("Center", component)             //valid but error prone

```

## <a name="api" id="api">The BorderLayout API</a>

The following table lists constructors and methods to specify gaps (in pixels).
<th id="h1">Constructor or Method</th><th id="h2">Purpose</th>
<td headers="h1">[`BorderLayout(int **horizontalGap**, int **verticalGap**)`](https://docs.oracle.com/javase/8/docs/api/java/awt/BorderLayout.html#BorderLayout-int-int-)</td><td headers="h2">Defines a border layout with specified gaps between components.</td>
<td headers="h1">[`setHgap(int)`](https://docs.oracle.com/javase/8/docs/api/java/awt/BorderLayout.html#setHgap-int-)</td><td headers="h2">Sets the horizontal gap between components.</td>
<td headers="h1">[`setVgap(int)`](https://docs.oracle.com/javase/8/docs/api/java/awt/BorderLayout.html#setVgap-int-)</td><td headers="h2">Sets the vertical gap between components.</td>

## <a name="eg" id="eg">Examples that Use BorderLayout</a>

The following table lists code examples that use the `BorderLayout` class and provides links to related sections.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
<td headers="h101">[`BorderLayoutDemo`](../examples/layout/index.html#BorderLayoutDemo)</td><td headers="h102">This page</td><td headers="h103">Puts a component in each of the five possible locations.</td>
<td headers="h101">[`TabbedPaneDemo`](../examples/components/index.html#TabbedPaneDemo)</td><td headers="h102">[How to Use Tabbed Panes](../components/tabbedpane.html)</td><td headers="h103">One of many examples that puts a single component in the center of a content pane, so that the component is as large as possible.</td>
<td headers="h101">[`CheckBoxDemo`](../examples/components/index.html#CheckBoxDemo)</td><td headers="h102">[How to Use Check Boxes](../components/button.html#checkbox)</td><td headers="h103">Creates a `JPanel` object that uses the `BorderLayout` class. Puts components into the left (actually, `LINE_START`) and center locations.</td>
