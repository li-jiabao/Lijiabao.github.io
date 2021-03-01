
# How to Use Panels

The 
[`JPanel`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPanel.html) class provides general-purpose containers for lightweight components. By default, panels do not add colors to anything except their own background; however, you can easily add borders to them and otherwise customize their painting. Details can be found in 
[Performing Custom Painting](../painting/index.html).

In many types of look and feel, panels are opaque by default. Opaque panels work well as content panes and can help with painting efficiently, as described in 
[Using Top-Level Containers](toplevel.html). You can change a panel's transparency by invoking the `setOpaque` method. A transparent panel draws no background, so that any components underneath show through.

## <a name="anexample" id="anexample">An Example</a>

The following picture shows a colored version of the `Converter` application, which is discussed in more detail in [Using Models](model.html).

The `Converter` example uses panels in several ways:

<li>One `JPanel` instance &#151; colored red in the preceding snapshot &#151; serves as a content pane for the application's frame. This content pane uses a top-to-bottom 
[`BoxLayout`](../layout/box.html) to lay out its contents, and an empty border to put 5 pixels of space around them. See [Using Top-Level Containers](toplevel.html) for information about content panes.</li>
- Two instances of a custom `JPanel` subclass named `ConversionPanel` &#151; colored cyan &#151; are used to contain components and coordinate communication between components. These `ConversionPanel` panels also have titled borders, which describe their contents and enclose the contents with a line. Each `ConversionPanel` panel uses a left-to-right `BoxLayout` object to lay out its contents.
- In each `ConversionPanel`, a `JPanel` instance &#151; colored magenta &#151; is used to ensure the proper size and position of the combo box. Each of these `JPanel` instances uses a top-to-bottom `BoxLayout` object (helped by an invisible space-filling component) to lay out the combo box.
- In each `ConversionPanel`, an instance of an unnamed `JPanel` subclass &#151; colored blue &#151; groups two components (a text field and a slider) and restricts their size. Each of these `JPanel` instances uses a top-to-bottom `BoxLayout` object to lay out its contents.

Here is what the `Converter` application normally looks like.

As the `Converter` example demonstrates, panels are useful for grouping components, simplifying component layout, and putting borders around groups of components. The rest of this section gives hints on grouping and laying out components. For information about using borders, see 
[How to Use Borders](../components/border.html).

## <a name="layout" id="layout">Setting the Layout Manager</a>

Like other containers, a panel uses a layout manager to position and size its components. By default, a panel's layout manager is an instance of 
[`FlowLayout`](../layout/flow.html), which places the panel's contents in a row. You can easily make a panel use any other layout manager by invoking the `setLayout` method or by specifying a layout manager when creating the panel. The latter approach is preferable for performance reasons, since it avoids the unnecessary creation of a `FlowLayout` object.

Here is an example of how to set the layout manager when creating the panel.

```

JPanel p = new JPanel(new BorderLayout()); //PREFERRED!

```

This approach does not work with `BoxLayout`, since the `BoxLayout` constructor requires a pre-existing container. Here is an example that uses `BoxLayout`.

```

JPanel p = new JPanel();
p.setLayout(new BoxLayout(p, BoxLayout.PAGE_AXIS));

```

## <a name="add" id="add">Adding Components</a>

When you add components to a panel, you use the `add` method. Exactly which arguments you specify to the `add` method depend on which layout manager the panel uses. When the layout manager is `FlowLayout`, `BoxLayout`, `GridLayout`, or `SpringLayout`, you will typically use the one-argument `add` method, like this:

```

aFlowPanel.add(aComponent);
aFlowPanel.add(anotherComponent);

```

When the layout manager is `BorderLayout`, you need to provide an argument specifying the added component's position within the panel. For example:

```

aBorderPanel.add(aComponent, BorderLayout.CENTER);
aBorderPanel.add(anotherComponent, BorderLayout.PAGE_END);

```

