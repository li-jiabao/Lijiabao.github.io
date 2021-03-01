
# How to Use GroupLayout

`GroupLayout` is a layout manager that was developed for GUI builders such as Matisse, the GUI builder provided with the NetBeans IDE. Although the layout manager was originally designed to suit the GUI builder needs, it also works well for manual coding. This discussion will teach you how `GroupLayout` works and show you how you can use `GroupLayout` to build GUIs, whether you choose to use a GUI builder like Matisse or write your own code.

If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

## Design Principle: Independent Dimensions

`GroupLayout` works with the horizontal and vertical layouts separately. The layout is defined for each dimension independently. You do not need to worry about the *vertical* dimension when defining the *horizontal* layout, and vice versa, as the layout along each axis is totally independent of the layout along the other axis.

When focusing on just one dimension, you only have to solve half the problem at one time. This is easier than handling both dimensions at once. This means, of course, that each component needs to be defined twice in the layout. If you forget to do this, `GroupLayout` will generate an exception.

## Layout Organization: Hierarchical Groups

`GroupLayout` uses two types of arrangements -- sequential and parallel, combined with hierarchical composition.

1. With **sequential** arrangement, the components are simply placed one after another, just like `BoxLayout` or `FlowLayout` would do along one axis. The position of each component is defined as being relative to the preceding component.
1. The second way places the components in **parallel**&#8212;on top of each other in the same space. They can be baseline-, top-, or bottom-aligned along the vertical axis. Along the horizontal axis, they can be left-, right-, or center-aligned if the components are not all the same size.

Usually, components placed in parallel in one dimension are in a sequence in the other, so that they do not overlap.

What makes these two arrangements powerful is that they can be nested hierarchically. For this purpose `GroupLayout` defines **layout groups**. A group is either sequential or parallel and may contain components, other groups, and gaps (discussed below).

The size of a sequential group is the sum of the sizes of the contained elements, and the size of a parallel group corresponds to the size of the largest element (although, depending on the elements and where the baseline lands, the size of a baseline-aligned group may be a bit larger than the largest element).

Defining a layout means defining how the components should be grouped by combining the sequential and parallel arrangements.

Let us use a simple example to see how it works in practice.

## An Example

Let us start with something simple, just three components in a row:

We will express this layout using groups. Starting with the horizontal axis it is easy to see there is a *sequential group* of 3 components arranged from left to right. Along the vertical axis there is a *parallel group* of the same 3 components with the same location, size, and baseline:

In pseudo code, the layout specification might look like this (the real code is in the *Writing Code* section below):

```

horizontal layout = sequential group { c1, c2, c3 }
vertical layout = parallel group (BASELINE) { c1, c2, c3 }

```

This illustrates a principle mentioned earlier: components grouped sequentially in one dimension usually form a parallel group in the other dimension.

Now let us add one more component, C4, left-aligned with C3:

Along the horizontal axis the new component occupies the same horizontal space as C3 so that it forms a parallel group with C3. Along the vertical axis C4 forms a sequential group with the original parallel group of the three components.

In pseudo code, the layout specification now looks like this:

```

horizontal layout = sequential group { c1, c2, **parallel group (LEFT) { **c3,** c4 }** }
vertical layout = **sequential group {** parallel group (BASELINE) { c1, c2, c3 }, **c4 }**

```

Now you understand the most important aspects of designing layouts with `GroupLayout`. There are just a few more details to explain: how to add gaps, how to define size and resize behavior, how to define justified layout, and how to write real code.

## Gaps

A gap can be thought of as an invisible component of a certain size. Gaps of arbitrary size can be added to groups just like components or other groups. Using gaps you can precisely control the distance between components or from the container border.

`GroupLayout` also defines *automatic* gaps that correspond to *preferred distances* between neighboring components (or between a component and container border). The size of such a gap is computed dynamically based on the look and feel the application is using (the `LayoutStyle` class is used for this). There are two advantages to using automatic (preferred) gaps: you do not have to specify the pixel sizes of the gaps, and they automatically adjust to the look and feel the UI runs with, reflecting the actual look and feel guidelines.

`GroupLayout` distinguishes between (a) the preferred gap between two components and (b) the preferred gap between a component and the container border. There are corresponding methods in the `GroupLayout` API for adding these gaps (`addPreferredGap` and `addContainerGap`). There are three types of component gaps: **related**, **unrelated** and **indented**. The `LayoutStyle.ComponentPlacement` enum defines corresponding constants to be used as parameters of the `addPreferredGap` method: `RELATED`, `UNRELATED` and `INDENT`. The difference between related and unrelated gaps is just in size&#8212;the distance between unrelated components is a bit bigger. *Indented* represents a preferred horizontal distance of two components when one of them is positioned underneath the second with an indent.

