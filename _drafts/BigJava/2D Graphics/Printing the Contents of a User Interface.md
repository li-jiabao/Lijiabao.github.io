
# Printing the Contents of a User Interface

Another common printing task is to print the contents of a window or a frame, either in whole, or in part. The window may contain the following components: toolbars, buttons sliders, text labels, scrollable text areas, images, and other graphical content. All of these components are printed using the following methods of the Java 2D printing API:

```

java.awt.Component.print(Graphics g);
java.awt.Component.printAll(Graphics g);

```

The following figure represents a simple user interface.

The code to create this UI is located in the sample program 
[`PrintUIWindow.java`](examples/PrintUIWindow.java).

To print this window, modify the code in the earlier examples which printed text or images. The resulting code should appear as follows:

```

public int print(Graphics g, PageFormat pf, int page)
    throws PrinterException {
    if (page &gt; 0) {
        return NO_SUCH_PAGE;
    }

    Graphics2D g2d = (Graphics2D)g;
    g2d.translate(pf.getImageableX(), pf.getImageableY());

    // Print the entire visible contents of a
    // java.awt.Frame.
    frame.printAll(g);

    return PAGE_EXISTS;
}

```

The `printAll(Graphics g)`method prints the component and all its subcomponents. This method is usually used to print object such as a complete window, rather than a single component.
