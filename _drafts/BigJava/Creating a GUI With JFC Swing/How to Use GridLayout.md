
# How to Use GridLayout

If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

The following figure represents a snapshot of an application that uses the 
[`GridLayout`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridLayout.html) class.

Click the Launch button to run GridLayoutDemo using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/layout/index.html#GridLayoutDemo).

The complete code of this demo is in the 
[`GridLayoutDemo.java`](../examples/layout/GridLayoutDemoProject/src/layout/GridLayoutDemo.java) file.

A `GridLayout` object places components in a grid of cells. Each component takes all the available space within its cell, and each cell is exactly the same size. If the `GridLayoutDemo` window is resized, the `GridLayout` object changes the cell size so that the cells are as large as possible, given the space available to the container.

The code snippet below creates the `GridLayout` object and the components it manages.

```


GridLayout experimentLayout = new GridLayout(0,2);

...

        compsToExperiment.setLayout(experimentLayout);

        compsToExperiment.add(new JButton("Button 1"));
        compsToExperiment.add(new JButton("Button 2"));
        compsToExperiment.add(new JButton("Button 3"));
        compsToExperiment.add(new JButton("Long-Named Button 4"));
        compsToExperiment.add(new JButton("5"));

```

The constructor of the `GridLayout` class creates an instance that has two columns and as many rows as necessary.

Use combo boxes to set up how much vertical or horizontal padding is put around the components. Then click the Apply gaps button. The following code snippet shows how your selection is processed by using the `setVgap` and `setHgap` methods of the `GridLayout` class:

```


applyButton.addActionListener(new ActionListener(){
            public void actionPerformed(ActionEvent e){
                //Get the horizontal gap value
                String horGap = (String)horGapComboBox.getSelectedItem();
                //Get the vertical gap value
                String verGap = (String)verGapComboBox.getSelectedItem();
                //Set up the horizontal gap value
                experimentLayout.setHgap(Integer.parseInt(horGap));
                //Set up the vertical gap value
                experimentLayout.setVgap(Integer.parseInt(verGap));
                //Set up the layout of the buttons
                experimentLayout.layoutContainer(compsToExperiment);
            }
        });


```

## <a name="api" id="api">The GridLayout API</a>

The following table lists constructors of the `GridLayout` class that specify the number of rows and columns.
<th id="h1">Constructor</th><th id="h2">Purpose</th>
<td headers="h1" width="30%">[`GridLayout(int **rows**, int **cols**)`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridLayout.html#GridLayout-int-int-)</td><td headers="h2">Creates a grid layout with the specified number of rows and columns. All components in the layout are given equal size. One, but not both, of `rows` and `cols` can be zero, which means that any number of objects can be placed in a row or in a column.</td>
<td headers="h1">[`GridLayout(int **rows**, int **cols**, int **hgap**, int **vgap**)`](https://docs.oracle.com/javase/8/docs/api/java/awt/GridLayout.html#GridLayout-int-int-int-int-)</td><td headers="h2">Creates a grid layout with the specified number of rows and columns. In addition, the horizontal and vertical gaps are set to the specified values. Horizontal gaps are places between each of columns. Vertical gaps are placed between each of the rows.</td>

The `GridLayout` class has two constructors:

## <a name="eg" id="eg">Examples that Use GridLayout</a>

The following table lists code examples that use the `GridLayout` class and provides links to related sections.
<th id="h101" align="left">Example</th><th id="h102" align="left">Where Described</th><th id="h103" align="left">Notes</th>
<td headers="h101" valign="top">[`GridLayoutDemo`](../examples/layout/index.html#GridLayoutDemo)</td><td headers="h102" valign="top">This page</td><td headers="h103" valign="top">Uses a 2-column grid.</td>
<td headers="h101" valign="top">[`ComboBoxDemo2`](../examples/components/index.html#ComboBoxDemo2)</td><td headers="h102" valign="top">[How to Use Combo Boxes](../components/combobox.html)</td><td headers="h103" valign="top">One of many examples that use a 1x1 grid to make a component as large as possible.</td>
<td headers="h101" valign="top">[`LabelDemo`](../examples/components/index.html#LabelDemo)</td><td headers="h102" valign="top">[How to Use Labels](../components/label.html)</td><td headers="h103" valign="top">Uses a 3-row grid.</td>
