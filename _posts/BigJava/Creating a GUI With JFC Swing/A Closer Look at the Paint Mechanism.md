
# A Closer Look at the Paint Mechanism

By now you know that the `paintComponent` method is where all of your painting code should be placed. It is true that this method will be invoked when it is time to paint, but painting actually begins higher up the class heirarchy, with the `paint` method (defined by `java.awt.Component`.) This method will be executed by the painting subsystem whenever you component needs to be rendered. Its signature is:

- `public void paint(Graphics g)`

`javax.swing.JComponent` extends this class and further factors the `paint` method into three separate methods, which are invoked in the following order:

- `protected void paintComponent(Graphics g)`
- `protected void paintBorder(Graphics g)`
- `protected void paintChildren(Graphics g)`

The API does nothing to prevent your code from overriding `paintBorder` and `paintChildren`, but generally speaking, there is no reason for you to do so. For all practical purposes `paintComponent` will be the only method that you will ever need to override.

As previously mentioned, most of the standard Swing components have their look and feel implemented by separate UI Delegates. This means that most (or all) of the painting for the standard Swing components proceeds as follows.

1. `paint()` invokes `paintComponent()`.
1. If the `ui` property is non-null, `paintComponent()` invokes `ui.update()`.
1. If the component's `opaque` property is true, `ui.update()` fills the component's background with the background color and invokes `ui.paint()`.
1. `ui.paint()` renders the content of the component.

This is why our `SwingPaintDemo` code invokes `super.paintComponent(g)`. We could add an additional comment to make this more clear:

```

public void paintComponent(Graphics g) {<strong>
    // Let UI Delegate paint first, which 
    // includes background filling since 
    // this component is opaque.</strong>

    super.paintComponent(g);       
    g.drawString("This is my custom Panel!",10,20);
    redSquare.paintSquare(g);
}  

```

If you have understood all of the demo code provided in this lesson, congratulations! You have enough practical knowledge to write efficient painting code in your own applications. If however you want a closer look "under the hood", please refer to the SDN article linked to from the first page of this lesson.
