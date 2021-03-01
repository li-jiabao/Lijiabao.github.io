
# How to Use GridBagLayout

If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

Here is a picture of an example that uses 
[`GridBagLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridBagLayout.html).

Click the Launch button to run GridBagLayoutDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/layout/index.html#GridBagLayoutDemo).

The code for GridBagDemo is in 
[`GridBagLayoutDemo.java`](../examples/layout/GridBagLayoutDemoProject/src/layout/GridBagLayoutDemo.java).

`GridBagLayout` is one of the most flexible &#151; and complex &#151; layout managers the Java platform provides. A `GridBagLayout` places components in a grid of rows and columns, allowing specified components to span multiple rows or columns. Not all rows necessarily have the same height. Similarly, not all columns necessarily have the same width. Essentially, `GridBagLayout` places components in rectangles (cells) in a grid, and then uses the components' preferred sizes to determine how big the cells should be.

The following figure shows the grid for the preceding applet. As you can see, the grid has three rows and three columns. The button in the second row spans all the columns; the button in the third row spans the two right columns.

If you enlarge the window as shown in the following figure, you will notice that the bottom row, which contains Button 5, gets all the new vertical space. The new horizontal space is split evenly among all the columns. This resizing behavior is based on weights the program assigns to individual components in the `GridBagLayout`. You will also notice that each component takes up all the available horizontal space &#151; but not (as you can see with button 5) all the available vertical space. This behavior is also specified by the program.

The way the program specifies the size and position characteristics of its components is by specifying **constraints** for each component. The preferred approach to set constraints on a component is to use the `Container.add` variant, passing it a `GridBagConstraints` object, as demonstrated in the next sections.

The following sections explain the constraints you can set and provide examples.

## <a name="gridbagConstraints" id="gridbagConstraints">Specifying Constraints</a>

The following code is typical of what goes in a container that uses a 
[`GridBagLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridBagLayout.html). You will see a more detailed example in the next section.

```

JPanel pane = new JPanel(new GridBagLayout());
GridBagConstraints c = new GridBagConstraints();

**//For each component to be added to this container:**
**//...Create the component...**
**//...Set instance variables in the GridBagConstraints instance...**
pane.add(theComponent, c);

```

As you might have guessed from the above example, it is possible to reuse the same `GridBagConstraints` instance for multiple components, even if the components have different constraints. However, it is recommended that you do not reuse `GridBagConstraints`, as this can very easily lead to you introducing subtle bugs if you forget to reset the fields for each new instance.

You can set the following 
[`GridBagConstraints`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridBagConstraints.html) instance variables:

**Note:** `GridBagLayout` does not allow components to span multiple rows unless the component is in the leftmost column or you have specified positive `gridx` and `gridy` values for the component.

Here is a picture of how these values are interpreted in a container that has the default, left-to-right component orientation. <!--[PENDING: A real figure will go here.
     It will put a hint of a button border around each constant,
     so that it's obvious that the component is hugging the 
     specified part of its display area.]-->
|FIRST_LINE_START|PAGE_START<td align="right">FIRST_LINE_END</td>
|LINE_START<td align="center">CENTER</td><td align="right">LINE_END</td>
|LAST_LINE_START|PAGE_END<td align="right">LAST_LINE_END</td>

Unless you specify at least one non-zero value for `weightx` or `weighty`, all the components clump together in the center of their container. This is because when the weight is 0.0 (the default), the `GridBagLayout` puts any extra space between its grid of cells and the edges of the container.

Generally weights are specified with 0.0 and 1.0 as the extremes: the numbers in between are used as necessary. Larger numbers indicate that the component's row or column should get more space. For each column, the weight is related to the highest `weightx` specified for a component within that column, with each multicolumn component's weight being split somehow between the columns the component is in. Similarly, each row's weight is related to the highest `weighty` specified for a component within that row. Extra space tends to go toward the rightmost column and bottom row.

The next section discusses constraints in depth, in the context of explaining how the example program works.

## <a name="gridbagExample" id="gridbagExample">The Example Explained</a>