As mentioned above, `GroupLayout` can insert gaps automatically&#8212;if you do not add your own gaps explicitly, it adds the *related* preferred gaps for you. This is not the default behavior, however. You have to turn this feature on by invoking `setAutoCreateGaps(true)` and `setAutoCreateContainerGaps(true)` on the `GroupLayout`. Then you will get correct spacing automatically.

## Writing Code

Now, let us take a look at the actual code to create the layout described above.

Let us assume we have a container named `panel` and the same four components already presented (`c1`, `c2`, `c3`, and `c4`). First, we create a new `GroupLayout` object and associate it with the panel:

```

GroupLayout layout = new GroupLayout(panel);
 panel.setLayout(layout);

```

We specify automatic gap insertion:

```

layout.setAutoCreateGaps(true);
layout.setAutoCreateContainerGaps(true);

```

Then, we define the groups and add the components. We establish a root group for each dimension using the `setHorizontalGroup` and `setVerticalGroup` methods. Groups are created via `createSequentialGroup` and `createParallelGroup` methods. Components are added to groups by using the `addComponent` method.

```

layout.setHorizontalGroup(
   layout.createSequentialGroup()
      .addComponent(c1)
      .addComponent(c2)
      .addGroup(layout.createParallelGroup(GroupLayout.Alignment.LEADING)
           .addComponent(c3)
           .addComponent(c4))
);
layout.setVerticalGroup(
   layout.createSequentialGroup()
      .addGroup(layout.createParallelGroup(GroupLayout.Alignment.BASELINE)
           .addComponent(c1)
           .addComponent(c2)
           .addComponent(c3))
      .addComponent(c4)
);

```

You can specify the alignment for parallel groups. It can be one of the following constants defined in the `GroupLayout.Alignment` enum: `LEADING`, `TRAILING`, `CENTER`, and `BASELINE`. These constants are used for both dimensions and depend on whether the component orientation is left-to-right or right-to-left (top-to-bottom or bottom-to-top). For example, if the horizontal (vertical) component orientation is left-to-right (top-to-bottom) `LEADING` means left (top) while `TRAILING` means right (bottom). `CENTER` means "centered" in both dimensions. If you do not specify the alignment, `LEADING` will be used. The `BASELINE` alignment is valid only in the vertical dimension.

Alignment in the layout of a group only has meaning for components of different sizes. Components of the same size will be automatically aligned for each of the `GroupLayout.Alignment` constants.

Some comments about the code:

- You do not need to add the component directly to the container&#8212;that is done for you implicitly when using one of the addComponent methods.
- Note the chained calls of the `addComponent` methods used to fill the groups. The `addComponent` method always returns the group on which it is called. Thanks to this you do not need to use local variables to hold the groups.
- It is a good idea to indent the code so it is easy to see the hierarchical structure of the groups. Give each component a new line, add one level of indent for each new group in the hierarchy. A good source editor will help you with pairing the parenthesis to close the `createXXXGroup` methods. By following these simple rules, it is easier to add a new component or remove an existing one.

## Component Size and Resizability

There is no limit on the number of resizable components in a layout.

The size of each component in a `GroupLayout` is constrained by three values; minimum size, preferred size and maximum size. These sizes control how the component resizes within the layout. The `GroupLayout.addComponent(...)` method allows the size constraints to be specified.

