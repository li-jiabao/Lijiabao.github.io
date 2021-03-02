
# A Visual Guide to Layout Managers

Several AWT and Swing classes provide layout managers for general use:

- [`BorderLayout`](#border)
- [`BoxLayout`](#box)
- [`CardLayout`](#card)
- [`FlowLayout`](#flow)
- [`GridBagLayout`](#gridbag)
- [`GridLayout`](#grid)
- [`GroupLayout`](#group)
- [`SpringLayout`](#spring)

This section shows example GUIs that use these layout managers, and tells you where to find the how-to page for each layout manager. 
You can find links for running the examples
in the how-to pages and in the
[example index](../examples/layout/index.html).


If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

<a name="border" id="border"></a>

## BorderLayout

Every content pane is initialized to use a `BorderLayout`. (As 
[Using Top-Level Containers](../components/toplevel.html) explains, the content pane is the main container in all frames, applets, and dialogs.) A `BorderLayout` places components in up to five areas: top, bottom, left, right, and center. All extra space is placed in the center area. Tool bars that are created using 
[JToolBar](../components/toolbar.html) must be created within a `BorderLayout` container, if you want to be able to drag and drop the bars away from their starting positions. 
For further details, see [How to Use BorderLayout](border.html).

## <a name="box" id="box">BoxLayout</a>

The `BoxLayout` class puts components in a single row or column. It respects the components' requested maximum sizes and also lets you align components. 
For further details, see [How to Use BoxLayout](box.html).

## <a name="card" id="card">CardLayout</a>


<img src="../../figures/uiswing/layout/CardLayoutDemo.png" width="265" height="105" alt="A picture of a GUI that uses CardLayout" /> 
<img src="../../figures/uiswing/layout/CardLayoutDemo-2.png" width="265" height="105" alt="Another picture of the same layout" />

The `CardLayout` class lets you implement an area that contains different components at different times. A `CardLayout` is often controlled by a combo box, with the state of the combo box determining which panel (group of components) the `CardLayout` displays. An alternative to using `CardLayout` is using a 
[tabbed pane](../components/tabbedpane.html), which provides similar functionality but with a pre-defined GUI. 
For further details, see [How to Use CardLayout](card.html).

## <a name="flow" id="flow">FlowLayout</a>

`FlowLayout` is the default layout manager for every `JPanel`. It simply lays out components in a single row, starting a new row if its container is not sufficiently wide. Both panels in CardLayoutDemo, shown [previously](#card), use `FlowLayout`. 
For further details, see [How to Use FlowLayout](flow.html).

## <a name="gridbag" id="gridbag">GridBagLayout</a>

`GridBagLayout` is a sophisticated, flexible layout manager. It aligns components by placing them within a grid of cells, allowing components to span more than one cell. The rows in the grid can have different heights, and grid columns can have different widths. 
For further details, see [How to Use GridBagLayout](gridbag.html).

## <a name="grid" id="grid">GridLayout</a>

`GridLayout` simply makes a bunch of components equal in size and displays them in the requested number of rows and columns. 
For further details, see [How to Use GridLayout](grid.html).

## <a name="group" id="group">GroupLayout</a>

`GroupLayout` is a layout manager that was developed for use by GUI builder tools, but it can also be used manually. `GroupLayout` works with the horizontal and vertical layouts separately. The layout is defined for each dimension independently. Consequently, however, each component needs to be defined twice in the layout. The Find window shown above is an example of a `GroupLayout`. For further details, see [How to Use GroupLayout](group.html).

## <a name="spring" id="spring">SpringLayout</a>

`SpringLayout` is a flexible layout manager designed for use by GUI builders. It lets you specify precise relationships between the edges of components under its control. For example, you might define that the left edge of one component is a certain distance (which can be dynamically calculated) from the right edge of a second component. `SpringLayout` lays out the children of its associated container according to a set of constraints, as shall be seen in [How to Use SpringLayout](spring.html).
