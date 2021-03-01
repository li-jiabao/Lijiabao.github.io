
# How to Use FlowLayout

If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

The 
[`FlowLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/FlowLayout.html) class provides a very simple layout manager that is used, by default, by the `JPanel` objects. The following figure represents a snapshot of an application that uses the flow layout:

Click the Launch button to run FlowLayoutDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/layout/index.html#FlowLayoutDemo).

The complete code of this demo is in the 
[`FlowLayoutDemo.java`](../examples/layout/FlowLayoutDemoProject/src/layout/FlowLayoutDemo.java) file.

The `FlowLayout` class puts components in a row, sized at their preferred size. If the horizontal space in the container is too small to put all the components in one row, the `FlowLayout` class uses multiple rows. If the container is wider than necessary for a row of components, the row is, by default, centered horizontally within the container. To specify that the row is to aligned either to the left or right, use a `FlowLayout` constructor that takes an alignment argument. Another constructor of the `FlowLayout` class specifies how much vertical or horizontal padding is put around the components.

The code snippet below creates a `FlowLayout` object and the components it manages.

```

FlowLayout experimentLayout = new FlowLayout();

...

    compsToExperiment.setLayout(experimentLayout);

    compsToExperiment.add(new JButton("Button 1"));
    compsToExperiment.add(new JButton("Button 2"));
    compsToExperiment.add(new JButton("Button 3"));
    compsToExperiment.add(new JButton("Long-Named Button 4"));
    compsToExperiment.add(new JButton("5"));

```

Select either the Left to Right or Right to Left option and click the Apply orientation button to set up the component's orientation. The following code snippet applies the Left to Right components orientation to the `experimentLayout`.

```

        compsToExperiment.setComponentOrientation(
                ComponentOrientation.LEFT_TO_RIGHT);

```

## <a name="api" id="api">The FlowLayout API</a>

The following table lists constructors of the `FlowLayout` class.
<th id="h1">Constructor</th><th id="h2">Purpose</th>
<td headers="h1">[`FlowLayout()`](https://docs.oracle.com/javase/8/docs/api/java/awt/FlowLayout.html#FlowLayout--)</td><td headers="h2">Constructs a new `FlowLayout` object with a centered alignment and horizontal and vertical gaps with the default size of 5 pixels.</td>
<td headers="h1">[`FlowLayout(int **align**)`](https://docs.oracle.com/javase/8/docs/api/java/awt/FlowLayout.html#FlowLayout-int-)</td><td headers="h2">Creates a new flow layout manager with the indicated alignment and horizontal and vertical gaps with the default size of 5 pixels. The alignment argument can be `FlowLayout.LEADING`, `FlowLayout.CENTER`, or `FlowLayout.TRAILING`. When the `FlowLayout` object controls a container with a left-to right component orientation (the default), the `LEADING` value specifies the components to be left-aligned and the `TRAILING` value specifies the components to be right-aligned.</td>
<td headers="h1">[`FlowLayout (int **align**, int **hgap**, int **vgap**)`](https://docs.oracle.com/javase/8/docs/api/java/awt/FlowLayout.html#FlowLayout-int-int-int-)</td><td headers="h2">Creates a new flow layout manager with the indicated alignment and the indicated horizontal and vertical gaps. The `hgap` and `vgap` arguments specify the number of pixels to put between components.</td>

## <a name="eg" id="eg">Examples that Use FlowLayout</a>

The following table lists code examples that use the `FlowLayout` class and provides links to related sections.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
<td headers="h101" valign="top">[`FlowLayoutDemo`](../examples/layout/index.html#FlowLayoutDemo)</td><td headers="h102" valign="top">This page</td><td headers="h103" valign="top">Sets up a content pane to use `FlowLayout`. If you set the `RIGHT_TO_LEFT` constant to `true` and recompile, you can see how `FlowLayout` handles a container that has a right-to-left component orientation.</td>
<td headers="h101" valign="top">[`CardLayoutDemo`](../examples/layout/index.html#CardLayoutDemo)</td><td headers="h102" valign="top">[How to Use CardLayout](card.html)</td><td headers="h103" valign="top">Centers a component nicely in the top part of a `BorderLayout`, and puts the component in a `JPanel` that uses a `FlowLayout`.</td>
<td headers="h101" valign="top">[`ButtonDemo`](../examples/components/index.html#ButtonDemo)</td><td headers="h102" valign="top">[How to Use Buttons, Check Boxes, and Radio Buttons](../components/button.html)</td><td headers="h103" valign="top">Uses the default `FlowLayout` of a `JPanel`.</td>
<td headers="h101" valign="top">[`TextInputDemo`](../examples/components/index.html#TextInputDemo)</td><td headers="h102" valign="top">[How to Use Formatted Text Fields](../components/formattedtextfield.html)</td><td headers="h103" valign="top">Uses a panel with a right-aligned `FlowLayout` presenting two buttons.</td>
