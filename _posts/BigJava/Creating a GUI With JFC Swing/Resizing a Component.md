
# Resizing a Component

Have you ever needed a smaller version of a component to place on a tool palette or tool bar, or in a status bar? You can resize a component by setting a client property on the component. Three sizes are supported in addition to the "regular" size: mini, small, and large.

The one component that does not support the size variants property is `JLabel`. However, you can change the size of a label by changing the size of its font.

Other look and feel implementations, such as Apple's Aqua, might also honor the size variants client property. Nimbus is currently the only Sun look and feel that supports size variants.

You can set the size of a component with one line of code, before the component is displayed. The following snippet shows how to use each size:

```

// mini
myButton.putClientProperty("JComponent.sizeVariant", "mini");
// small
mySlider.putClientProperty("JComponent.sizeVariant", "small");
// large
myTextField.putClientProperty("JComponent.sizeVariant", "large");

```

If you have set the size variants property correctly but the component displays in its "regular" size, you might need to force an update to the UI. You can do so by invoking the 
[`SwingUtilities.updateComponentTreeUI(Component)`](https://docs.oracle.com/javase/8/docs/api/javax/swing/SwingUtilities.html#updateComponentTreeUI-java.awt.Component-) method before the window is displayed. The following code snippet updates the window and all the components it contains:

```

JFrame frame = ...;

**SwingUtilities.updateComponentTreeUI(frame);**

frame.pack();
frame.setVisible(true);

```
