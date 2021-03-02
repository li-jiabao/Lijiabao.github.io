
# How Layout Management Works




If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

Here is an example of a layout management sequence for a container using 
[`LayoutManager2`](https://docs.oracle.com/javase/8/docs/api/java/awt/LayoutManager2.html).

<li>Layout managers basically do two things:
<ul>
1. Calculate the minimum/preferred/maximum sizes for a container.
1. Lay out the container's children.
</ul>
Layout managers do this based on the provided constraints, the container's properties (such as insets) and on the children's minimum/preferred/maximum sizes. If a child is itself a container then its own layout manger is used to get its minimum/preferred/maximum sizes and to lay it out.
</li>
<li>
<p>A container can be *valid* (namely, `isValid()` returns true) or *invalid*. For a container to be valid, all the container's children must be laid out already and must all be valid also. The 
[`Container.validate`](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#validate--) method can be used to validate an invalid container. This method triggers the layout for the container and all the child containers down the component hierarchy and marks this container as valid.</p>
</li>
<li>
<p>After a component is created it is in the invalid state by default. The 
[`Window.pack`](https://docs.oracle.com/javase/8/docs/api/java/awt/Window.html) method validates the window and lays out the window's component hierarchy for the first time.</p>
</li>

The end result is that to determine the best size for the container, the system determines the sizes of the containers at the bottom of the containment hierarchy. These sizes then percolate up the containment hierarchy, eventually determining the container's total size.

If the size of a component changes, for example following a change of font, the component must be resized and repainted by calling the `revalidate` and `repaint` methods on that component. Both `revalidate` and `repaint` are 
[thread-safe](../concurrency/index.html) &#151; you need not invoke them from the event-dispatching thread.

When you invoke `revalidate` on a component, a request is passed up the containment hierarchy until it encounters a container, such as a scroll pane or top-level container, that should not be affected by the component's resizing. (This is determined by calling the container's `isValidateRoot` method.) The container is then laid out, which has the effect of adjusting the revalidated component's size and the size of all affected components.