Here, again, is a picture of the GridBagLayoutDemo application.

Click the Launch button to run GridBagLayoutDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/layout/index.html#GridBagLayoutDemo).

The following code creates the `GridBagLayout` and the components it manages. You can find the entire source file in 
[`GridBagLayoutDemo.java`](../examples/layout/GridBagLayoutDemoProject/src/layout/GridBagLayoutDemo.java).

```

JButton button;
pane.setLayout(new GridBagLayout());
GridBagConstraints c = new GridBagConstraints();
if (shouldFill) {
                //natural height, maximum width
                c.fill = GridBagConstraints.HORIZONTAL;
}

button = new JButton("Button 1");
if (shouldWeightX) {
                   c.weightx = 0.5;
}
c.fill = GridBagConstraints.HORIZONTAL;
c.gridx = 0;
c.gridy = 0;
pane.add(button, c);

button = new JButton("Button 2");
c.fill = GridBagConstraints.HORIZONTAL;
c.weightx = 0.5;
c.gridx = 1;
c.gridy = 0;
pane.add(button, c);

button = new JButton("Button 3");
c.fill = GridBagConstraints.HORIZONTAL;
c.weightx = 0.5;
c.gridx = 2;
c.gridy = 0;
pane.add(button, c);

button = new JButton("Long-Named Button 4");
c.fill = GridBagConstraints.HORIZONTAL;
c.ipady = 40;      //make this component tall
c.weightx = 0.0;
c.gridwidth = 3;
c.gridx = 0;
c.gridy = 1;
pane.add(button, c);

button = new JButton("5");
c.fill = GridBagConstraints.HORIZONTAL;
c.ipady = 0;       //reset to default
c.weighty = 1.0;   //request any extra vertical space
c.anchor = GridBagConstraints.PAGE_END; //bottom of space
c.insets = new Insets(10,0,0,0);  //top padding
c.gridx = 1;       //aligned with button 2
c.gridwidth = 2;   //2 columns wide
c.gridy = 2;       //third row
pane.add(button, c);

```

This example uses one `GridBagConstraints` instance for all the components the `GridBagLayout` manages, however in real-life situations it is recommended that you do not reuse `GridBagConstraints`, as this can very easily lead to you introducing subtle bugs if you forget to reset the fields for each new instance. Just before each component is added to the container, the code sets (or resets to default values) the appropriate instance variables in the `GridBagConstraints` object. It then adds the component to its container, specifying the `GridBagConstraints` object as the second argument to the `add` method.

For example, to make button 4 be extra tall, the example has this code:

```

c.ipady = 40;

```

And before setting the constraints of the next component, the code resets the value of `ipady` to the default:

```

c.ipady = 0;

```