If not specified explicitly, the layout asks the component for its default sizes (by using the component's `getMinimumSize()`, `getPreferredSize()` and `getMaximumSize()` methods). You do not need to specify anything for most of the components, like making `JTextField` resizable or `JButton` fixed, because the components themselves have the desired resizing behavior as default. On the other hand you can override the default behavior. For example you can make a `JTextField` fixed or `JButton` resizable.

`GroupLayout` defines constants that provide precise control over resize behavior. They can be used as parameters in the `addComponent(Component comp, int min, int pref, int max)`&#160; method. Here are two examples:

<li>To force a component to be resizable (allow shrinking and growing):
<pre><code>
*group*.addComponent(component, 0, GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE) ...
</code></pre>
This allows the component to resize between zero size (minimum) to any size (`Short.MAX_VALUE` as maximum size means "infinite"). If we wanted the component not to shrink below its default minimum size, we would use `GroupLayout.DEFAULT_SIZE` instead of `0` in the second parameter.
</li>
<li>To make a component fixed size (suppress resizing):
<pre><code>
group.addComponent(component, GroupLayout.PREFERRED_SIZE, GroupLayout.DEFAULT_SIZE,
          GroupLayout.PREFERRED_SIZE) ...
</code></pre>
</li>

In these examples the initial size of the component is not altered, its default size is the component's preferred size. If we wanted a specific size for the component, we would specify it in the second parameter instead of using `GroupLayout.DEFAULT_SIZE`.

Resizable gaps

Specifying size and resizability applies to gaps as well, including the preferred ones. For example, you can specify a preferred gap between two components that acts like a *spring* pushing the components away from each other (to the opposite sides of the container). The preferred distance of the two components is only used as the minimum size of the gap. See the following snippet:

```

layout.createSequentialGroup()
    .addComponent(c1)
    .addPreferredGap(LayoutStyle.ComponentPlacement.RELATED,
                     GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
    .addComponent(c2);

```

## Sizing in Parallel Groups

Resizable elements placed in a parallel group are stretched to fill the space of the group determined by the largest element in the group, so they end up aligned with the same size. `GroupLayout` also provides control over whether the enclosing parallel group itself should resize. If group resizing is suppressed, it prevents the contained elements from growing over the preferred size of the group. This way you can make a block of components align on both sides, or constrain individual components to have the same size.

Let us try to achieve the same size for two components from our example (`c3` and `c4` in the horizontal dimension):

```

layout.createParallelGroup(GroupLayout.Alignment.LEADING, false)
  .addComponent(c3, GroupLayout.DEFAULT_SIZE, GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
  .addComponent(c4, GroupLayout.DEFAULT_SIZE, GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE);

```

The underlying mechanism works as follows:

1. The size of the parallel group is set to the preferred size of the largest element; so to the preferred size of `c4` in our example.
1. Resizable elements are stretched to the size of the group. In our example, only `c3` is effectively stretched, the size of `c4` already corresponds to the size of the group.

As a result, `c3` and `c4` would have the same width. The components would not resize further because the parallel group itself is not resizable (the second parameter of the `createParallelGroup` method, above, is `false`).

Question for attentive readers: Why do we define both components in the parallel group as resizable in this example? It seems enough to have just `c3` resizable since `c4` is not stretched anyway...

The answer is: because of platform and localization independence. Otherwise we would have to rely on that `c4` component always being bigger than `c3`. But this may change when the application runs on different platform or is translated to another language. By having both components resizeable they adjust to each other, no matter which one is bigger at a given moment.

## Making Components the Same Size

The previous case is special because the components are in the same parallel group. But what if we wanted unrelated components to have the same size? Clearly, the same size cannot always be ensured by grouping. The OK and Cancel buttons in a row at the bottom of a dialog are a good example. For this purpose `GroupLayout` provides a `linkSize` method. This method allows the size of arbitrary components to be linked regardless of where they are placed. The resulting size of the linked components is set according to the largest component. For example:

```

layout.linkSize(SwingConstants.HORIZONTAL, c3, c4);

```

In this example, the size is linked selectively for the horizontal dimension.

## Runtime Changes to Your GUI

There are two important methods that you can use to make changes to your GUI at runtime, `replace()` and `setHonorsVisibility()`. Using these two methods, you can exchange components or change the visibility of components at runtime and have the GUI rearrange itself accordingly.

`replace(Component existingComponent, Component newComponent)` replaces an existing component with a new one. One of the common operations needed for dynamic layouts is the ability to replace components like this. For example, perhaps a check box toggles between a component displaying a graph or a tree. `GroupLayout` makes this scenario simple with the `replace()` method. You can swap components without recreating all the groups.

Another common operation in user interfaces is to dynamically change the visibility of components. Perhaps components are shown only as a user completes earlier portions of a form. To avoid components shuffling around in such a scenario, space should be taken up regardless of the visibility of the components. `GroupLayout` offers two ways to configure how invisible components are treated. The `setHonorsVisibility(boolean)` method globally sets how invisible components are handled. A value of true, the default, indicates invisible components are treated as if they are not there. On the other hand, a value of false provides space for invisible components, treating them as though they were visible. The `setHonorsVisibility(Component,Boolean)` method can be used to configure the behavior at the level of a specific component. To determine how visibility is handled, `GroupLayout` first checks if a value has been specified for the Component, if not, it checks the setting of the global property.

`GroupLayout` in the Java Standard Edition 6 consists of three distinct bodies of work: the ability to get the baseline for a component, the ability to get the preferred gap between components (`LayoutStyle`), and `GroupLayout`. This work was originally done as an open source project at 
[http://java.net/projects/swing-layout/](http://java.net/projects/swing-layout/)

NetBeans 5.0 supports `GroupLayout` by way of the swing-layout project. Because of the success of this work, all three portions have been rolled into `GroupLayout` in Java Standard Edition version 6. The main difference between the `GroupLayout` in Java SE 6 and swing-layout is in the package name and method names. NetBeans 5.5 provides the ability to target either the `GroupLayout` in Java SE 6, or the `GroupLayout` in swing-layout. Which version NetBeans targets is determined by the version of the Java platform your project targets. A project targeting Java SE 6 uses the `GroupLayout` in Java SE, otherwise `GroupLayout` in swing-layout is used.
