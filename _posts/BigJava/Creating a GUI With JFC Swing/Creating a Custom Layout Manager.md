
# Creating a Custom Layout Manager

Before you start creating a custom layout manager, make sure that no existing layout manager meets your requirements. In particular, layout managers such as [`GridBagLayout`](gridbag.html), [`SpringLayout`](spring.html), and [`BoxLayout`](box.html) are flexible enough to work in many cases. You can also find layout managers from other sources, such as from the Internet. Finally, you can simplify layout by grouping components into containers such as 
[panels](../components/panel.html).

If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

To create a custom layout manager, you must create a class that implements the 
[`LayoutManager`](https://docs.oracle.com/javase/8/docs/api/java/awt/LayoutManager.html) interface. You can either implement it directly, or implement its subinterface, 
[`LayoutManager2`](https://docs.oracle.com/javase/8/docs/api/java/awt/LayoutManager2.html).

Every layout manager must implement at least the following five methods, which are required by the `LayoutManager` interface:

This method must take into account the container's internal borders, which are returned by the `getInsets` method. If appropriate, it should also take the container's orientation (returned by the 
[`getComponentOrientation`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html#getComponentOrientation--) method) into account. You cannot assume that the `preferredLayoutSize` or `minimumLayoutSize` methods will be called before `layoutContainer` is called.

Besides implementing the preceding five methods, layout managers generally implement at least one public constructor and the `toString` method.

If you wish to support component constraints, maximum sizes, or alignment, then your layout manager should implement the `LayoutManager2` interface. In fact, for these reasons among many others, nearly all modern layout managers will need to implement `LayoutManager2`. That interface adds five methods to those required by `LayoutManager`:

- `addLayoutComponent(Component, Object)`
- `getLayoutAlignmentX(Container)`
- `getLayoutAlignmentY(Container)`
- `invalidateLayout(Container)`
- `maximumLayoutSize(Container)`

Of these methods, the most important are `addLayoutComponent(Component, Object)` and `invalidateLayout(Container)`. The `addLayoutComponent` method is used to add components to the layout, using the specified constraint object. The `invalidateLayout` method is used to invalidate the layout, so that if the layout manager has cached information, this should be discarded. For more information about `LayoutManager2`, see the 
[`LayoutManager2` ](https://docs.oracle.com/javase/8/docs/api/java/awt/LayoutManager2.html) API documentation.

Finally, whenever you create custom layout managers, you should be careful of keeping references to `Component` instances that are no longer children of the `Container`. Namely, layout managers should override `removeLayoutComponent` to clear any cached state related to the `Component`.

## Example of a Custom Layout

The example `CustomLayoutDemo` uses a custom layout manager called `DiagonalLayout`. You can find the layout manager's source code in 
[`DiagonalLayout.java`](../examples/layout/CustomLayoutDemoProject/src/layout/DiagonalLayout.java). `DialogLayout` lays out components diagonally, from left to right, with one component per row. Here is a picture of CustomLayoutDemo using `DialogLayout` to lay out five buttons.

Click the Launch button to run `CustomLayoutDemo` using 
[Java&#8482; Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html) ([download JDK 7 or later](http://www.oracle.com/technetwork/java/javase/downloads/index.html)). Alternatively, to compile and run the example yourself, consult the [example index](../examples/layout/index.html#CustomLayoutDemo).