If a component's display area is larger than the component itself, then you can specify whereabouts in the display area the component will be displayed by using the `GridBagConstraints.anchor` constraint. The `anchor` constraint's values can be absolute (north, south, east, west, and so on), or orientation-relative (at start of page, at end of line, at the start of the first line, and so on), or relative to the component's baseline. For a full list of the possible values of the `anchor` constraint, including baseline-relative values,see the API documentation for 
[`GridBagConstraints.anchor`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridBagConstraints.html#anchor). You can see in the code extract above that Button 5 specifies that it should be displayed at the end of the display area by setting an anchor at `GridBagConstraints.PAGE_END`.

```

GridBagLayout gridbag = new GridBagLayout();
pane.setLayout(gridbag);
...
gridbag.setConstraints(button, c);
pane.add(button);

```

Here is a table that shows all the constraints for each component in GridBagLayoutDemo's content pane. Values that are not the default are marked in **boldface**. Values that are different from those in the previous table entry are marked in *italics*.
<th id="h101" align="left">Component</th><th id="h102" align="left">Constraints</th>
<td headers="h101">All components</td><td headers="h102"><pre>`ipadx = 0**fill = GridBagConstraints.HORIZONTAL**`</pre></td>
<td headers="h101">Button 1</td><td headers="h102"><pre>`ipady = 0 **weightx = 0.5** weighty = 0.0 gridwidth = 1 anchor = GridBagConstraints.CENTER insets = new Insets(0,0,0,0) **gridx = 0** **gridy = 0**`</pre></td>
<td headers="h101">Button 2</td><td headers="h102"><pre>`**weightx = 0.5*****gridx = 1*****gridy = 0**`</pre></td>
<td headers="h101">Button 3</td><td headers="h102"><pre>`**weightx = 0.5*****gridx = 2*****gridy = 0**`</pre></td>
<td headers="h101">Button 4</td><td headers="h102"><pre>`***ipady = 40****weightx = 0.0****gridwidth = 3******gridx = 0******gridy = 1***`</pre></td>
<td headers="h101">Button 5</td><td headers="h102"><pre>`*ipady = 0*weightx = 0.0***weighty = 1.0******anchor = GridBagConstraints.PAGE_END******insets = new Insets(10,0,0,0)******gridwidth = 2******gridx = 1******gridy = 2***`</pre></td>

GridBagLayoutDemo has two components that span multiple columns (buttons 4 and 5). To make button 4 tall, we added internal padding (`ipady`) to it. To put space between buttons 4 and 5, we used insets to add a minimum of 10 pixels above button 5, and we made button 5 hug the bottom edge of its cell.

All the components in the `pane` container are as wide as possible, given the cells that they occupy. The program accomplishes this by setting the `GridBagConstraints` `fill` instance variable to `GridBagConstraints.HORIZONTAL`, leaving it at that setting for all the components. If the program did not specify the fill, the buttons would be at their natural width, like this:

When you enlarge GridBagLayoutDemo's window, the columns grow proportionately. This is because each component in the first row, where each component is one column wide, has `weightx = 0.5`. The actual value of these components' `weightx` is unimportant. What matters is that all the components, and consequently, all the columns, have an equal weight that is greater than 0. If no component managed by the `GridBagLayout` had `weightx` set, then when the components' container was made wider, the components would stay clumped together in the center of the container, like this:

If the container is given a size that is smaller or bigger than the prefered size, then any space is distributed according to the `GridBagContainer` weights.

Note that if you enlarge the window, the last row is the only one that gets taller. This is because only button 5 has `weighty` greater than zero.

## <a name="api" id="api">The GridBagLayout API</a>

The `GridBagLayout` and `GridBagConstraints` classes each have only one constructor, with no arguments. Instead of invoking methods on a `GridBagConstraints` object, you manipulate its instance variables, as described in [Specifying Constraints](#gridbagConstraints). Generally, the only method you invoke on a `GridBagLayout` object is `setConstraints`, as demonstrated in [The Example Explained](#gridbagExample).

## <a name="eg" id="eg">Examples that Use GridBagLayout</a>

You can find examples of using `GridBagLayout` throughout this tutorial. The following table lists a few.
<th id="h201" align="left">Example</th><th id="h202" align="left">Where Described</th><th id="h203" align="left">Notes</th>
<td headers="h201" valign="top">[`GridBagLayoutDemo`](../examples/layout/index.html#GridBagLayoutDemo)</td><td headers="h202" valign="top">This section</td><td headers="h203" valign="top">Uses many features &#151; weights, insets, internal padding, horizontal fill, exact cell positioning, multi-column cells, and anchoring (component positioning within a cell).</td>
<td headers="h201" valign="top">[`TextSamplerDemo`](../components/../examples/components/index.html#TextSamplerDemo)</td><td headers="h202" valign="top">[Using Text Components](../components/text.html)</td><td headers="h203" valign="top">Aligns two pairs of labels and text fields, plus adds a label across the full width of the container.</td>
<td headers="h201" valign="top">[`ContainerEventDemo`](../events/../examples/events/index.html#ContainerEventDemo)</td><td headers="h202" valign="top">[How to Write a Container Listener](../events/containerlistener.html)</td><td headers="h203" valign="top">Positions five components within a container, using weights, fill, and relative positioning.</td>