With `GridBagLayout` you can use either `add` method, but you must somehow specify 
[grid bag constraints](../layout/gridbag.html#gridbagConstraints) for each component.

For information about choosing and using the standard layout managers, see 
[Using Layout Managers](../layout/using.html).

## <a name="api" id="api">The Panel API</a>

The API in the `JPanel` class itself is minimal. The methods you are most likely to invoke on a `JPanel` object are those it inherits from its superclasses &#151; 
[`JComponent`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html), 
[`Container`](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html), and 
[`Component`](https://docs.oracle.com/javase/8/docs/api/java/awt/Component.html). The following tables list the API you are most likely to use, with the exception of methods related to 
[borders](../components/border.html) and [layout hints](jcomponent.html#layoutapi). For more information about the API that all `JComponent` objects can use, see [The JComponent Class](jcomponent.html).




- [Creating a `JPanel`](#creating)
- [Managing a Container's Components](#contents)
- [Setting or Getting the Layout Manager](#layoutapi)
<th id="h1" align="left"><a name="creating__1" id="creating__1">Constructor</a></th><th id="h2" align="left">Purpose</th>
<td headers="h1">[JPanel()](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPanel.html#JPanel--)<br />[JPanel(LayoutManager)](https://docs.oracle.com/javase/8/docs/api/javax/swing/JPanel.html#JPanel-java.awt.LayoutManager-)</td><td headers="h2">Creates a panel. The `LayoutManager` parameter provides a layout manager for the new panel. By default, a panel uses a `FlowLayout` to lay out its components.</td>
<th id="h101" align="left">Method</th><th id="h102" align="left">Purpose</th>
<td headers="h101">[void add(Component)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.awt.Component-)<br />[void add(Component, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.awt.Component-int-)<br />[void add(Component, Object)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.awt.Component-java.lang.Object-)<br />[void add(Component, Object, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.awt.Component-java.lang.Object-int-)<br />[void add(String, Component)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#add-java.lang.String-java.awt.Component-)</td><td headers="h102">Adds the specified component to the panel. When present, the `int` parameter is the index of the component within the container. By default, the first component added is at index 0, the second is at index 1, and so on. The `Object` parameter is layout manager dependent and typically provides information to the layout manager regarding positioning and other layout constraints for the added component. The `String` parameter is similar to the `Object` parameter.</td>
<td headers="h101">[int getComponentCount()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentCount--)</td><td headers="h102">Gets the number of components in this panel.</td>
<td headers="h101">[Component getComponent(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponent-int-)<br />[Component getComponentAt(int, int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentAt-int-int-)<br />[Component getComponentAt(Point)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponentAt-java.awt.Point-)<br />[Component[] getComponents()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getComponents--)</td><td headers="h102">Gets the specified component or components. You can get a component based on its index or **x, y** position.</td>
<td headers="h101">[void remove(Component)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#remove-java.awt.Component-)<br />[void remove(int)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#remove-int-)<br />[void removeAll()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#removeAll--)</td><td headers="h102">Removes the specified component(s).</td>
<th id="h201" align="left">Method</th><th id="h202" align="left">Purpose</th>
<td headers="h201">[void setLayout(LayoutManager)](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#setLayout-java.awt.LayoutManager-)<br />[LayoutManager getLayout()](https://docs.oracle.com/javase/8/docs/api/java/awt/Container.html#getLayout--)</td><td headers="h202">Sets or gets the layout manager for this panel. The layout manager is responsible for positioning the panel's components within the panel's bounds according to some philosophy.</td>

## <a name="eg" id="eg">Examples That Use Panels</a>

Many examples contained in this lesson use `JPanel` objects. The following table lists a few.
<th id="h301" align="left">Example</th><th id="h302" align="left">Where Described</th><th id="h303" align="left">Notes</th>
<td headers="h301">[`Converter`](../examples/components/index.html#Converter)</td><td headers="h302">This section</td><td headers="h303">Uses five panels, four of which use `BoxLayout` and one of which uses `GridLayout`. The panels use borders and, as necessary, size and alignment hints to affect layout.</td>
<td headers="h301">[`ListDemo`](../examples/components/index.html#ListDemo)</td><td headers="h302">[How to Use Lists](list.html)</td><td headers="h303">Uses a panel, with its default `FlowLayout` manager, to center three components in a row. </td>
<td headers="h301">[`ToolBarDemo`](../examples/components/index.html#ToolBarDemo)</td><td headers="h302">[How to Use Tool Bars](toolbar.html)</td><td headers="h303">Uses a panel as a content pane. The panel contains three components, laid out by `BorderLayout`.</td>
<td headers="h301">[`BorderDemo`](../examples/components/index.html#BorderDemo)</td><td headers="h302">[How to Use Borders](border.html)</td><td headers="h303">Contains many panels that have various kinds of borders. Several panels use `BoxLayout`.</td>
<td headers="h301">[`BoxLayoutDemo`](../examples/layout/index.html#BoxLayoutDemo)</td><td headers="h302">[How to Use BoxLayout](../layout/box.html)</td><td headers="h303">Illustrates the use of a panel with Swing's `BoxLayout` manager.</td>
