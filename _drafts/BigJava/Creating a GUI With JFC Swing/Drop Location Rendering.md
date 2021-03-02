
# Drop Location Rendering

This is a more advanced topic and most people do not need to worry about it. However, if you have a custom component you will need to handle the drop location rendering yourself.

You can register to be notified whenever the `dropLocation` property changes. You would listen for this change and do your own rendering of the drop location in a custom renderer for the component or in the `paintComponent` method, using the `getDropLocation` method.

Here is an example of listening for the `dropLocation` property:

```

class Repainter extends PropertyChangeListener {
    public void propertyChange(PropertyChangeEvent pce) {
        repaintDropLocation(pce.getOldValue());
        repaintDropLocation(pce.getNewValue());
    }
}

comp.addPropertyChangeListener("dropLocation", newRepainter());

```

Here is an example of the `paintComponent` approach:

```

public void paintComponent(Graphics g) {
    super.paintComponent(g);

    DropLocation loc= getDropLocation();
    if (loc == null) {
        return;
    }

    renderPrettyIndicatorAt(loc);
}

```
