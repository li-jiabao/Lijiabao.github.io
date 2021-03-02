
# Solving Common Layout Problems

If you are interested in using JavaFX to create your GUI, see
[Working With Layouts in JavaFX](https://docs.oracle.com/javase/8/javafx/layout-tutorial/index.html).

**Problem:** How do I specify a component's exact size?

- A handful of the more modern layout managers provide ways to override the size set by the component. Check whether the layout manager you are using allows you to specify component sizes.
- Make sure that you really need to set the component's exact size. Each Swing component has a different preferred size, depending on the font it uses and the look and feel. For this reason, it often does not make sense to specify a Swing component's exact size.
- If the component is not controlled by a layout manager, you can set its size by invoking the `setSize` or `setBounds` method on it. Otherwise, you need to provide size hints and then make sure you are using a layout manager that respects the size hints.
- If you extend a Swing component class, you can give size hints by overriding the component's `getMinimumSize`, `getPreferredSize`, and `getMaximumSize` methods. What is nice about this approach is that each `get**Xxxx**Size` method can get the component's default size hints by invoking `super.get**Xxxx**Size()`. Then it can adjust the size, if necessary, before returning it. This is particularly handy for text components, where you might want to fix the width, but have the height determined from the content. However, sometimes problems can be encountered with `GridBagLayout` and text fields, wherein if the size of the container is smaller than the preferred size, the minimum size gets used, which can cause text fields to shrink quite substantially.
- Another way to give size hints is to invoke the component's `setMinimumSize`, `setPreferredSize`, and `setMaximumSize` methods.
- If you specify new size hints for a component that is already visible, you then need to invoke the `revalidate` method on it, to make sure that its containment hierarchy is laid out again. Then invoke the `repaint` method.

**Problem:** My component does not appear after I have added it to the container.

- You need to invoke `revalidate` and `repaint` after adding a component before it will show up in your container.

**Problem:** My custom component is being sized too small.

- Does the component implement the `getPreferredSize` and `getMinimumSize` methods? If so, do they return the right values?
- Are you using a layout manager that can use as much space as is available? See [Tips on Choosing a Layout Manager](using.html#choosing) for some tips on choosing a layout manager and specifying that it use the maximum available space for a particular component.

If you do not see your problem in this list, see 
[Solving Common Component Problems](../components/problems.html).
